# A Guide to RN Native package development.

First you have to read this

[Native Guide For Android](https://reactnative.dev/docs/native-modules-android)

[Native Guide For iOS](https://reactnative.dev/docs/native-modules-ios)

## What we need ?

For Android, Android Studio is required. For iOS, you need Xcode and CocoaPods.

I suggest installing CocoaPods by Homebrew. `brew install cocoapods`.

PS: remember add `~/Library/Android/sdk/platform-tools/` to your bin path.


# Android Part

## About Gradle
Android use gradle to manage deployment and workflows. Here are some basic conecpts you have to know.

```
In most case,  there are at least two build.gradle file in your project.
https://stackoverflow.com/questions/28295933/difference-between-build-gradleproject-and-build-gradlemodule

Top-level build file where you can add configuration options common to all sub-projects/modules.

All modules have a specific gradle file itself. Whatever is included in this gradle file, it will only affect the module that is included on.
```

### repositories
Unlike npm in js, there are multiple package source.

Declare all repo source in build.gradle in `repositories`

```
repositories {
  google()
  jcenter()
  mavenCentral()
  maven { url "https://jitpack.io" }
  maven {
    // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
    url "$rootDir/../node_modules/react-native/android"
  }
}

```

### dependencies

declare dependencies you need in `dependencies`. Then click gradle button to sync in Android Studio.

```
dependencies {
  def googlePlayServicesVisionVersion = safeExtGet('googlePlayServicesVisionVersion', safeExtGet('googlePlayServicesVersion', '17.0.2'))

  //noinspection GradleDynamicVersion
  implementation 'com.facebook.react:react-native:+'  // From node_modules
  implementation "com.google.zxing:core:3.3.3"
  implementation "com.drewnoakes:metadata-extractor:2.11.0"
  generalImplementation "com.google.android.gms:play-services-vision:$googlePlayServicesVisionVersion"
  implementation "androidx.exifinterface:exifinterface:1.0.0"
  implementation "androidx.annotation:annotation:1.0.0"
  implementation "androidx.legacy:legacy-support-v4:1.0.0"
  mlkitImplementation "com.google.firebase:firebase-ml-vision:${safeExtGet('firebase-ml-vision', '19.0.3')}"
  mlkitImplementation "com.google.firebase:firebase-ml-vision-face-model:${safeExtGet('firebase-ml-vision-face-model', '17.0.2')}"
}
```

## Build variants: Source set

It is very useful to set mutilple source sets for different build targets.

You can set current source set in `Build variants` panel on the left bottom in Android Studio.

### Define source sets

Define source sets in build.gradle.

For more details, check [my RN camera](https://github.com/lumixraku/react-native-camera/blob/master/android/build.gradle)
```
  sourceSets {
    main {
      java.srcDirs = ['src/main/java']
    }
    general {
      java.srcDirs = ['src/general/java']
    }
    mlkit {
      java.srcDirs = ['src/mlkit/java']
    }
  }
```


## Passing Values From Native to JS.

Use `com.facebook.react.bridge.WritableArray` and `com.facebook.react.bridge.WritableMap` to serialize data.

# iOS part

## cocoapods

Use cocoapods to manage iOS dependencies.

Install pods by `brew install cocoapods`

In a RN project, [RN Face Game](https://github.com/lumixraku/OpenCVRN)

Then cd into ios folder, running `pod install` in the folder which contains `Podfile`

Then `yarn run ios --simulator='iPhone SE (2nd generation)'` to run your RN app.

##  Passing Values From Native to JS.

Just use native object to store value. `NSMutableDictionary` OR `NSMutableArray`.