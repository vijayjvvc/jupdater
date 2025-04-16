
![logoJUpdater](https://github.com/user-attachments/assets/34f1a89c-cc1c-40c8-bc9d-01703bbb576f)

# 📦 JUpdater - Smart Android Update Checker Library

`JUpdater` is a **lightweight, developer-friendly Android library** to detect new app updates and trigger an elegant in-app update prompt. Whether you're hosting your app update data on your own server or relying on third-party sources like APKPure, `JUpdater` gives you the flexibility to control everything.

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



## ✅ Best Practices

- Call `init(...)` early in app lifecycle (preferably in `Application`).
- Always update `versionCode` in your build config.
- Enable debug logs during development.
- Avoid misuse of custom URL without proper response.

---

## 🧾 Permissions Required

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

> Must be declared in your `AndroidManifest.xml` manually.

---

## 📄 License

MIT License © 2025 QTLWS

---

> 📢 **Don't forget to ⭐ star this project if you find it useful!**
