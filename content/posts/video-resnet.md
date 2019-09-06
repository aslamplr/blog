---
title: "Video Resnet"
date: 2019-09-05T16:43:14+05:30
draft: true
---

## What is Video Resnet?

__TODO: Write this__

18 layer deep R(2+1)D (VideoResnet) network as in https://arxiv.org/abs/1711.11248

<!--more-->

> **Disclaimer**: In most of this post I am using the paper author's own words to describe what is what. This is done becuase, I felt my words won't be just to describe what is what. And my purpose of writing this post is to put it all together for myself and others in a way that is easier for me to understand. And probably some of you might find it interesting as well. 


*Abstract:*

> In this paper we discuss several forms of spatiotemporal convolutions for video analysis and study their effects on action recognition. Our motivation stems from the observation that 2D CNNs applied to individual frames of the video have remained solid performers in action recognition. In this work we empirically demonstrate the accuracy advantages of 3D CNNs over 2D CNNs within the framework of residual learning. Furthermore, we show that factorizing the 3D convolutional filters into separate spatial and temporal components yields significantly gains in accuracy. Our empirical study leads to the design of a new spatiotemporal convolutional block “R(2+1)D” which produces CNNs that achieve results comparable or superior to the state-of-theart on Sports-1M, Kinetics, UCF101, and HMDB51.


There are 5 residual network architectures being discused in this paper.

1. **`R2D`** (2D Resnet)
2. **`MCx`** (Resnet with mixed convolutions 3D convolutions followed by 2D convolution layers)
3. **`rMCx`** (Resnet with mixed convolutions reversed, where 2D convolutions followed by 3D convolution layers)
4. **`R3D`** (3D Resnet)
5. **`R(2+1)D`** (ResNet with (2+1)D convolutions)

*The architecture:*

![r2plus1d.png](/img/video-resnet/r2plus1d.png)

R(2+1)D are ResNets with (2+1)D convolutions. For interpretablility, residual connections are omitted in the above image.

## (2+1)D vs 3D convolution 

![2plus1dConv.png](/img/video-resnet/2plus1dConv.png)
Consider a video sample with `t` frames (the temporal extend) and `d x d` width and height (the spacial width and height) and a single input channel for simplicity.
(a) Full 3D convolution is carried out using a filter of size `t x d x d`.
(b) A (2+1)D convolution block splits the computation into a spacial 2D convolution followed by a temporal 1D convolution. The spacial convolutions are done using a 3D convolution function with kernel size `1 x d x d` and number of filters being `Mi`. Which is the input size to the temporal convolution function. Which is again a 3D convolution function with kernel size `t x 1 x 1`.

## Why are (2+1)D convolutions better than 3D?

Following figure shows the training and testing errors on Kinetics for R3D and R(2+1)D, using 18-layers (left) and 34-layers (right).

![train_test_r2plus1d_vs_r3d.png](/img/video-resnet/train_test_r2plus1d_vs_r3d.png)
**Training and testing errors for R(2+1)D and R3D.**
Results are reported for ResNets of 18 layers (left) and 34 layers (right). It can be observed that the training error (thin lines) is smaller for R(2+1)D compared to R3D, particularly for the network with larger depth (right). This suggests that the the spatialtemporal decomposition implemented by R(2+1)D eases the optimization, especially as depth is increased.


R(2+1)D gives lower testing error than R3D but the interesting message in this plot is that R(2+1)D yields also lower training error. The reduction in training error for R(2+1)D compared to R3D is particularly accentuated for the architecture having 34 layers. This suggests that the spatiotemporal decomposition of R(2+1)D renders the optimization easier compared to R3D, especially as depth is increased.


### Code

```python
import torch
import torch.nn as nn

from torchvision.models.utils import load_state_dict_from_url

```


```python
MODEL_URL = 'https://download.pytorch.org/models/r2plus1d_18-91a641e6.pth'
```


```python
class BasicBlock(nn.Module):

    expansion = 1

    def __init__(self, inplanes, planes, conv_builder, stride=1, downsample=None):
        midplanes = (inplanes * planes * 3 * 3 * 3) // (inplanes * 3 * 3 + 3 * planes)

        super(BasicBlock, self).__init__()
        self.conv1 = nn.Sequential(
            conv_builder(inplanes, planes, midplanes, stride),
            nn.BatchNorm3d(planes),
            nn.ReLU(inplace=True)
        )
        self.conv2 = nn.Sequential(
            conv_builder(planes, planes, midplanes),
            nn.BatchNorm3d(planes)
        )
        self.relu = nn.ReLU(inplace=True)
        self.downsample = downsample
        self.stride = stride

    def forward(self, x):
        residual = x

        out = self.conv1(x)
        out = self.conv2(out)
        if self.downsample is not None:
            residual = self.downsample(x)

        out += residual
        out = self.relu(out)

        return out
```


```python
class Conv2Plus1D(nn.Sequential):

    def __init__(self,
                 in_planes,
                 out_planes,
                 midplanes,
                 stride=1,
                 padding=1):
        super(Conv2Plus1D, self).__init__(
            nn.Conv3d(in_planes, midplanes, kernel_size=(1, 3, 3),
                      stride=(1, stride, stride), padding=(0, padding, padding),
                      bias=False),
            nn.BatchNorm3d(midplanes),
            nn.ReLU(inplace=True),
            nn.Conv3d(midplanes, out_planes, kernel_size=(3, 1, 1),
                      stride=(stride, 1, 1), padding=(padding, 0, 0),
                      bias=False))

    @staticmethod
    def get_downsample_stride(stride):
        return (stride, stride, stride)


```


```python
class R2Plus1dStem(nn.Sequential):
    """R(2+1)D stem is different than the default one as it uses separated 3D convolution
    """
    def __init__(self):
        super(R2Plus1dStem, self).__init__(
            nn.Conv3d(3, 45, kernel_size=(1, 7, 7),
                      stride=(1, 2, 2), padding=(0, 3, 3),
                      bias=False),
            nn.BatchNorm3d(45),
            nn.ReLU(inplace=True),
            nn.Conv3d(45, 64, kernel_size=(3, 1, 1),
                      stride=(1, 1, 1), padding=(1, 0, 0),
                      bias=False),
            nn.BatchNorm3d(64),
            nn.ReLU(inplace=True))
    
```


```python
class VideoResNet(nn.Module):

    def __init__(self, block, conv_makers, layers,
                 stem, num_classes=400,
                 zero_init_residual=False):
        """Generic resnet video generator.

        Args:
            block (nn.Module): resnet building block
            conv_makers (list(functions)): generator function for each layer
            layers (List[int]): number of blocks per layer
            stem (nn.Module, optional): Resnet stem, if None, defaults to conv-bn-relu. Defaults to None.
            num_classes (int, optional): Dimension of the final FC layer. Defaults to 400.
            zero_init_residual (bool, optional): Zero init bottleneck residual BN. Defaults to False.
        """
        super(VideoResNet, self).__init__()
        self.inplanes = 64

        self.stem = stem()

        self.layer1 = self._make_layer(block, conv_makers[0], 64, layers[0], stride=1)
        self.layer2 = self._make_layer(block, conv_makers[1], 128, layers[1], stride=2)
        self.layer3 = self._make_layer(block, conv_makers[2], 256, layers[2], stride=2)
        self.layer4 = self._make_layer(block, conv_makers[3], 512, layers[3], stride=2)

        self.avgpool = nn.AdaptiveAvgPool3d((1, 1, 1))
        self.fc = nn.Linear(512 * block.expansion, num_classes)

        # init weights
        self._initialize_weights()

        if zero_init_residual:
            for m in self.modules():
                if isinstance(m, Bottleneck):
                    nn.init.constant_(m.bn3.weight, 0)

    def forward(self, x):
        x = self.stem(x)

        x = self.layer1(x)
        x = self.layer2(x)
        x = self.layer3(x)
        x = self.layer4(x)

        x = self.avgpool(x)
        # Flatten the layer to fc
        x = x.flatten(1)
        x = self.fc(x)

        return x

    def _make_layer(self, block, conv_builder, planes, blocks, stride=1):
        downsample = None

        if stride != 1 or self.inplanes != planes * block.expansion:
            ds_stride = conv_builder.get_downsample_stride(stride)
            downsample = nn.Sequential(
                nn.Conv3d(self.inplanes, planes * block.expansion,
                          kernel_size=1, stride=ds_stride, bias=False),
                nn.BatchNorm3d(planes * block.expansion)
            )
        layers = []
        layers.append(block(self.inplanes, planes, conv_builder, stride, downsample))

        self.inplanes = planes * block.expansion
        for i in range(1, blocks):
            layers.append(block(self.inplanes, planes, conv_builder))

        return nn.Sequential(*layers)

    def _initialize_weights(self):
        for m in self.modules():
            if isinstance(m, nn.Conv3d):
                nn.init.kaiming_normal_(m.weight, mode='fan_out',
                                        nonlinearity='relu')
                if m.bias is not None:
                    nn.init.constant_(m.bias, 0)
            elif isinstance(m, nn.BatchNorm3d):
                nn.init.constant_(m.weight, 1)
                nn.init.constant_(m.bias, 0)
            elif isinstance(m, nn.Linear):
                nn.init.normal_(m.weight, 0, 0.01)
                nn.init.constant_(m.bias, 0)


```


```python
def r2plus1d_18(pretrained=False, progress=True, **kwargs):
    
    model = VideoResNet(
        block=BasicBlock,
        conv_makers=[Conv2Plus1D] * 4,
        layers=[2, 2, 2, 2],
        stem=R2Plus1dStem, 
        **kwargs
    )

    if pretrained:
        state_dict = load_state_dict_from_url(MODEL_URL,
                                              progress=progress)
        model.load_state_dict(state_dict)
    return model
```


```python
model = r2plus1d_18(pretrained=True, progress=True)
```


```python
# Detect devices
use_cuda = torch.cuda.is_available()
device = torch.device("cuda" if use_cuda else "cpu")
```


```python
X = torch.randn(1, 3, 8, 112, 112)

model, X = model.to(device), X.to(device)

y_ = model(X)

```


```python
torch.argmax(torch.softmax(y_, dim=1), dim=1)
```




    tensor([316], device='cuda:0')




```python

```
