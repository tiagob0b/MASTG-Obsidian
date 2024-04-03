## Overview

## Static Analysis

A screenshot of the current activity is taken when an Android app goes into background and displayed for aesthetic purposes when the app returns to the foreground. However, this may leak sensitive information.

To determine whether the application may expose sensitive information via the app switcher, find out whether the [`FLAG_SECURE` ↗](https://developer.android.com/reference/android/view/Display.html#FLAG_SECURE "FLAG_SECURE Option") option has been set. You should find something similar to the following code snippet:

Example in Java:

`getWindow().setFlags(WindowManager.LayoutParams.FLAG_SECURE,                 WindowManager.LayoutParams.FLAG_SECURE);  setContentView(R.layout.activity_main);`

Example in Kotlin:

`window.setFlags(WindowManager.LayoutParams.FLAG_SECURE,                 WindowManager.LayoutParams.FLAG_SECURE)  setContentView(R.layout.activity_main)`

If the option has not been set, the application is vulnerable to screen capturing.

## Dynamic Analysis

While black-box testing the app, navigate to any screen that contains sensitive information and click the home button to send the app to the background, then press the app switcher button to see the snapshot. As shown below, if `FLAG_SECURE` is set (left image), the snapshot will be empty; if the flag has not been set (right image), activity information will be shown:

![](https://mas.owasp.org/assets/Images/Chapters/0x05d/2.png) ![](https://mas.owasp.org/assets/Images/Chapters/0x05d/1.png)

On devices supporting [file-based encryption (FBE) ↗](https://source.android.com/security/encryption/file-based "FBE"), snapshots are stored in the `/data/system_ce/<USER_ID>/<IMAGE_FOLDER_NAME>` folder. `<IMAGE_FOLDER_NAME>` depends on the vendor but most common names are `snapshots` and `recent_images`. If the device doesn't support FBE, the `/data/system/<IMAGE_FOLDER_NAME>` folder is used.

> Accessing these folders and the snapshots requires root.

## Resources

- [FBE ↗](https://source.android.com/security/encryption/file-based "FBE")
- [FLAG_SECURE Option ↗](https://developer.android.com/reference/android/view/Display.html#FLAG_SECURE "FLAG_SECURE Option")