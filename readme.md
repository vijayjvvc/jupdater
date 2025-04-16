
![logoJUpdater](https://github.com/user-attachments/assets/34f1a89c-cc1c-40c8-bc9d-01703bbb576f)

# 📦 JUpdater - Smart Android Update Checker Library

`JUpdater` is a **lightweight, developer-friendly Android library** to detect new app updates and trigger an elegant in-app update prompt. Whether you're hosting your app update data on your own server or relying on third-party sources like APKPure, `JUpdater` gives you the flexibility to control everything.

---

## 🌍 Works with Any App Distribution Platform

Unlike the **Play Core library**, which only works with apps distributed via the **Google Play Store** and relies on **Google Play Services**, `JUpdater` is fully **platform-independent** and doesn’t rely on any specific store or proprietary SDK.

Whether your app is available on:

- ✅ Google Play Store
- ✅ Xiaomi GetApps
- ✅ Samsung Galaxy Store
- ✅ Huawei AppGallery
- ✅ Aptoide, APKPure, Softonic, or other APK-sharing sites
- ✅ Or even just hosted on your **own website or file server**

Your users will **still receive update notifications**.

---

### 🚫 No More Store-Specific Dependencies

If you publish your app on a specific store (like **Play Store**, **Xiaomi**, or **Samsung Store**), you're often required to integrate their **specific SDKs** just to enable update checking and force updates. These SDKs are platform-dependent and often increase your app size unnecessarily.

Moreover, using multiple store SDKs can make your APK:

- Heavier to download and install
- More resource-intensive on the device
- Potentially slower due to background server checks

With `JUpdater`, **you don’t need**:

- Google Play Services
- Xiaomi’s proprietary update libraries
- Samsung’s Galaxy Store SDK
- Or any third-party store APIs

---

### 🆓 Designed for Indie Developers & Lightweight Builds

Making developer accounts and maintaining separate builds for different app stores can be time-consuming and complex. In addition, integrating different SDKs for each store just to check for updates often bloats your app.

That’s why `JUpdater` is a perfect choice for:

- 💻 **Indie developers** building and sharing apps for free
- 🚀 **Startups** testing MVPs or distributing beta builds
- 🎓 **Students and hobbyists** showcasing their apps
- 🧪 **Internal tools** or private app deployments
- 🌐 Developers distributing APKs from their **own website** or file hosting platforms

---

### 📦 Self-hosted Updates Made Easy

If you’re hosting your app updates on your own server or website, you don’t have access to platform-level update checks or push notifications. There’s **no default way** to notify users of a new version.

That’s where `JUpdater` comes in. It becomes your custom, flexible, and lightweight update handler:

- Define your version info in a simple API response or file
- No extra store involvement
- Every time your user opens the app, they get the latest version check
- Show update dialogs or force updates — **your call**

---

### 🔐 Privacy First — No Data Collection

Your privacy and your users' privacy are **top priorities**. `JUpdater` does **not collect any personal data**, analytics, or background telemetry. 

- No tracking
- No user fingerprinting
- No crash reporting
- No third-party data sharing

We provide a simple update service — nothing more, nothing less. You stay fully in control of your data and your users’ experience.

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

👉 [**Click here to download JUpdater.aar**](https://github.com/vijayjvvc/jupdater/raw/refs/heads/main/v1.0.1/jupdater-V1.0.1-release.aar)

### 2. Create `libs` folder

Place the downloaded `.aar` file inside your app module:

```
YourProject/
└── app/
    └── libs/
        └── jupdater-v1.0.1.aar
```

> 📁 If you have multiple `.aar` or `.jar` files, use the `fileTree` method instead:

```Groovy
dependencies {
    implementation fileTree(dir: "libs", include: ["*.aar", "*.jar"])
}
```

```Kotlin
dependencies {
    implementation(fileTree(mapOf("dir" to "libs", "include" to listOf("*.jar", "*.aar"))))
}
```

### 3. Add repositories (Top-level `settings.gradle` or `settings.gradle.kts`)

#### Groovy DSL:
```groovy
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        flatDir {
            dirs 'libs'
        }
    }
}
```

#### Kotlin DSL:
```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        flatDir {
            dirs("libs")
        }
    }
}
```

### 4. Add dependency (App-level `build.gradle` or `build.gradle.kts`)

#### Groovy:
```groovy
dependencies {
      implementation files('libs/jupdater-v1.0.1.aar')
}
```

#### Kotlin:
```kotlin
dependencies {
    implementation(files("libs/jupdater-v1.0.1.aar"))
}
```

---

## 🛠️ Basic Usage

### Kotlin (Recommended)

```kotlin
val config = JUpdaterConfig("https://your-latest-apk-url.com","https://yourserver.com/check")
config.enableForceUpdate(5)  // Optional
config.isDebug = true        // Optional
JUpdater.getInstance().init(applicationContext, config)
```

### Java

```java
JUpdaterConfig config = new JUpdaterConfig("https://your-latest-apk-url.com","https://yourserver.com/check");
config.enableForceUpdate(5);  // Optional
config.setDebug(true);        // Optional
JUpdater.getInstance().init(getApplicationContext(), config);
```

> ✅ Replace the URL with the **direct APK download link** of your latest version.
> ✅ Replace the Server URL with the **Jupdater API link** provided by us.

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
#### 🔸 Note :- Value less then 1 will not be accepted

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
config.setDebug(true)
```

In production, either avoid calling this or set:

```kotlin
config.setDebug(false)
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

## 🧠 Version History

| Version | Description |
|--------|-------------|
| [v1.0.1](https://github.com/vijayjvvc/jupdater/raw/refs/heads/main/v1.0.1/jupdater-V1.0.1-release.aar) | **Updates:**<br>• Server URL is now required (your API or ours)<br>• Threshold values less than 1 are no longer accepted<br>• Minor bug fixes |
| [v1.0.0](https://github.com/vijayjvvc/jupdater/raw/refs/heads/main/v1.0.0/jupdater-V1.0.0-release.aar) | **Initial release:**<br>• Base functionality implemented |


> 🆕 New versions will be updated here along with changelogs.

---

### ✅ Summary

With `JUpdater`:

- You stay **independent of app stores**
- Your app can be **freely distributed** from any source
- Users always get notified when updates are available
- You avoid bloat by skipping heavy SDK integrations
- You retain **full privacy** and **zero tracking**
- Your project remains **lightweight, fast, and developer-first**

> 🛠️ JUpdater is built by devs for devs — especially those who want full control of their distribution and update strategy. No gatekeepers, no vendor lock-in, no data collection.


## 🌐 Official Documentation & Contact

📘 Visit our [official](https://vijaydevportfolio.onrender.com/) docs page. 

📞 Want your app to be listed on our update server?
- Contact us through our official site
- You'll find 📧 email, ☎️ phone, and 🔗 LinkedIn there

> For better visibility and update delivery, registration is recommended.

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
