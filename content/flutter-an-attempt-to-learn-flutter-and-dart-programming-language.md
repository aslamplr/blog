+++
title = "Flutter: An attempt to learn Flutter and Dart programming language!"
date = 2020-05-24T17:04:55.770Z
draft = true
category = "blog"
+++
**Flutter** is an UI rendering engine and framework works for building simple and fast mobile applications both for Android and iOS. A hybrid approach like ionic or React Native but this is far better since it compiles everything down to native code which renders on the canvas infrastructure provided by the platform.

**Dart** is programming language for working with Flutter. Dart is more UI centric programming language (is what the official site claims it to be with hot reloading capabilities and all). 

### Why I am interested in Flutter & Dart lately?

My Frontend application development activity is limited to web. I have Javascript/Typescript with React/Vue in my tool belt for any web frontend development need. I have never seriously considered filling the empty space for mobile application development. 

And now I have a requirement to write a small mobile application. And when I checked I have several options to go develop the application. But I want to take it as an oppurtunity to add a language/framework/tool to my toolbelt so that I have other oppurtunites opened up in near future as this is more interesting times for serious app development. 

I have never written a mobile application (except for some hobby projects while working with Tensorflow lite). I have never felt natural at writing in the Android SDK (Java + XML etc.). And never attempted at iOS (Swift or Objective C). 

And talking to my friend who had recently been aquinted with Flutter + Dart was talking really good things about it. And made me think seriously about aquiring this skillset. But I am not going to invest much time if this not coming natural to me and if the toolchain is tedious to setup which I hate these days. That is another reason I love Rust compared to my attempt learning Go. When I was trying to learn Go the toolchain and the setup and the way how it all was defined felt quite not natural to me. Rust came in really handy and natural. Especially because I come from the ease of having an easy setup like getting used to the `npm`.

Enough of history. Let's see where to get started with Flutter. Let me head to https://flutter.dev and see what is the way forward to get this setup.

I am planning to use my MacBook Air for the setup. And I am planning to use `vscode` as my IDE for the development.

I don’t want to bother myself to download Xcode or Android Studio at this point in time.

Update: Quick change of mind, I decide to download Xcode for the sake of it. It's downloading 8.1 GB (☹️ Not good in my opinion!)

{{ resize_image(path="img/uploads/screenshot-2020-05-25-at-10.50.13-pm.png", width=600, height=400, op="fit_width") }}

{{ resize_image(path="img/uploads/screenshot-2020-05-25-at-10.50.39-pm.png", width=600, height=400, op="fit_width") }}

_`Note: Xcode is necessary to do iOS development using Flutter!`_

Meanwhile I headed straight to the https://flutter.dev and downloading the `flutter_macos_1.17.1-stable.zip` which is around 1.08 GB (☹️ Not impressed again)

{{ resize_image(path="img/uploads/screenshot-2020-05-25-at-10.47.07-pm.png", width=600, height=400, op="fit_width") }}

So its all getting downloaded and I am heading to vscode to checkout the flutter extension. 

{{ resize_image(path="img/uploads/screenshot-2020-05-25-at-10.49.29-pm.png", width=600, height=400 op="fit_width") }}

---

### Setting up the Flutter SDK

After the download, unzipped the `flutter_macos_1.17.1-stable.zip` to the `~/Downloads/` itself for now! Yet to graduate to my `$HOME`!

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-12.38.25-pm.png", width=600, height=500, op="fit_width") }}

Time to run `flutter doctor` see what dependencies are missing!

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-12.46.15-pm.png", width=600, height=500, op="fit_width") }}

---