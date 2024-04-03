## Overview

## Static Analysis

Test the app native libraries to determine if they have the PIE and stack smashing protections enabled.

You can use [radare2's rabin2](https://mas.owasp.org/MASTG/Tools/0x08a-Testing-Tools#radare2) to get the binary information. We'll use the [UnCrackable App for Android Level 4](https://mas.owasp.org/MASTG/Tools/0x08b-Reference-Apps#android-uncrackable-l4) v1.0 APK as an example.

All native libraries must have `canary` and `pic` both set to `true`.

That's the case for `libnative-lib.so`:

`rabin2 -I lib/x86_64/libnative-lib.so | grep -E "canary|pic" canary   true pic      true`

But not for `libtool-checker.so`:

`rabin2 -I lib/x86_64/libtool-checker.so | grep -E "canary|pic" canary   false pic      true`

In this example, `libtool-checker.so` must be recompiled with stack smashing protection support.