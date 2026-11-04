# DayOneMart - Single Vendor E-Commerce Management System Flutter Customer App

The DayOneMart Customer App is a feature-rich Flutter application designed for seamless online shopping, real-time delivery tracking, and account management. Built with modern Flutter architecture, it serves as the dedicated user-facing component for the DayOneMart single-vendor e-commerce management system.

## 🚀 Getting Started

> [!IMPORTANT]
> **Change the API base URL**
> All API requests point to the `baseUrl` defined in `lib/config/util/app_constants.dart`.
> Currently it is set to the demo/developer's server.
> You MUST change this to your own backend URL before running the app in production.
> Failure to do so will cause the app to send requests to the wrong server. More details in [SETUP.md](SETUP.md#3-api-base-url)

### 1. Install Dependencies

```sh
flutter pub get
```

### 2. Run the App

Make sure to open an Android emulator or iOS simulator or connect a physical device. You also need to select the Android device from the selector.

```sh
flutter run
```

### 3. Build Android APK

```sh
flutter build apk
```

## ⚙️ Development


### Generate Localization (l10n)

Regenerate localization files after updating `.arb` files:

```sh
flutter gen-l10n
```

### Build Generated Files

Run code generation for annotations (e.g., `@injectable`, `freezed`, `json_serializable`):

```sh
dart run build_runner build --delete-conflicting-outputs
```

### Continuous Build

Automatically rebuild generated files on changes:

```sh
dart run build_runner watch --delete-conflicting-outputs
```

## Creating Release Builds

### Android

App is already configured to automatically securely sign the release build if you provide a `key.properties` file. For more information, see the [official Flutter documentation](https://docs.flutter.dev/deployment/android#signing-the-app).

1. **Create a Keystore**:
   Create a release keystore using the following command (store it securely, you will need it for all future updates):
   ```sh
   keytool -genkey -v -keystore android/app/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
   ```

2. **Create `key.properties`**:
   Create a file named `key.properties` inside the `android/` directory (e.g. `android/key.properties`). Add the following content, replacing the values with your keystore details from Step 1:
   ```properties
   storePassword=<your-store-password>
   keyPassword=<your-key-password>
   keyAlias=upload
   storeFile=upload-keystore.jks
   ```

3. **Build release APK (with obfuscation – strongly recommended)**:
   ```sh
   flutter build apk --release --obfuscate --split-debug-info=build/app/outputs/symbols
   ```

4. **Or App Bundle (preferred for Play Store)**:
   ```sh
   flutter build appbundle --release --obfuscate --split-debug-info=build/app/outputs/symbols
   ```

**Note**: Always keep the keystore and the debug info files safe! They are needed for app updates and crash reporting (e.g., Firebase Crashlytics / Google Play Console).

## 🛠️ Useful Commands

* **Clean project cache**

  ```sh
  flutter clean
  flutter pub cache clean
  ```

---

## 📚 Resources

* [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
* [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)
* [Flutter Documentation](https://docs.flutter.dev/)
