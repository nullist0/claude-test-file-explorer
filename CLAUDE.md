# CLAUDE.md

This file provides guidance for Claude Code when working with this Android file explorer app repository.

## Project Overview

An Android file explorer application built with Kotlin and Gradle. The app allows users to browse, navigate, and manage files and directories on their Android device.

## Build System

This project uses **Gradle** with the Android Gradle Plugin.

> **Note:** The commands below use the Gradle wrapper (`./gradlew`). The wrapper scripts (`gradlew`, `gradlew.bat`, `gradle/wrapper/`) will be added to the repository as part of the initial project setup. Until then, use a locally installed Gradle or Android Studio to run these tasks.

### Common Commands

```bash
# Build debug APK
./gradlew assembleDebug

# Build release APK
./gradlew assembleRelease

# Run unit tests
./gradlew test

# Run instrumented tests (requires connected device/emulator)
./gradlew connectedAndroidTest

# Run lint checks
./gradlew lint

# Clean build artifacts
./gradlew clean

# Build and install debug APK on connected device
./gradlew installDebug
```

## Project Structure

> **Note:** The structure below reflects the intended layout once the Android project is initialized. The repository is currently in the early setup phase and does not yet contain these files.

```text
app/
  src/
    main/
      java/           # Kotlin/Java source files
      res/            # Android resources (layouts, drawables, strings, etc.)
      AndroidManifest.xml
    test/             # Unit tests
    androidTest/      # Instrumented tests
  build.gradle.kts    # App-level build config
build.gradle.kts      # Project-level build config
settings.gradle.kts   # Gradle settings
```

## Development Guidelines

### Code Style
- Use Kotlin as the primary language
- Follow [Android Kotlin Style Guide](https://developer.android.com/kotlin/style-guide)
- Use `ktlint` or `detekt` if configured for linting

### Architecture
- Prefer MVVM (Model-View-ViewModel) architecture
- Use Android Jetpack components (ViewModel, LiveData/StateFlow, Room if needed)
- Keep business logic out of Activities/Fragments

### File System Access

Android storage permissions vary significantly by API level:

- **Android 12 and below**: Request `READ_EXTERNAL_STORAGE` for reading files; `WRITE_EXTERNAL_STORAGE` is deprecated on API 29+ and ignored on API 33+
- **Android 13+ (API 33+)**: Use granular media permissions — `READ_MEDIA_IMAGES`, `READ_MEDIA_VIDEO`, `READ_MEDIA_AUDIO` — instead of `READ_EXTERNAL_STORAGE`
- **All versions**: Prefer **Storage Access Framework (SAF)** (`DocumentsProvider`, `ACTION_OPEN_DOCUMENT_TREE`) and **MediaStore** APIs for broad file access — these work across all modern Android versions without special permissions
- **`MANAGE_EXTERNAL_STORAGE`** (Android 11+, API 30+): Grants "All files access" but is heavily restricted:
  - Requires a runtime prompt via `ACTION_MANAGE_APP_ALL_FILES_ACCESS_PERMISSION`
  - Subject to Google Play policy review — only approved for apps that qualify as file managers or backup tools
  - **Do not use** unless the app genuinely requires unrestricted file system access
- Always handle `SecurityException` when accessing restricted paths

### Testing
- Write unit tests for ViewModels, use cases, and utility functions
- Use `MockK` or `Mockito` for mocking
- Write instrumented tests for UI flows using Espresso or Compose Test

## Key Permissions

Declare permissions in `AndroidManifest.xml` based on the target API level:

```xml
<!-- For reading files on Android 12 and below -->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"
    android:maxSdkVersion="32" />

<!-- For granular media access on Android 13+ -->
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
<uses-permission android:name="android.permission.READ_MEDIA_AUDIO" />

<!-- Only if the app qualifies as a file manager under Play policy -->
<uses-permission android:name="android.permission.MANAGE_EXTERNAL_STORAGE" />
```

## Notes

- `local.properties` is not tracked — it contains the local Android SDK path (`sdk.dir`)
- Keystore files (`*.jks`, `*.keystore`) are excluded from version control
- `google-services.json` is excluded — add manually if Firebase is used
