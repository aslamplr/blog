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

To be honest I didn't like a bit about this setup. All contrived and complicated seems not straight forward. Lot of dependencies like having to install both `Xcode` and `Android Studio` and in addition to that `Cocoapods` which again requires ruby gem! ☹️ Huge disappointment so far!

Let me see!

After setting up the following as suggested by the `flutter doctor` output:

- Xcode (xcode-select switch, xcodebuild install)
- vscode (Install flutter extension)

_Note: vscode was complaining that it couldn't find Flutter SDK, so moved flutter SDK to home and add path extension to the `~/.profile`_


Run `flutter-doctor` after all that from `vscode`. Still complaining about the missing Android Toolchain and Android Studio. 

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-1.20.36-pm.png", width=600, height=500, op="fit_width") }}

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-1.27.49-pm.png", width=600, height=500, op="fit_width") }}

**Setting up Android SDK**

I cannot ignore installing Android SDK (remember the market share of Android in India is huge, so any app you are developing to appeal entire population should have an Android app in the play store is a must). But, I do not want to install Android Studio. Let me see what options do I have? After a google search and couple of clicks I ended up in this page where there are several options including one with "Command line tools only" bliss! [https://developer.android.com/studio#downloads](https://developer.android.com/studio#downloads)

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-1.30.45-pm.png", width=600, height=500, op="fit_width") }}

Let's download it, what a bliss it's only 81.7 MB!

Extracted the archive after download, moved everything to my $HOME and added the bin directory to my $PATH. And run `sdkmanager` boom!

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-1.39.36-pm.png", width=600, height=500, op="fit_width") }}

I forgot AndroidSDK requires `JDK` - The Java Development Kit. I decided to give it a hold for now! It's been around 2 hours after I started to setup Flutter. Including all the downloads and installation etc.

Let me come back to you later! Flutter!

I have decided to download Android Studio rather than setting up JDK. Since I have uninstalled JDK sometimes back, I no longer remember when, as it no longer comes with commercial usage license for public. 

Downloading the Android Studio its around 805 MB download for Mac. Which is a lighter download considering the fact that Xcode is 8.1 GB. It comes everything bundled Android SDK + JDK etc (This what I thought initially). 

Let's open it! It requires to download the Android SDK again and the virtual device which is another 1 GB+. I am OK with it atleast because, you don't need to set it up manually. The installer takes care of it. Which is a huge relief at this point in time.

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-4.32.21-pm.png", width=600, height=500, op="fit_width") }}

Its stuck downloading "https://dl.google.com/android/repository/sys-img/google_apis/x86-29_r10.zip" 

Let's see what's the size of the resource. `curl -I https://dl.google.com/android/repository/sys-img/google_apis/x86-29_r10.zip`

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-4.37.23-pm.png", width=600, height=500, op="fit_width") }}

Oh! it's another 1+ GB. ☹️

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-4.43.21-pm.png", width=600, height=500, op="fit_width") }}

Bam! It asked for admin privileges!

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-4.45.44-pm.png", width=600, height=500, op="fit_width") }}

Huh! 

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-4.51.26-pm.png", width=600, height=500, op="fit_width") }}

The installation finished at last!

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-4.51.41-pm.png", width=600, height=500, op="fit_width") }}

The Android Studio welcome screen! 

Run `flutter doctor` again and see if all good and green!
{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-4.53.25-pm.png", width=600, height=500, op="fit_width") }}

Bam! No!

```
...
[!] Android toolchain - develop for Android devices (Android SDK version 29.0.3)
    ✗ Android licenses not accepted.  To resolve this, run: flutter doctor
      --android-licenses
...
[!] Android Studio (version 3.6)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
...
```

These seems simple enough complaints, lets resolve these!

`flutter doctor --android-licenses` Oh! I am going crazy again `Accept (y/N):` or die.

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-5.06.05-pm.png", width=600, height=500, op="fit_width") }}

And install the Flutter plugin (+ Dart plugin) for Android Studio

Run `flutter doctor` again, see where we are –

{{ resize_image(path="img/uploads/screenshot-2020-05-26-at-6.20.12-pm.png", width=600, height=500, op="fit_width") }}

Seems like the doctor is happy for the time being!

---

### Let's write some customery "Hello, World!"

Create a flutter project `flutter create hello_flutter`. 

{{ resize_image(path="img/uploads/flutter_create_hello_flutter.png", width=600, height=500, op="fit_width") }}

{{ resize_image(path="img/uploads/flutter_project_created.png", width=600, height=500, op="fit_width") }}

Change directory to the created project directory `cd hello_flutter`.

Let's run the automatically created flutter project `flutter run`. 

```
No supported devices connected.
```

Yes! you need to connect a device in order to install the application and run it. It can be a virtual device or an actual device connected.

For running iOS you need to run `Simulator` and for Android `emulator`.

This is when I realised none of the Android SDK command line tools are in my $PATH. This is how my $PATH extensions in `~/.profile` looks like –

{{ resize_image(path="img/uploads/flutter_path_after_flutter.png", width=600, height=500, op="fit_width") }}

- Flutter commands are already in $PATH.
- Android SDK CLI tools.
- Add JDK bundled with Android Studio into JAVA_HOME (it is required by the Android SDK CLI tools).
- Android Emulator as well.

Now let's check the CLI tool `avdmanager` to see all created images (By default it comes with a Nexus image).

{{ resize_image(path="img/uploads/flutter_avdmanager_list_devices.png", width=600, height=500, op="fit_width") }}

Let's see if this avd is getting picked up by emulator.

{{ resize_image(path="img/uploads/flutter_emulator_list_avds.png", width=600, height=500, op="fit_width") }}

Start the Android emulator for the Nexus device.

{{ resize_image(path="img/uploads/flutter_emulator_start_avd.png", width=600, height=500, op="fit_width") }}

Wow!

{{ resize_image(path="img/uploads/flutter_emulator_screen1.png", width=200, height=600, op="fit_width") }}

{{ resize_image(path="img/uploads/flutter_emulator_screen2.png", width=200, height=600, op="fit_width") }}

{{ resize_image(path="img/uploads/flutter_emulator_screen3.png", width=200, height=600, op="fit_width") }}



---