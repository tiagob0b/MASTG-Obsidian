## Overview
## Static Analysis
Identify all the instances of the cryptographic primitives in code. Identify all custom cryptography implementations. You can look for:

- classes `Cipher`, `Mac`, `MessageDigest`, `Signature`
- interfaces `Key`, `PrivateKey`, `PublicKey`, `SecretKey`
- functions `getInstance`, `generateKey`
- exceptions `KeyStoreException`, `CertificateException`, `NoSuchAlgorithmException`
- classes which uses `java.security.*`, `javax.crypto.*`, `android.security.*` and `android.security.keystore.*` packages.

Identify that all calls to getInstance use default `provider` of security services by not specifying it (it means AndroidOpenSSL aka Conscrypt). `Provider` can only be specified in `KeyStore` related code (in that situation `KeyStore` should be provided as `provider`). If other `provider` is specified it should be verified according to situation and business case (i.e. Android API version), and `provider` should be examined against potential vulnerabilities.

Ensure that the best practices outlined in the "[Cryptography for Mobile Apps](https://mas.owasp.org/MASTG/General/0x04g-Testing-Cryptography)" chapter are followed. Look at [insecure and deprecated algorithms](https://mas.owasp.org/MASTG/General/0x04g-Testing-Cryptography#identifying-insecure-and/or-deprecated-cryptographic-algorithms) and [common configuration issues](https://mas.owasp.org/MASTG/General/0x04g-Testing-Cryptography#common-configuration-issues).

## Dynamic Analysis

You can use [method tracing](https://mas.owasp.org/MASTG/Android/0x05c-Reverse-Engineering-and-Tampering#method-tracing) on cryptographic methods to determine input / output values such as the keys that are being used. Monitor file system access while cryptographic operations are being performed to assess where key material is written to or read from. For example, monitor the file system by using the [API monitor ↗](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security#8-api-monitor---android-only) of [RMS - Runtime Mobile Security](https://mas.owasp.org/MASTG/Tools/0x08a-Testing-Tools#RMS-Runtime-Mobile-Security).

## Resources
- [API monitor ↗](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security#8-api-monitor---android-only)