# Android SDK Version Config

Google Play requires apps to **target Android 16 (API level 36)** for all
new apps and updates as of August 31, 2026. Note: bumping this is a
one-line Gradle change, but Android 16 also enforces new adaptive-layout
rules for large screens (600dp+) — orientation/aspect-ratio locking and
edge-to-edge opt-outs are removed at this target level, so test on a
tablet-sized emulator if you plan to lock portrait-only.

This app is configured to run on **Android 9.0 (API 28) and above**.

## Where this is already configured

The `android/` folder in this project already has these values set in
`android/app/build.gradle` — this doc explains what's there and why, in
case you need to change them later (e.g. a plugin requires a higher
`minSdk`, or Google Play raises its `targetSdk` requirement again).

```groovy
android {
    compileSdk 36

    defaultConfig {
        applicationId "com.rtoexpert.rto_expert"
        minSdk 28            // Android 9.0 (Pie) and above
        targetSdk 36
        versionCode 1
        versionName "1.0.0"
    }
}
```

If using Kotlin DSL (`build.gradle.kts`):

```kotlin
android {
    compileSdk = 36

    defaultConfig {
        applicationId = "com.rtoexpert.rto_expert"
        minSdk = 28
        targetSdk = 36
        versionCode = 1
        versionName = "1.0.0"
    }
}
```

## Why minSdk 28

You've chosen Android 9.0 (API 28, released 2018) as the floor. This
still covers the large majority of active Android devices in India as
of 2026, while letting you skip a few years of pre-2018 compatibility
shims and older permission-model workarounds (e.g. simpler runtime
storage/location permission handling than API 24-27 required). The
trade-off: a small number of very old budget phones (2016-2018 models
still in use) won't be able to install the app. If you later find your
target users skew toward older hardware, this is a one-line change
back down to `minSdk 24`.

## Checking a plugin's minimum supported SDK

If a future `flutter pub add <package>` build fails with a "requires
minSdkVersion X" Gradle error, that plugin's floor is higher than 28 —
raise `minSdk` in `build.gradle` to match, rather than pinning an old
plugin version.
