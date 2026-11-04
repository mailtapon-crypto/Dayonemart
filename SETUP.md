# Project Setup

## Required Configuration Files

Before building the app, you need to create the following configuration files.

> ⚠️ **These files contain sensitive credentials and are gitignored. Never commit them!**

---

## 1. Firebase Configuration

### Android (`google-services.json`)

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create or select your project
3. Go to **Project Settings** > **General**
4. Under "Your apps", click **Add app** > **Android**
5. Register your app with package name: `com.dayonesoft.onemartcustomer`
6. Download `google-services.json`
7. Place it in: `android/app/google-services.json`

### iOS (`GoogleService-Info.plist`)

1. In Firebase Console, add an iOS app
2. Register with bundle ID: `com.dayonesoft.onemartcustomer`
3. Download `GoogleService-Info.plist`
4. Place it in: `ios/Runner/GoogleService-Info.plist`

### Flutter Options (`firebase_options.dart`)

Generate this file automatically:

```bash
# Install FlutterFire CLI (if not already installed)
dart pub global activate flutterfire_cli

# Login to Firebase
firebase login

# Generate firebase_options.dart
flutterfire configure
```

---

## 2. Android Secrets (`secrets.xml`)

This file contains Google Maps API key and Facebook credentials.

### Steps:

1. Copy the template from the project root:
   ```bash
   cp secrets.xml.example android/app/src/main/res/values/secrets.xml
   ```

2. Edit `secrets.xml` and fill in your real values:

   | Key | Where to get it |
   |-----|-----------------|
   | `google_maps_api_key` | [Google Cloud Console](https://console.cloud.google.com/apis/credentials) |
   | `facebook_app_id` | [Facebook Developers](https://developers.facebook.com/apps) |
   | `facebook_client_token` | Facebook App Dashboard > Settings > Advanced |

### Example `secrets.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="google_maps_api_key">AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</string>
    <string name="facebook_app_id">1234567890123456</string>
    <string name="fb_login_protocol_scheme">fb1234567890123456</string>
    <string name="facebook_client_token">abcdef1234567890abcdef1234567890</string>
</resources>
```

## 3. API Base URL

**Change the API base URL**
All API requests point to the `baseUrl` defined in `lib/config/util/app_constants.dart`. Currently it is set to the demo/developer's server:

```dart
static const String baseUrl = 'https://your-server.com';
```

> ⚠️ **You MUST change this to your own backend URL** (e.g., `https://your-server.com`) before running the app in production. Failure to do so will cause the app to send requests to the wrong server.

---

## 4. Build the App

After setting up all configuration files and setting the correct API base URL:

```bash
# Get dependencies
flutter pub get

# Run the app
flutter run
```

---

## 5. Secure Storage

Auth tokens are stored via `flutter_secure_storage` (Android Keystore / iOS Keychain) instead of `SharedPreferences`.

### Android

- `minSdkVersion` must be **23 or higher**. Already configured in `android/app/build.gradle.kts` as `minSdk = maxOf(23, flutter.minSdkVersion)`.
- `AndroidManifest.xml` sets `android:allowBackup="false"` and `android:fullBackupContent="false"` to prevent decryption errors after auto-restore.

### iOS

- iOS deployment target must be **12.0 or higher** (already satisfied by the current `Podfile`).
- The **Keychain Sharing** capability is enabled via `ios/Runner/Runner.entitlements`. If you regenerate the Xcode project, re-add it: Xcode → Runner target → Signing & Capabilities → **+ Capability → Keychain Sharing**.

---

## 6. Crash Reporting

- **Debug builds**: errors print to the console (Flutter default). No remote reporting.
- **Release builds**: uncaught errors are reported to Firebase Crashlytics automatically, via `FlutterError.onError` and `PlatformDispatcher.instance.onError` in `lib/main.dart`.

### Setup

1. Enable Crashlytics in the Firebase Console for the project configured by `flutterfire configure`.
2. **Android symbols** are uploaded automatically by the Crashlytics Gradle plugin (already wired in `android/app/build.gradle.kts`).
3. **iOS dSYMs** upload via the standard Crashlytics run-script build phase in Xcode.
4. To switch crash destination (e.g., Sentry), replace the two handlers in `lib/main.dart`.

---

## Troubleshooting

### "google-services.json not found"
Make sure you've downloaded and placed the file in `android/app/google-services.json`

### "Cannot resolve symbol '@string/google_maps_api_key'"
Create `secrets.xml` from the template as described above.

### Firebase initialization failed
Run `flutterfire configure` to regenerate `firebase_options.dart`
