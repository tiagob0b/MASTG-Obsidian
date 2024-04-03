## Overview

## Static Analysis

A text field that masks its input can be configured in two ways:

**Storyboard** In the iOS project's storyboard, navigate to the configuration options for the text field that takes sensitive data. Make sure that the option "Secure Text Entry" is selected. If this option is activated, dots are shown in the text field in place of the text input.

**Source Code** If the text field is defined in the source code, make sure that the option [`isSecureTextEntry` ↗](https://developer.apple.com/documentation/uikit/uitextinputtraits/1624427-issecuretextentry "isSecureTextEntry in Text Field") is set to "true". This option obscures the text input by showing dots.

`sensitiveTextField.isSecureTextEntry = true`

## Dynamic Analysis

To determine whether the application leaks any sensitive information to the user interface, run the application and identify components that either show such information or take it as input.

If the information is masked by, for example, asterisks or dots, the app isn't leaking data to the user interface.

## Resources

- [isSecureTextEntry in Text Field ↗](https://developer.apple.com/documentation/uikit/uitextinputtraits/1624427-issecuretextentry "isSecureTextEntry in Text Field")