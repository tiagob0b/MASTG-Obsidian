## Overview

## Static Analysis

There are various keywords to look for: check the libraries mentioned in the overview and static analysis of the section "Verifying the Configuration of Cryptographic Standard Algorithms" for which keywords you can best check on how keys are stored.

Always make sure that:

- keys are not synchronized over devices if it is used to protect high-risk data.
- keys are not stored without additional protection.
- keys are not hardcoded.
- keys are not derived from stable features of the device.
- keys are not hidden by use of lower level languages (e.g. C/C++).
- keys are not imported from unsafe locations.

Check also the [list of common cryptographic configuration issues](https://mas.owasp.org/MASTG/General/0x04g-Testing-Cryptography#common-configuration-issues).

Most of the recommendations for static analysis can already be found in chapter "Testing Data Storage for iOS". Next, you can read up on it at the following pages:

- [Apple Developer Documentation: Certificates and keys ↗](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys "Certificates and keys")
- [Apple Developer Documentation: Generating new keys ↗](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys/generating_new_cryptographic_keys "Generating new keys")
- [Apple Developer Documentation: Key generation attributes ↗](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys/key_generation_attributes "Key Generation attributes")

## Dynamic Analysis

Hook cryptographic methods and analyze the keys that are being used. Monitor file system access while cryptographic operations are being performed to assess where key material is written to or read from.

## Resources

- [Certificates and keys ↗](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys "Certificates and keys")
- [Generating new keys ↗](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys/generating_new_cryptographic_keys "Generating new keys")
- [Key Generation attributes ↗](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys/key_generation_attributes "Key Generation attributes")