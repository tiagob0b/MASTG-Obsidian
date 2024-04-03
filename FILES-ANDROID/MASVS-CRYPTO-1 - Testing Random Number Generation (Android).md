## Overview

## Static Analysis

Identify all the instances of random number generators and look for either custom or well-known insecure classes. For instance, `java.util.Random` produces an identical sequence of numbers for each given seed value; consequently, the sequence of numbers is predictable. Instead a well-vetted algorithm should be chosen that is currently considered to be strong by experts in the field, and a well-tested implementations with adequate length seeds should be used.

Identify all instances of `SecureRandom` that are not created using the default constructor. Specifying the seed value may reduce randomness. Prefer the [no-argument constructor of `SecureRandom` ↗](https://www.securecoding.cert.org/confluence/display/java/MSC02-J.+Generate+strong+random+numbers "Generation of Strong Random Numbers") that uses the system-specified seed value to generate a 128-byte-long random number.

In general, if a PRNG is not advertised as being cryptographically secure (e.g. `java.util.Random`), then it is probably a statistical PRNG and should not be used in security-sensitive contexts. Pseudo-random number generators [can produce predictable numbers ↗](https://www.securecoding.cert.org/confluence/display/java/MSC63-J.+Ensure+that+SecureRandom+is+properly+seeded "Proper seeding of SecureRandom") if the generator is known and the seed can be guessed. A 128-bit seed is a good starting point for producing a "random enough" number.

Once an attacker knows what type of weak pseudo-random number generator (PRNG) is used, it can be trivial to write a proof-of-concept to generate the next random value based on previously observed ones, as it was [done for Java Random ↗](https://franklinta.com/2014/08/31/predicting-the-next-math-random-in-java/ "Predicting the next Math.random() in Java"). In case of very weak custom random generators it may be possible to observe the pattern statistically. Although the recommended approach would anyway be to decompile the APK and inspect the algorithm (see Static Analysis).

If you want to test for randomness, you can try to capture a large set of numbers and check with the Burp's [sequencer ↗](https://portswigger.net/burp/documentation/desktop/tools/sequencer "Burp\'s Sequencer") to see how good the quality of the randomness is.

## Dynamic Analysis
You can use [method tracing](https://mas.owasp.org/MASTG/Android/0x05c-Reverse-Engineering-and-Tampering#method-tracing) on the mentioned classes and methods to determine input / output values being used.

## Resources

- [Burp's Sequencer ↗](https://portswigger.net/burp/documentation/desktop/tools/sequencer "Burp's Sequencer")
- [Generation of Strong Random Numbers ↗](https://www.securecoding.cert.org/confluence/display/java/MSC02-J.+Generate+strong+random+numbers "Generation of Strong Random Numbers")
- [Predicting the next Math.random() in Java ↗](https://franklinta.com/2014/08/31/predicting-the-next-math-random-in-java/ "Predicting the next Math.random() in Java")
- [Proper seeding of SecureRandom ↗](https://www.securecoding.cert.org/confluence/display/java/MSC63-J.+Ensure+that+SecureRandom+is+properly+seeded "Proper seeding of SecureRandom")