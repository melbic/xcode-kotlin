# Kotlin Native Xcode Support

Plugin to facilitate debugging iOS applications using Kotlin Native in Xcode. Defines Kotlin files as source code, with basic highlighting. Allows you to set breakpoints and includes llvm support to view data in the debug window. Xcode does not officially support custom language definitions, but they also don't explicitly block them.

> ## **We're Hiring!**
>
> Touchlab is looking for a Mobile Developer, with Android/Kotlin experience, who is eager to dive into Kotlin Multiplatform Mobile (KMM) development. Come join the remote-first team putting KMM in production. [More info here](https://go.touchlab.co/careers-gh).

## Installation

There are 2 parts to Kotlin support: 1) debugging support and 2) language color and style formatting.

You need to tell Xcode that `*.kt` files are source files, and run an lldb formatter script when debugging starts. Advanced users may want to do this manually, but if you have Xcode installed in the default place, you can run the setup script.

### Xcode 11 and newer versions

For debugging support in Xcode 11 or a newer version, run the installation script:

```
./setup-xcode11.sh
```

 Xcode 11 introduced several breaking changes from earlier versions, and some resolutions are still outstanding. If you're using Xcode 11 or a newer version, you need to move some files into a protected area. Some users may not want to do this, and may possibly not have permissions to do this. You'll need to run the formatting support script with sufficient permissions, which generally means `sudo`.

```
sudo ./colorsetup-xcode11.sh
```

*You can still debug Kotlin without formatting support, just FYI. This step is not required.*

### All Other Xcode Versions

For all other versions of Xcode, the following script will install both debugging and formatting support:

```
./setup.sh
```

## Kotlin 1.3.6x Issue

Using static frameworks and/or Cocoapods may remove debug info in this version. [More info here](https://github.com/JetBrains/kotlin-native/issues/3446)

### Special Note

All of that magic was sorted out by [Ellen Shapiro](https://github.com/designatednerd), who understands all of this 
far better than I ever will.
 
[Tracking Issue Here](https://github.com/apollographql/xcode-graphql/issues/23)

## Usage

If properly set up, you should be able to add Kotlin source to Xcode, set up breakpoints, and step through code.

We're deprecating the Xcode Sync plugin. Add folder reference instead! [See issue](https://github.com/touchlab/xcode-kotlin/issues/16). Description and video coming soon.

~~To help automate adding Kotlin source, check out the [Kotlin Xcode Sync](https://github.com/touchlab/KotlinXcodeSync) Gradle plugin.~~

### Sample

The [Droidcon NYC](https://github.com/touchlab/DroidconKotlin/) app has both the Xcode Gradle sync and Xcode projects enabled for debugging.

### Sources

Setting up the Plugin has been an amalgam of various source projects, as Xcode "Plugins" are undocumented. The most significant piece, the language color file came from other color files shipped with Xcode. Xcode plugin file from [GraphQL](https://github.com/apollographql/xcode-graphql/blob/master/GraphQL.ideplugin/Contents/Resources/GraphQL.xcplugindata)

LLDB formatting originally comes from the Kotlin/Native project, source [konan_lldb.py](https://github.com/JetBrains/kotlin-native/blob/dbb162a4b523071f31913e888e212df344a1b61e/llvmDebugInfoC/src/scripts/konan_lldb.py), although the way data is grabbed has been heavily modified to better support an interactive debugger.

## Possible Future Stuff

### Color File

The color definition is basically Java's with minor additions. This could be better adapted to Kotlin.

### Install

It's a bash script, which works, but does not take into account non-standard install directories and various other possible config options. This could be improved.

### From Swift

You can see variables when you're debugging Kotlin, but when you're in a swift file that has a class that came from Kotlin
you can't see much. It would be great to be able to improve that.

### Better Debug Alignment

This happens in the Kotlin compiler, so it's a little deeper, but the breakpoints don't always track with the source 
when there are more complex structures (lambdas, etc). This should improve over time.

## Xcode Updates

Every time Xcode is updated we need the UUID. It needs to be added to `Kotlin.ideplugin/Contents/Info.plist`. To find the 
UUID of your version of Xcode, run the following:

```
defaults read /Applications/Xcode.app/Contents/Info DVTPlugInCompatibilityUUID
```

Info [from here](https://www.mokacoding.com/blog/xcode-plugins-update/)
