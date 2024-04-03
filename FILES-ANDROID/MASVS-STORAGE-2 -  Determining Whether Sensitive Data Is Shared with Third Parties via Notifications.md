## Overview

## Static Analysis

Search for any usage of the `NotificationManager` class which might be an indication of some form of notification management. If the class is being used, the next step would be to understand how the application is [generating the notifications ↗](https://developer.android.com/training/notify-user/build-notification#SimpleNotification "Create a Notification") and which data ends up being shown.

## Dynamic Analysis

Run the application and start tracing all calls to functions related to the notifications creation, e.g. `setContentTitle` or `setContentText` from [`NotificationCompat.Builder` ↗](https://developer.android.com/reference/androidx/core/app/NotificationCompat.Builder). Observe the trace in the end and evaluate if it contains any sensitive information which another app might have eavesdropped.

## Resources

- [Create a Notification ↗](https://developer.android.com/training/notify-user/build-notification#SimpleNotification "Create a Notification")
- [`NotificationCompat.Builder` ↗](https://developer.android.com/reference/androidx/core/app/NotificationCompat.Builder)