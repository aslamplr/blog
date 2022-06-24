+++
title = "Classify video using deep learning!"
date = 2019-09-05T11:13:14.000Z
draft = true
thumbnail = ""
category = "blog"
+++
## What is Video Resnet?

Video Resnet is a 3D CNN architecture uses residual learning (skip connections). This network perform 3D convolutions over the spatiotemporal video volume. 

2D Resnet has shown huge improvements in tasks such as image classification. But video classification, especially action recognition in video is less popular when compared to image classification. 

There are several architectures and research papers interested in video classification, or specifically action classification. Some of the great advances over the years in video classification are deeply rooted in advancement in image classification. 

Availability of a large scale dataset for video classification is also one reason the earlier research were focused on image classification techniques to achieve results in video classification. Most of the research utilize available small scale action recognition datasets. Earlier techniques explored the possibility of using 2D CNNs trained on ImageNet.

<!-- more -->

Early works ---

**"Large-scale Video Classification with Convolutional Neural Networks"** _2014_ - Andrej Karparthy • George Toderici • Sanketh Shetty • Thomas Leung • Rahul Suthankar • Li Fei-Fei (Google Research & Computer Science Department, Stanford University) [pdf](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/42455.pdf)

https://cs.stanford.edu/people/karpathy/deepvideo/

* This paper introduces `Sports-1M` Dataset. --- An academic dataset which contains 1,133,158 video URLs which have been annotated automatically with 487 labels. The annotation was done via YouTube Topics API. 
* The paper uses CNN (2D) to classify video using a technique called "Slow fusion" to convolve over the temporal information. And a Multiresolution CNN with a Fovea stream and context stream. 

**"Two-Stream Convolutional Networks for Action Recognition in Videos"** _2014_ Karen Simonyan • Andrew Zisserman (Visual Geometry Group, University of Oxford) [pdf](https://papers.nips.cc/paper/5353-two-stream-convolutional-networks-for-action-recognition-in-videos.pdf) \[NIPS]

* This paper introduces optical flow temporal stream input for action recognition.
* Two ConvNet are used in the model proposed in this paper.
* First ConvNet takes a single RGB frame randomly sampled from the video for Spacial stream. The ConvNet is pretrained in ImageNet.
* Second ConvNet takes dense optical flow input. Passed sequentially to the ConvNet. 
* Finally the outputs from both streams are fed to a _class score fusion_ function. The paper implements an SVM to do this.

**"Beyond Short Snippets: Deep Networks for Video Classification"** _2015_ Joe Yue-Hei Ng • Matthew Hausknecht • Sudheendra Vijayanarasimhan • Oriol Vinyals • Rajat Monga • George Toderici (University of Maryland, College Park • University of Texas at Austin • Google, Inc.) [pdf](https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Ng_Beyond_Short_Snippets_2015_CVPR_paper.pdf) \[CVPR]

* This paper explores an approach using a ConvNet(AlexNet or GoogLeNet). The ConvNet output an average pooled 1000 dimentional vector. The 1000 dimentional vector is fed to an LSTM network. The deep LSTM network, in which the output from one LSTM layer is input for the next layer. Uses 5 such stacked layers each with 512 memory cells. The following LSTM layers, a Softmax classifier makes a prediction at every frame.
* Data augmentation --- Multiple random crops per video, by randomly selecting the position of the first frame and consistent random crops for each frame during  both training and testing.
* Use of optical flow inputs as suggested in _"Two-Stream Convolutional Networks for Action Recognition in Videos"_ by K. Simonyan and A. Zisserman.

**"Quo Vadis, Action Recognition? A New Model and the Kinetics Dataset"** _Feb 2018_ Joao Carreira • Andrew Zisserman (DeepMind • Depratment of Engineering Science, University of Oxford) [pdf](https://arxiv.org/pdf/1705.07750.pdf) [**SOTA**]

* Introduces Kinetics-700 dataset
* Inflated 3D Convolution

The focus of this blog post is mainly on the following paper ---

**"A Closer Look at Spatiotemporal Convolutions for Action Recognition"** _Apr 2018_ Du Tran • Heng Wang • Lorenzo Torresani • Jamie Ray • Yann LeCun • Manohar Paluri (Facebook Research • Dartmouth College) [pdf](https://arxiv.org/pdf/1711.11248.pdf)

* Introduces Residual learning framework into video classification.
* Introduces R(2+1)D CNN function which is comparable or superior to I3D(State of the art) function.

I will be discussing about the R(2+1)D in the following part of this post.
An 18 layer deep R(2+1)D (VideoResnet) network as in https://arxiv.org/abs/1711.11248

- - -

> **Disclaimer**: In most of this post I am using the paper author's own words to describe what is what. This is done because, I felt my words won't be just to describe what is what. And my purpose of writing this post is to put it all together for myself and others in a way that is easier for me to understand. And probably some of you might find it interesting as well. 

There are 5 residual network architectures being discused in this paper.

1. **`R2D`** (2D Resnet)
2. **`MCx`** (Resnet with mixed convolutions 3D convolutions followed by 2D convolution layers)
3. **`rMCx`** (Resnet with mixed convolutions reversed, where 2D convolutions followed by 3D convolution layers)
4. **`R3D`** (3D Resnet)
5. **`R(2+1)D`** (ResNet with (2+1)D convolutions)

_The architecture:_

![r2plus1d.png](/img/video-resnet/r2plus1d.png)
R(2+1)D are ResNets with (2+1)D convolutions. The residual connections are not shown in the image.

## (2+1)D vs 3D convolution

![2plus1dConv.png](/img/video-resnet/2plus1dConv.png)
Consider a video sample with `t` frames (the temporal extend) and `d x d` width and height (the spacial width and height) and a single input channel for simplicity.
(a) Full 3D convolution is carried out using a filter of size `t x d x d`.
(b) A (2+1)D convolution block splits the computation into a spacial 2D convolution followed by a temporal 1D convolution. The spacial convolutions are done using a 3D convolution function with kernel size `1 x d x d` and number of filters being `Mi`. Which is the input size to the temporal convolution function. Which is again a 3D convolution function with kernel size `t x 1 x 1`.

## Why are (2+1)D convolutions better than 3D?

Following figure shows the training and testing errors on Kinetics for R3D and R(2+1)D, using 18-layers (left) and 34-layers (right).

![train_test_r2plus1d_vs_r3d.png](/img/video-resnet/train_test_r2plus1d_vs_r3d.png)
**Train and test error - R(2+1)D vs. R3D**
ResNets with 18 layers (left) and 34 layers (right). The observation is that the training error is smaller for the R(2+1)D network compared to the R3D network. This is particularly true in case of network with more depth (34 layer network on the right). 

Based on this observation the paper suggests that _"**the spatial-temporal decomposition implemented by the R(2+1)D eases the optimization, especially as the depth is increased**"._ 

- - -

## Code

Original source: https://pytorch.org/docs/stable/_modules/torchvision/models/video/resnet.html (slightly modified in this post to reduce unwanted complexity).

> The code part is being worked on, I will update the blog post soon with the code part explained. For the time being following code is working and is tested. 

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
    def __init__(
        self, 
        inplanes, 
        planes, 
        conv_builder, 
        stride=1, 
        downsample=None
    ):
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
    def __init__(
        self,
        in_planes,
        out_planes,
        midplanes,
        stride=1,
        padding=1
    ):
        super(Conv2Plus1D, self).__init__(
            nn.Conv3d(in_planes, midplanes, 
                kernel_size=(1, 3, 3),
                stride=(1, stride, stride), 
                padding=(0, padding, padding),
                bias=False
            ),
            nn.BatchNorm3d(midplanes),
            nn.ReLU(inplace=True),
            nn.Conv3d(midplanes, out_planes, 
                kernel_size=(3, 1, 1),
                stride=(stride, 1, 1),
                padding=(padding, 0, 0),
                bias=False
            )
        )
```

```python
class R2Plus1dStem(nn.Sequential):
    def __init__(self):
        super(R2Plus1dStem, self).__init__(
            nn.Conv3d(3, 45, 
                    kernel_size=(1, 7, 7),
                    stride=(1, 2, 2), 
                    padding=(0, 3, 3),
                    bias=False
            ),
            nn.BatchNorm3d(45),
            nn.ReLU(inplace=True),
            nn.Conv3d(45, 64,
                    kernel_size=(3, 1, 1),
                    stride=(1, 1, 1), padding=(1, 0, 0),
                    bias=False
            ),
            nn.BatchNorm3d(64),
            nn.ReLU(inplace=True)
        )    
```

```python
class R2Plus1D18(nn.Module):
    def __init__(self, num_classes=400):
        """
        R(2+1)D VideoResNet
        Args:
            num_classes (int, optional): Dimension of the final FC layer. Defaults to 400.
        """
        super(R2Plus1D18, self).__init__()
        self.stem = R2Plus1dStem()

        self.layer1 = nn.Sequential(
            BasicBlock(64, 64, Conv2Plus1D),
            BasicBlock(64, 64, Conv2Plus1D)
        )

        self.layer2 = nn.Sequential(
            BasicBlock(64, 128, Conv2Plus1D, 
                stride=2, 
                downsample=self._get_downsample(64, 128)
            ),
            BasicBlock(128, 128, Conv2Plus1D)
        )

        self.layer3 = nn.Sequential(
            BasicBlock(128, 256, Conv2Plus1D, 
                stride=2, 
                downsample=self._get_downsample(128, 256)
            ),
            BasicBlock(256, 256, Conv2Plus1D)
        )

        self.layer4 = nn.Sequential(
            BasicBlock(256, 512, Conv2Plus1D, 
                stride=2, 
                downsample=self._get_downsample(256, 512)
            ),
            BasicBlock(512, 512, Conv2Plus1D)
        )

        self.avgpool = nn.AdaptiveAvgPool3d((1, 1, 1))
        self.fc = nn.Linear(512, num_classes)

        # init weights
        self._initialize_weights()

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

    def _get_downsample(self, inplane, outplane):
        return nn.Sequential(
            nn.Conv3d(
                inplane, outplane, 
                kernel_size=1, 
                stride=(2, 2, 2), 
                bias=False
            ),
            nn.BatchNorm3d(outplane)
        )

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
    model = R2Plus1D18(**kwargs)

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

Let's run it!

```python
X = torch.randn(1, 3, 8, 112, 112)

model, X = model.to(device), X.to(device)

y_ = model(X)
```

```python
torch.argmax(torch.softmax(y_, dim=1), dim=1)
```

Output: `tensor([316], device='cuda:0')`
