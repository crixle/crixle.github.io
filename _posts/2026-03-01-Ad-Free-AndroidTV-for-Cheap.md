---
layout: post
title: Ad-Free Android TV for Under $50
date: 2026-03-01 20:25
category: guide
author: Crixle
tags: ["androidtv", "streaming"]
summary: Android TV device recommendation and how to debloat and remove ads from it.
---
![Clean Android TV Interface](/images/androidtv/cleanandroidtv.png)
## Summary
Onn's 4K Streaming Devices are impressively capable considering the price. With a few tweaks, you can have a clean, ad-free, and more privacy respecting home streaming experience for under $25.

> This guide goes over steps like utilizing Android Debug Bridge (ADB) which *ALWAYS* has risks and can brick/bootloop your device. Proceed with caution and I take no responsibility for any actions you perform on your device.
{: .prompt-warning }


## Pick Your Device
If you weren't aware, 'Onn' is a Walmart's brand for electronics so you'll be able to find all these devices at your local Walmart.

As of writing, there are 4 different models to choose from:

| Name                         | Price | Storage | RAM   | WiFi   |
|------------------------------|-------|---------|-------|--------|
| onn Full HD (1080p) Streaming Device | $15   | 8GB     | 1.5GB | WiFi 5 |
| onn 4K Streaming Device      | $20   | 8GB     | 2GB   | WiFi 5 |
| onn 4K Plus Streaming Device | $30   | 16GB    | 2GB   | WiFi 6 |
| onn 4K Pro Streaming Device  | $45   | 32GB    | 3GB   | WiFi 6 |

If you're unsure, I would recommend the **4K Plus**. It has a better CPU than it's predecessors which makes the experience much more smooth and snappy, and the introduction of WiFi 6 allows better throughput for faster downloads and less buffering for certain content.

I personally wouldn't recommend the Pro since it has a built-in Gemini speaker and you probably don't need *another* eavesdropping microphone in your home.

## Setup Your Device
* Go through the entire out-of-box experience and sign in with your Google account.
* Turn on Developer Mode and enable wireless ADB [Guide](https://www.makeuseof.com/how-to-use-adb-on-android-tv/)
* **OPTIONAL** - Install [SCRCPY](https://github.com/Genymobile/scrcpy) on your PC. It makes typing in passwords much easier and is very useful for other projects.


## Replace Google TV Launcher
All modern Android TV devices come stock with Google TV as it's launcher. This launcher transmits telemetry and tries to suggest content for you to watch all while bogging down your device. We're going to replace it with a new launcher that doesn't do these things.

1. Determine what launcher you'd like to replace the stock one with and install it from Google Play.
    >I would recommend [Projectivy Launcher](https://www.makeuseof.com/how-to-use-adb-on-android-tv/).<br>Projectivy Launcher looks very similar to the old Android TV launcher pre-Google TV and has many customization features, all for free!
    {: .prompt-info }
2. To simplify the process of replacing your default home launcher, download the [Launcher Manager APK for Android TV](https://xdaforums.com/t/app-firetv-noroot-launcher-manager-change-launcher-without-root.4176349/) (half way through the post) and save it somewhere easily accessible on your PC.
    * Note: This is technically optional, just much more easier and convenient. You can always use ADB commands in later steps to replace the launcher but you will have to research what commands work best for your device.
3. Sideload Launcher Manager onto your TV.
    * **Method 1 (easiest):** Install [LocalSend](https://play.google.com/store/apps/details?id=org.localsend.localsend_app) on your TV and put it into Receive mode. Go to [LocalSend web](https://web.localsend.org/) on your PC and send the APK you just downloaded to your TV.\
    Select the transfered APK and install it.\
    Allow any permissions if prompted.
    * **Method 2 (advanced):** Rename the APK you just downlaoded to ```LM.apk``` (or something without spaces) and open your CLI with ADB installed and type in ```adb connect [DEVICE IP ADDRESS]```\
    Navigate to the folder with the Launcher Manager APK in it and input ```adb install LM.apk``` and observe the output for successful install.
4. Launch the newly installed Launcher Manager and select **Enable Custom Launcher**\
The new launcher you recently installed should be auto-selected. \
If it's not then select **Launcher Options** and change **Active launcher** to your new one.

## Remove the Bloat
If you haven't already, go through your home screen on your device and uninstall any app you don't intend on using. If you have some that keep failing to uninstall, refer to the steps below.

>Do not uninstall any packages that start with `com.droidlogic`, `com.amlogic`, `com.dlg`, or `android` unless specifically stated in this guide or after throroughly researching the impact on your own.
{: .prompt-warning }

### Remove Packages with ADB
To you, Netflix is an app on your TV. To Android, it's a package that it manages. We can leverage this to remove apps and other services we don't want. 

>Replace `pm uninstall` with `pm disable` if you are getting errors about uninstalling certain packages.
{: .prompt-tip }

1. Go to your CLI on your PC with ADB installed and make sure your device is connected through WiFi. \
    `adb connect [DEVICE IP ADDRESS]`
2. Enter `adb shell` and you'll enter the shell on the device.
3. Enter `pm list packages` and a list of all installed devices.
4. Locate the package name of the app you want to install. \
    Most apps have their name in the package name like `com.netflix.ninja` but some can be tricky to locate. In doubt, lookup the app on Google Play on your PC and the name will be between the URL between `id=` and `&hl`.
5. Run this command for every package:
    `pm uninstall -k --user 0 com.[package]`\
    Example: `pm uninstall -k --user 0 com.netflix.ninja`

### Remove Tracking Packages
Enter these commands one-by-one to remove the ads and tracking services.
```
pm uninstall -k --user 0 com.android.adservices.api
pm uninstall -k --user 0 com.android.ondevicepersonalization.services
pm uninstall -k --user 0 com.android.federatedcompute.services
```

## Wrap Up
At this point now, you have a debloated, more privacy respecting, and faster Android TV device for under $25. If you want to go further, you can enhance your experience with a few additional apps.

| Name                                                                                                                                | Price          | Comment                                                                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Notifications for Android TV](https://play.google.com/store/apps/details?id=de.cyberdream.androidtv.notifications.google&hl=en-US) | Free           | Pair with Home Assistant to send things like door bell notifications to your TV.                                                                                                                        |
| [tvQuickActions Pro](https://play.google.com/store/apps/details?id=dev.vodik7.tvquickactions&hl=en-US)                              | $3.99          | **ESSENTIAL** Allows for button remapping to quickly navigate your TV, control your smart home, change apps, and dim your display.                                                                      |
| [Stremio](https://play.google.com/store/apps/details?id=com.stremio.one&hl=en-US)                                                   | Free           | Best client for sea-streaming 🏴‍☠️                                                                                                                                                        |
| [Jellyfin](https://play.google.com/store/apps/details?id=org.jellyfin.androidtv&hl=en-US)                                           | Free           | View content on your local Jellyfin server.                                                                             |
| [SmartTube](https://github.com/yuliskov/smarttube)                                                                                  | Free           | YouTube with Adblock, SponsorBlock, and many more QoL features.                                                                                                                       |
| [ProtonVPN](https://play.google.com/store/apps/details?id=ch.protonvpn.android&hl=en-US)                                            | $12.99 / month | VPN for bypassing geo-restrictions. ExpressVPN is another great option. |