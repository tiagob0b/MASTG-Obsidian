## Overview

All the presented cases must be carefully analyzed as a whole. For example, even if the app does not permit cleartext traffic in its Info.plist, it might actually still be sending HTTP traffic. That could be the case if it's using a low-level API (for which ATS is ignored) or a badly configured cross-platform framework.

> IMPORTANT: You should apply these tests to the app main code but also to any app extensions, frameworks or Watch apps embedded within the app as well.

For more information refer to the article ["Preventing Insecure Network Connections" ↗](https://developer.apple.com/documentation/security/preventing_insecure_network_connections) and ["Fine-tune your App Transport Security settings" ↗](https://developer.apple.com/news/?id=jxky8h89) in the Apple Developer Documentation.

## Static Analysis

### Testing Network Requests over Secure Protocols

First, you should identify all network requests in the source code and ensure that no plain HTTP URLs are used. Make sure that sensitive information is sent over secure channels by using [`URLSession` ↗](https://developer.apple.com/documentation/foundation/urlsession) (which uses the standard [URL Loading System from iOS ↗](https://developer.apple.com/documentation/foundation/url_loading_system)) or [`Network` ↗](https://developer.apple.com/documentation/network) (for socket-level communication using TLS and access to TCP and UDP).

### Check for Low-Level Networking API Usage

Identify the network APIs used by the app and see if it uses any low-level networking APIs.

> **Apple Recommendation: Prefer High-Level Frameworks in Your App**: "ATS doesn’t apply to calls your app makes to lower-level networking interfaces like the Network framework or CFNetwork. In these cases, you take responsibility for ensuring the security of the connection. You can construct a secure connection this way, but mistakes are both easy to make and costly. It’s typically safest to rely on the URL Loading System instead" (see [source ↗](https://developer.apple.com/documentation/security/preventing_insecure_network_connections)).

If the app uses any low-level APIs such as [`Network` ↗](https://developer.apple.com/documentation/network) or [`CFNetwork` ↗](https://developer.apple.com/documentation/cfnetwork), you should carefully investigate if they are being used securely. For apps using cross-platform frameworks (e.g. Flutter, Xamarin, ...) and third party frameworks (e.g. Alamofire) you should analyze if they're being configured and used securely according to their best practices.

Make sure that the app:

- verifies the challenge type and the host name and credentials when performing server trust evaluation.
- doesn't ignore TLS errors.
- doesn't use any insecure TLS configurations (see [Testing the TLS Settings](https://mas.owasp.org/MASTG/tests/ios/MASVS-NETWORK/MASTG-TEST-0065/)

These checks are orientative, we cannot name specific APIs since every app might use a different framework. Please use this information as a reference when inspecting the code.

### Testing for Cleartext Traffic

Ensure that the app is not allowing cleartext HTTP traffic. Since iOS 9.0 cleartext HTTP traffic is blocked by default (due to App Transport Security (ATS)) but there are multiple ways in which an application can still send it:

- Configuring ATS to enable cleartext traffic by setting the `NSAllowsArbitraryLoads` attribute to `true` (or `YES`) on `NSAppTransportSecurity` in the app's `Info.plist`.
- [Retrieve the `Info.plist`](https://mas.owasp.org/MASTG/iOS/0x06b-iOS-Security-Testing#the-infoplist-file)
- Check that `NSAllowsArbitraryLoads` is not set to `true` globally of for any domain.
    
- If the application opens third party web sites in WebViews, then from iOS 10 onwards `NSAllowsArbitraryLoadsInWebContent` can be used to disable ATS restrictions for the content loaded in web views.
    

> **Apple warns:** Disabling ATS means that unsecured HTTP connections are allowed. HTTPS connections are also allowed, and are still subject to default server trust evaluation. However, extended security checks—like requiring a minimum Transport Layer Security (TLS) protocol version—are disabled. Without ATS, you’re also free to loosen the default server trust requirements, as described in ["Performing Manual Server Trust Authentication" ↗](https://developer.apple.com/documentation/foundation/url_loading_system/handling_an_authentication_challenge/performing_manual_server_trust_authentication).

The following snippet shows a **vulnerable example** of an app disabling ATS restrictions globally.

`<key>NSAppTransportSecurity</key> <dict>     <key>NSAllowsArbitraryLoads</key>     <true/> </dict>`

ATS should be examined taking the application's context into consideration. The application may _have to_ define ATS exceptions to fulfill its intended purpose. For example, the [Firefox iOS application has ATS disabled globally ↗](https://github.com/mozilla-mobile/firefox-ios/blob/v97.0/Client/Info.plist#L82). This exception is acceptable because otherwise the application would not be able to connect to any HTTP website that does not have all the ATS requirements. In some cases, apps might disable ATS globally but enable it for certain domains to e.g. securely load metadata or still allow secure login.

ATS should include a [justification string ↗](https://developer.apple.com/documentation/security/preventing_insecure_network_connections#3138036) for this (e.g. "The app must connect to a server managed by another entity that doesn’t support secure connections.").

## Dynamic Analysis

Intercept the tested app's incoming and outgoing network traffic and make sure that this traffic is encrypted. You can intercept network traffic in any of the following ways:

- Capture all HTTP(S) and Websocket traffic with an interception proxy like [OWASP ZAP](https://mas.owasp.org/MASTG/Tools/0x08a-Testing-Tools#owasp-zap) or [Burp Suite](https://mas.owasp.org/MASTG/Tools/0x08a-Testing-Tools#burp-suite) and make sure all requests are made via HTTPS instead of HTTP.
- Interception proxies like Burp and OWASP ZAP will show HTTP(S) traffic only. You can, however, use a Burp plugin such as [Burp-non-HTTP-Extension ↗](https://github.com/summitt/Burp-Non-HTTP-Extension "Burp-non-HTTP-Extension") or the tool [mitm-relay ↗](https://github.com/jrmdev/mitm_relay "mitm-relay") to decode and visualize communication via XMPP and other protocols.

> Some applications may not work with proxies like Burp and OWASP ZAP because of Certificate Pinning. In such a scenario, please check [Testing Custom Certificate Stores and Certificate Pinning](https://mas.owasp.org/MASTG/tests/ios/MASVS-NETWORK/MASTG-TEST-0065/).

For more details refer to:

- "Intercepting Traffic on the Network Layer" from chapter [Testing Network Communication](https://mas.owasp.org/MASTG/General/0x04f-Testing-Network-Communication#intercepting-traffic-on-the-network-layer)
- "Setting up a Network Testing Environment" from chapter [iOS Basic Security Testing](https://mas.owasp.org/MASTG/iOS/0x06b-iOS-Security-Testing#setting-up-a-network-testing-environment)

## Resources

- [Burp-non-HTTP-Extension ↗](https://github.com/summitt/Burp-Non-HTTP-Extension "Burp-non-HTTP-Extension")
- [Fine-tune your App Transport Security settings ↗](https://developer.apple.com/news/?id=jxky8h89)
- [Firefox iOS application has ATS disabled globally ↗](https://github.com/mozilla-mobile/firefox-ios/blob/v97.0/Client/Info.plist#L82)
- [Performing Manual Server Trust Authentication ↗](https://developer.apple.com/documentation/foundation/url_loading_system/handling_an_authentication_challenge/performing_manual_server_trust_authentication)
- [Preventing Insecure Network Connections ↗](https://developer.apple.com/documentation/security/preventing_insecure_network_connections)
- [URL Loading System from iOS ↗](https://developer.apple.com/documentation/foundation/url_loading_system))
- [`CFNetwork` ↗](https://developer.apple.com/documentation/cfnetwork)
- [`Network` ↗](https://developer.apple.com/documentation/network)
- [`URLSession` ↗](https://developer.apple.com/documentation/foundation/urlsession)
- [justification string ↗](https://developer.apple.com/documentation/security/preventing_insecure_network_connections#3138036)
- [mitm-relay ↗](https://github.com/jrmdev/mitm_relay "mitm-relay")
- [source ↗](https://developer.apple.com/documentation/security/preventing_insecure_network_connections))