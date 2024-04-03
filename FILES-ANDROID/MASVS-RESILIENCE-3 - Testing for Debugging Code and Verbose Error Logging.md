## Overview

## Static Analysis

To determine whether `StrictMode` is enabled, you can look for the `StrictMode.setThreadPolicy` or `StrictMode.setVmPolicy` methods. Most likely, they will be in the `onCreate` method.

The [detection methods for the thread policy ↗](https://javabeat.net/strictmode-android-1/ "What is StrictMode in Android?") are

`detectDiskWrites() detectDiskReads() detectNetwork()`

The [penalties for thread policy violation ↗](https://javabeat.net/strictmode-android-1/ "What is StrictMode in Android?") are

`penaltyLog() // Logs a message to LogCat penaltyDeath() // Crashes application, runs at the end of all enabled penalties penaltyDialog() // Shows a dialog`

Have a look at the [best practices ↗](https://code.tutsplus.com/tutorials/android-best-practices-strictmode--mobile-7581 "Android Best Practices: StrictMode") for using StrictMode.

## Dynamic Analysis

There are several ways of detecting `StrictMode`; the best choice depends on how the policies' roles are implemented. They include

- Logcat,
- a warning dialog,
- application crash.

## Resources

- [Android Best Practices: StrictMode ↗](https://code.tutsplus.com/tutorials/android-best-practices-strictmode--mobile-7581 "Android Best Practices: StrictMode")
- [What is StrictMode in Android? ↗](https://javabeat.net/strictmode-android-1/ "What is StrictMode in Android?")