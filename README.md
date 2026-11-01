# TV Text Bridge — Android phone + Xiaomi Mi Box

This is an Android Studio source project with two installable app modules:

- **Phone** (`:phone`): discovers a receiver on the same Wi-Fi and sends text.
- **TV** (`:tv`): displays received text and provides a custom Android keyboard (IME). When that keyboard is selected in a text field, its **PASTE TEXT** key inserts the received text into the focused app.

## Important behavior / Android limitation

Android does not let an ordinary app force text into another app's focused field. The supported route is an Android **Input Method (keyboard)**. Therefore:

1. Open **TV Text Receiver** on the Mi Box.
2. Keep its screen open while you want to receive text. The receiver only listens while this screen is visible; it does not run a background service.
3. On the phone, open **TV Text Sender**, tap **Find TV**, choose the Mi Box, type/paste text, then tap **SEND**.
4. On the TV, open the destination app and focus its text field.
5. Switch the on-screen keyboard to **TV Text Keyboard** (usually keyboard icon / long-press OK on the keyboard, then choose it).
6. Tap **PASTE TEXT** on the TV keyboard. The text is committed into the focused field.

The receiver app screen itself also shows the latest received text and has a **CLEAR** button. The paste action is intentionally on the custom keyboard because that is the Android-supported way to insert text into another app.

## Build

Open this folder in a recent Android Studio (Giraffe or newer), allow Gradle sync, then:

- Select `phone` configuration and Build > Build APK(s).
- Select `tv` configuration and Build > Build APK(s).

APK outputs are under each module's `build/outputs/apk/debug/`.

Suggested compatibility: Android 6.0+ (minSdk 23). Both devices must be on the same local Wi-Fi. Some guest Wi-Fi networks block device-to-device traffic; use a normal home LAN.

## Pairing / network

The receiver advertises `_tvtext._tcp.` using Android Network Service Discovery (NSD). The phone discovers that service; no IP address entry is needed. Text is sent as UTF-8 over a local TCP connection. There is no cloud account or external server.

## Security note

This prototype has no pairing PIN or encryption. Use it only on a trusted home Wi-Fi network and avoid sending passwords or other sensitive text until pairing/security is added.
