# CLAUDE.md

This file provides guidance for Claude Code when working with this Android file explorer app repository.

## Project Overview

An Android file explorer application built with Kotlin and Gradle. The app allows users to browse, navigate, and manage files and directories on their Android device.

## Build System

This project uses **Gradle** with the Android Gradle Plugin.

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

```
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
- Request `READ_EXTERNAL_STORAGE` / `WRITE_EXTERNAL_STORAGE` permissions appropriately
- On Android 10+, use scoped storage APIs
- On Android 11+, use `MANAGE_EXTERNAL_STORAGE` for broad file access if needed
- Handle `SecurityException` when accessing restricted paths

### Testing
- Write unit tests for ViewModels, use cases, and utility functions
- Use `MockK` or `Mockito` for mocking
- Write instrumented tests for UI flows using Espresso or Compose Test

## Key Permissions

The app likely requires these permissions in `AndroidManifest.xml`:
- `android.permission.READ_EXTERNAL_STORAGE`
- `android.permission.WRITE_EXTERNAL_STORAGE`
- `android.permission.MANAGE_EXTERNAL_STORAGE` (Android 11+, for full access)

## Notes

- `local.properties` is not tracked — it contains the local Android SDK path (`sdk.dir`)
- Keystore files (`*.jks`, `*.keystore`) are excluded from version control
- `google-services.json` is excluded — add manually if Firebase is used
