## Overview

## Static Analysis

Identify all instances where cryptography is used. You can look for:

- classes `Cipher`, `Mac`, `MessageDigest`, `Signature`
- interfaces `Key`, `PrivateKey`, `PublicKey`, `SecretKey`
- functions `getInstance`, `generateKey`
- exceptions `KeyStoreException`, `CertificateException`, `NoSuchAlgorithmException`
- classes importing `java.security.*`, `javax.crypto.*`, `android.security.*`, `android.security.keystore.*`

For each identified instance, identify its purpose and its type. It can be used:

- for encryption/decryption - to ensure data confidentiality
- for signing/verifying - to ensure integrity of data (as well as accountability in some cases)
- for maintenance - to protect keys during certain sensitive operations (such as being imported to the KeyStore)

Additionally, you should identify the business logic which uses identified instances of cryptography.

During verification the following checks should be performed:

- are all keys used according to the purpose defined during its creation? (it is relevant to KeyStore keys, which can have KeyProperties defined)
- for asymmetric keys, is the private key being exclusively used for signing and the public key encryption?
- are symmetric keys used for multiple purposes? A new symmetric key should be generated if it's used in a different context.
- is cryptography used according to its business purpose?

## Dynamic Analysis
You can use [method tracing](https://mas.owasp.org/MASTG/Android/0x05c-Reverse-Engineering-and-Tampering#method-tracing) on cryptographic methods to determine input / output values such as the keys that are being used. Monitor file system access while cryptographic operations are being performed to assess where key material is written to or read from. For example, monitor the file system by using the [API monitor ↗](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security#8-api-monitor---android-only) of [RMS - Runtime Mobile Security](https://mas.owasp.org/MASTG/Tools/0x08a-Testing-Tools#RMS-Runtime-Mobile-Security).

## Resources

- [API monitor ↗](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security#8-api-monitor---android-only)