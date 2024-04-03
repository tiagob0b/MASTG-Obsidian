## Overview

To test for jailbreak detection install the app on a jailbroken device.

**Launch the app and see what happens:**

If it implements jailbreak detection, you might notice one of the following things:

- The app crashes and closes immediately, without any notification.
- A pop-up window indicates that the app won't run on a jailbroken device.

Note that crashes might be an indicator of jailbreak detection but the app may be crashing for any other reasons, e.g. it may have a bug. We recommend to test the app on non-jailbroken device first, especially when you're testing preproduction versions.

**Launch the app and try to bypass Jailbreak Detection using an automated tool:**

If it implements jailbreak detection, you might be able to see indicators of that in the output of the tool. See section [Automated Jailbreak Detection Bypass](https://mas.owasp.org/MASTG/iOS/0x06j-Testing-Resiliency-Against-Reverse-Engineering#automated-jailbreak-detection-bypass).

**Reverse Engineer the app:**

The app might be using techniques that are not implemented in the automated tools that you've used. If that's the case you must reverse engineer the app to find proofs. See section [Manual Jailbreak Detection Bypass](https://mas.owasp.org/MASTG/iOS/0x06j-Testing-Resiliency-Against-Reverse-Engineering#manual-jailbreak-detection-bypass).