# Yesmin AI (Android, Kotlin + Jetpack Compose)

Futuristic Hindi/Hinglish voice assistant. Gemini brain, voice in/out, app launching, YouTube search, coding mode.

> **Status:** source code generated but NOT compiled or tested yet (the generating environment had no Gradle/Android SDK/internet). Expect to fix a small compile error or two on first build.

## Features
- Animated orb (idle/listening/thinking/speaking), waveform, chat + Coding mode, history screen
- Gemini via REST (`generateContent`), model/key/creativity/length settings, connection test, retry on errors
- SpeechRecognizer (hi-IN / en-IN) + TextToSpeech, stop button, mic permission explanation
- Open installed apps via intents (ambiguous matches ask you to choose), Settings screens, YouTube / YouTube Music search (browser fallback)
- Code blocks with Copy and Save (saved in app storage `files/snippets/`)
- API key in EncryptedSharedPreferences, masked in UI, never logged

## Limitations
- No always-on listening / wake word, no AccessibilityService, no auto-clicking or sending messages
- History is stored as JSON (not Room); no editable app-shortcut list (4 fixed quick actions)
- Quick-command replies are always Hinglish; AI replies follow the Language setting
- Generated code is never executed

## Setup
1. Get a Gemini API key from Google AI Studio.
2. Install the app, open Settings → AI Settings, paste key → Save key → Test connection.

## Build option A: GitHub Actions (recommended)
Push this folder to a GitHub repo → Actions → "Build debug APK" → download artifact `yesmin-debug-apk` → install on phone (allow "install unknown apps").

## Build option B: Termux (best effort)
The Android Gradle Plugin downloads an x86 `aapt2` that does not run on ARM Termux, and the Android SDK is not in Termux's default repos.
```
pkg update && pkg install openjdk-17 gradle aapt2 unzip
# Android SDK: you need platforms/android-34 and build-tools 34.x (community Termux SDK installers).
export ANDROID_HOME=$HOME/android-sdk
echo "sdk.dir=$ANDROID_HOME" > local.properties
gradle wrapper --gradle-version 8.7        # creates gradlew
./gradlew assembleDebug -Pandroid.aapt2FromMavenOverride=$PREFIX/bin/aapt2
```
APK: `app/build/outputs/apk/debug/app-debug.apk`. Copy: `termux-setup-storage && cp app/build/outputs/apk/debug/app-debug.apk ~/storage/downloads/`, then open from Files app.
If the SDK/aapt2 setup fails, use option A.

## Troubleshooting
- `SDK location not found` → create `local.properties` with `sdk.dir=...`
- `aapt2` errors on Termux → use the override flag above
- Out of memory → lower `org.gradle.jvmargs` in `gradle.properties`
- Model not found → change Model ID in Settings (names change over time)
- Unit tests: `./gradlew testDebugUnitTest`
