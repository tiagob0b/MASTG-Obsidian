## Overview

## Static Analysis

- Search through the source code for similar implementations, such as

  `textObject.autocorrectionType = UITextAutocorrectionTypeNo;   textObject.secureTextEntry = YES;`

- Open xib and storyboard files in the `Interface Builder` of Xcode and verify the states of `Secure Text Entry` and `Correction` in the `Attributes Inspector` for the appropriate object.

The application must prevent the caching of sensitive information entered into text fields. You can prevent caching by disabling it programmatically, using the `textObject.autocorrectionType = UITextAutocorrectionTypeNo` directive in the desired UITextFields, UITextViews, and UISearchBars. For data that should be masked, such as PINs and passwords, set `textObject.secureTextEntry` to `YES`.

`UITextField *textField = [ [ UITextField alloc ] initWithFrame: frame ]; textField.autocorrectionType = UITextAutocorrectionTypeNo;`

## Dynamic Analysis

If a jailbroken iPhone is available, execute the following steps:

1. Reset your iOS device keyboard cache by navigating to `Settings > General > Reset > Reset Keyboard Dictionary`.
2. Use the application and identify the functionalities that allow users to enter sensitive data.
3. Dump the keyboard cache file with the extension `.dat` in the following directory and its subdirectories. (which might be different for iOS versions before 8.0): `/private/var/mobile/Library/Keyboard/`
4. Look for sensitive data, such as username, passwords, email addresses, and credit card numbers. If the sensitive data can be obtained via the keyboard cache file, the app fails this test.

`UITextField *textField = [ [ UITextField alloc ] initWithFrame: frame ]; textField.autocorrectionType = UITextAutocorrectionTypeNo;`

If you must use a non-jailbroken iPhone:

1. Reset the keyboard cache.
2. Key in all sensitive data.
3. Use the app again and determine whether autocorrect suggests previously entered sensitive information.