## Overview

## Static Analysis

The following section summarizes keywords that you should look for to identify IPC implementations within iOS source code.

### XPC Services

Several classes may be used to implement the NSXPCConnection API:

- NSXPCConnection
- NSXPCInterface
- NSXPCListener
- NSXPCListenerEndpoint

You can set [security attributes ↗](https://www.objc.io/issues/14-mac/xpc/#security-attributes-of-the-connection "Security Attributes of NSXPCConnection") for the connection. The attributes should be verified.

Check for the following two files in the Xcode project for the XPC Services API (which is C-based):

- [`xpc.h` ↗](https://developer.apple.com/documentation/xpc/xpc_services_xpc.h "xpc.h")
- `connection.h`

### Mach Ports

Keywords to look for in low-level implementations:

- mach_port_t
- mach_msg_*

Keywords to look for in high-level implementations (Core Foundation and Foundation wrappers):

- CFMachPort
- CFMessagePort
- NSMachPort
- NSMessagePort

### NSFileCoordinator

Keywords to look for:

- NSFileCoordinator

## Dynamic Analysis
Verify IPC mechanisms with static analysis of the iOS source code. No iOS tool is currently available to verify IPC usage.

## Resources

- [Security Attributes of NSXPCConnection ↗](https://www.objc.io/issues/14-mac/xpc/#security-attributes-of-the-connection "Security Attributes of NSXPCConnection")
- [xpc.h ↗](https://developer.apple.com/documentation/xpc/xpc_services_xpc.h "xpc.h")