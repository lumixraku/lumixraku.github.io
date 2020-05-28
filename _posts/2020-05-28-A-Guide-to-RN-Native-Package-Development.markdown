# A Guide to RN Native package development.

First you have to read this

[Native Guide For Android](https://reactnative.dev/docs/native-modules-android)

[Native Guide For iOS](https://reactnative.dev/docs/native-modules-ios)

## What you need ?

For Android, Android Studio is required. For iOS, you need Xcode and CocoaPods.

I suggest install CocoaPods by Homebrew. `brew install cocoapods`.

PS: remember add `~/Library/Android/sdk/platform-tools/` to your bin path.

## About Gradle
Android use gradle to manage deployment and workflows. Here are some basic conecpts you have to know.

### In most case,  there are at least two build.gradle file in your project.
https://stackoverflow.com/questions/28295933/difference-between-build-gradleproject-and-build-gradlemodule

Top-level build file where you can add configuration options common to all sub-projects/modules.

All modules have a specific gradle file itself. Whatever is included in this gradle file, it will only affect the module that is included on.