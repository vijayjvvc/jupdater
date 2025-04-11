# 📦 JUpdater - Smart Android Update Checker Library

`JUpdater` is a **lightweight, developer-friendly Android library** to detect new app updates and trigger an elegant in-app update prompt. Whether you're hosting your app update data on your own server or relying on third-party sources like APKPure, `JUpdater` gives you the flexibility to control everything.

---

## 🚀 Features

- 🔍 **Auto-detect updates** based on `versionCode`.
- 🎯 **Custom threshold** for optional vs. forced updates.
- 🧩 **Customizable update UI** — BottomSheetDialog or Fullscreen blocking screen.
- 🌐 **Custom backend server support** — you define the update logic.
- 🛡️ **R8 / ProGuard safe** (rules are pre-packaged in the `.aar`).
- ☁️ Internet permission hint included.
- 📜 Zero configuration required for Play Store/third-party-based versioning.

---

## 📥 Download & Setup (Manual)

Since this library is **not published on Maven or Gradle repositories**, follow these steps to manually integrate the `.aar`:

### 1. Download AAR

👉 [**Click here to download JUpdater.aar**](./path-to-your-aar/JUpdater.aar)

### 2. Create `libs` folder

Place the downloaded `.aar` file inside your app module:

```
YourProject/
└── app/
    └── libs/
        └── JUpdater.aar
```

### 3. Modify your `build.gradle`

#### For Groovy (build.gradle)

```groovy
repositories {
    flatDir {
        dirs 'libs'
    }
}

dependencies {
    implementation(name: 'JUpdater', ext: 'aar')
}
```

#### For Kotlin DSL (build.gradle.kts)

```kotlin
dependencyResolutionManagement {
    repositories {
        flatDir {
            dirs("libs")
        }
    }
}

dependencies {
    implementation(name = "JUpdater", ext = "aar")
}
```

---

## 🛠️ Basic Usage

### Kotlin (Recommended)

```kotlin
val config = JUpdaterConfig("https://your-latest-apk-url.com")
config.enableForceUpdate(5)
JUpdater.getInstance().init(applicationContext, config)
JUpdater.getInstance().setDebugLog(true) // Optional
```

### Java

```java
JUpdaterConfig config = new JUpdaterConfig("https://your-latest-apk-url.com");
config.enableForceUpdate(5);
JUpdater.getInstance().init(getApplicationContext(), config);
JUpdater.getInstance().setDebugLog(true); // Optional
```

> ✅ Replace the URL with the **direct APK download link** of your latest version.

---

## ⚙️ Optional Features & Extensions

### 1. Custom Server URL

This is optional and only needed if you want full control via your own backend.

#### Kotlin

```kotlin
config.setCustomServerUrl("https://yourserver.com/check")
```

#### Java

```java
config.setCustomServerUrl("https://yourserver.com/check");
```

### 📡 Required Custom Server Response:

If you're using a custom server URL, make sure your backend responds with:

```json
{
  "pid": "com.your.package",
  "version_code": 9
}
```

It must support GET requests with query parameters:

```
/check?pid=com.example.app&version_code=7
```

> ❗ Incorrect or missing fields will fail the update check silently.

---

## 📊 Threshold Behavior Explained

### 🔸 Threshold Value (Default: 5)

- Defines the version gap between current and available version.
- If the gap is **within** threshold → show **BottomSheetDialog** (optional update).
- If the gap **exceeds** threshold → show **Fullscreen Activity** (forced update).
- Forced update = App will not work without updating.

#### Kotlin & Java

```kotlin
config.enableForceUpdate(threshold = 3)
```

```java
config.enableForceUpdate(3);
```

---

## 🔍 Debug Logs (Recommended in Development)

While developing, it is strongly recommended to enable debug logs:

```kotlin
JUpdater.getInstance().setDebugLog(true)
```

In production, either avoid calling this or set:

```kotlin
JUpdater.getInstance().setDebugLog(false)
```

By default, logs are suppressed in production mode.

---

## 🎨 UI Types

| Component        | Type            | Behavior                             |
| ---------------- | --------------- | ------------------------------------ |
| Bottom Sheet     | Optional Update | Non-blocking, user can skip          |
| Fullscreen Alert | Forced Update   | Blocking screen, update is mandatory |

---

## ✅ Best Practices

- Call `init(...)` early in app lifecycle (preferably in `Application`).
- Always update `versionCode` in your build config.
- Enable debug logs during development.
- Avoid misuse of custom URL without proper response.

---

## 📦 Dependencies Used

| Library             | Purpose                         |
| ------------------- | ------------------------------- |
| Material Components | UI for dialogs & sheets         |
| AndroidX AppCompat  | Compatibility support libraries |

> 🔐 No need to add ProGuard/R8 rules. They are already built into the AAR.

---

## 🧾 Permissions Required

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

> Must be declared in your `AndroidManifest.xml` manually.

---

## 🤝 Contributions & Custom Builds

We welcome suggestions! If you:

- 🔧 Want to contribute features — you may request access to the source code (granted selectively based on proposal).
- 🛠 Need a **custom build for your organization** (internal URL, security, analytics, etc.) — reach out to us. We'll provide a private AAR tailored to your setup.

---

## 📄 License

MIT License © 2025 QTLWS

---

> 📢 **Don't forget to ⭐ star this project if you find it useful!**

