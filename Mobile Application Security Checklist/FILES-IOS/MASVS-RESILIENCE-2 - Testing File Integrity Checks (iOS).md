## Overview

**Application Source Code Integrity Checks:**

Run the app on the device in an unmodified state and make sure that everything works. Then apply patches to the executable using optool, re-sign the app as described in the chapter [iOS Tampering and Reverse Engineering](https://mas.owasp.org/MASTG/iOS/0x06c-Reverse-Engineering-and-Tampering#patching-repackaging-and-re-signing), and run it.

The app should respond in some way. For example by:

- Alerting the user and asking for accepting liability.
- Preventing execution by gracefully terminating.
- Securely wiping any sensitive data stored on the device.
- Reporting to a backend server, e.g, for fraud detection.

Work on bypassing the defenses and answer the following questions:

- Can the mechanisms be bypassed trivially (e.g., by hooking a single API function)?
- How difficult is identifying the detection code via static and dynamic analysis?
- Did you need to write custom code to disable the defenses? How much time did you need?
- What is your assessment of the difficulty of bypassing the mechanisms?

**File Storage Integrity Checks:**

Go to the app data directories as indicated in section [Accessing App Data Directories](https://mas.owasp.org/MASTG/iOS/0x06b-iOS-Security-Testing#accessing-app-data-directories) and modify some files.

Next, work on bypassing the defenses and answer the following questions:

- Can the mechanisms be bypassed trivially (e.g., by changing the contents of a file or a key-value pair)?
- How difficult is obtaining the HMAC key or the asymmetric private key?
- Did you need to write custom code to disable the defenses? How much time did you need?
- What is your assessment of the difficulty of bypassing the mechanisms?