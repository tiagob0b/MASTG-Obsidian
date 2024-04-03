## Overview

## Static Analysis

In Swift, the [`SecRandomCopyBytes` API ↗](https://developer.apple.com/reference/security/1399291-secrandomcopybytes "SecRandomCopyBytes (Swift)") is defined as follows:

`func SecRandomCopyBytes(_ rnd: SecRandomRef?,                       _ count: Int,                       _ bytes: UnsafeMutablePointer<UInt8>) -> Int32`

The [Objective-C version ↗](https://developer.apple.com/reference/security/1399291-secrandomcopybytes?language=objc "SecRandomCopyBytes (Objective-C)") is

`int SecRandomCopyBytes(SecRandomRef rnd, size_t count, uint8_t *bytes);`

The following is an example of the APIs usage:

`int result = SecRandomCopyBytes(kSecRandomDefault, 16, randomBytes);`

Note: if other mechanisms are used for random numbers in the code, verify that these are either wrappers around the APIs mentioned above or review them for their secure-randomness. Often this is too hard, which means you can best stick with the implementation above.

## Dynamic Analysis

If you want to test for randomness, you can try to capture a large set of numbers and check with [Burp's sequencer plugin ↗](https://portswigger.net/burp/documentation/desktop/tools/sequencer "Sequencer") to see how good the quality of the randomness is.

## Resources

- [SecRandomCopyBytes (Objective-C) ↗](https://developer.apple.com/reference/security/1399291-secrandomcopybytes?language=objc "SecRandomCopyBytes (Objective-C)")
- [SecRandomCopyBytes (Swift) ↗](https://developer.apple.com/reference/security/1399291-secrandomcopybytes "SecRandomCopyBytes (Swift)")
- [Sequencer ↗](https://portswigger.net/burp/documentation/desktop/tools/sequencer "Sequencer")