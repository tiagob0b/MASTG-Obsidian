## Overview

## Static Analysis

### Testing Network Requests over Secure Protocols
First, you should identify all network requests in the source code and ensure that no plain HTTP URLs are used. Make sure that sensitive information is sent over secure channels by using [`HttpsURLConnection` ↗](https://developer.android.com/reference/javax/net/ssl/HttpsURLConnection.html "HttpsURLConnection") or [`SSLSocket` ↗](https://developer.android.com/reference/javax/net/ssl/SSLSocket.html "SSLSocket") (for socket-level communication using TLS).

### Testing Network API Usage

Next, even when using a low-level API which is supposed to make secure connections (such as `SSLSocket`), be aware that it has to be securely implemented. For instance, `SSLSocket` **doesn't** verify the hostname. Use `getDefaultHostnameVerifier` to verify the hostname. The Android developer documentation includes a [code example ↗](https://developer.android.com/training/articles/security-ssl.html#WarningsSslSocket "Warnings About Using SSLSocket Directly").

### Testing for Cleartext Traffic

Next, you should ensure that the app is not allowing cleartext HTTP traffic. Since Android 9 (API level 28) cleartext HTTP traffic is blocked by default (thanks to the [default Network Security Configuration](https://mas.owasp.org/MASTG/Android/0x05g-Testing-Network-Communication#default-configurations)) but there are multiple ways in which an application can still send it:

- Setting the [`android:usesCleartextTraffic` ↗](https://developer.android.com/guide/topics/manifest/application-element#usesCleartextTraffic "Android documentation - usesCleartextTraffic flag") attribute of the `<application>` tag in the AndroidManifest.xml file. Note that this flag is ignored in case the Network Security Configuration is configured.
- Configuring the Network Security Configuration to enable cleartext traffic by setting the `cleartextTrafficPermitted` attribute to true on `<domain-config>` elements.
- Using low-level APIs (e.g. [`Socket` ↗](https://developer.android.com/reference/java/net/Socket "Socket class")) to set up a custom HTTP connection.
- Using a cross-platform framework (e.g. Flutter, Xamarin, ...), as these typically have their own implementations for HTTP libraries.

All of the above cases must be carefully analyzed as a whole. For example, even if the app does not permit cleartext traffic in its Android Manifest or Network Security Configuration, it might actually still be sending HTTP traffic. That could be the case if it's using a low-level API (for which Network Security Configuration is ignored) or a badly configured cross-platform framework.

For more information refer to the article ["Security with HTTPS and SSL" ↗](https://developer.android.com/training/articles/security-ssl.html).

## Dynamic Analysis

Intercept the tested app's incoming and outgoing network traffic and make sure that this traffic is encrypted. You can intercept network traffic in any of the following ways:

- Capture all HTTP(S) and Websocket traffic with an interception proxy like [OWASP ZAP](https://mas.owasp.org/MASTG/Tools/0x08a-Testing-Tools#owasp-zap) or [Burp Suite](https://mas.owasp.org/MASTG/Tools/0x08a-Testing-Tools#burp-suite) and make sure all requests are made via HTTPS instead of HTTP.
- Interception proxies like Burp and OWASP ZAP will show HTTP(S) traffic only. You can, however, use a Burp plugin such as [Burp-non-HTTP-Extension ↗](https://github.com/summitt/Burp-Non-HTTP-Extension "Burp-non-HTTP-Extension") or the tool [mitm-relay ↗](https://github.com/jrmdev/mitm_relay "mitm-relay") to decode and visualize communication via XMPP and other protocols.

> Some applications may not work with proxies like Burp and OWASP ZAP because of Certificate Pinning. In such a scenario, please check [Testing Custom Certificate Stores and Certificate Pinning](https://mas.owasp.org/MASTG/tests/android/MASVS-NETWORK/MASTG-TEST-0019/).

For more details refer to:

- [Intercepting Traffic on the Network Layer](https://mas.owasp.org/MASTG/General/0x04f-Testing-Network-Communication#intercepting-traffic-on-the-network-layer) from chapter "Mobile App Network Communication"
- [Setting up a Network Testing Environment](https://mas.owasp.org/MASTG/Android/0x05b-Android-Security-Testing#setting-up-a-network-testing-environment) from chapter "Android Basic Security Testing"

## Resources

- [Android documentation - usesCleartextTraffic flag ↗](https://developer.android.com/guide/topics/manifest/application-element#usesCleartextTraffic "Android documentation - usesCleartextTraffic flag")
- [Burp-non-HTTP-Extension ↗](https://github.com/summitt/Burp-Non-HTTP-Extension "Burp-non-HTTP-Extension")
- [HttpsURLConnection ↗](https://developer.android.com/reference/javax/net/ssl/HttpsURLConnection.html "HttpsURLConnection")
- [SSLSocket ↗](https://developer.android.com/reference/javax/net/ssl/SSLSocket.html "SSLSocket")
- [Security with HTTPS and SSL ↗](https://developer.android.com/training/articles/security-ssl.html)
- [Socket class ↗](https://developer.android.com/reference/java/net/Socket "Socket class")
- [Warnings About Using SSLSocket Directly ↗](https://developer.android.com/training/articles/security-ssl.html#WarningsSslSocket "Warnings About Using SSLSocket Directly")
- [mitm-relay ↗](https://github.com/jrmdev/mitm_relay "mitm-relay")