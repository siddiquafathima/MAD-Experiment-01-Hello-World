

https://github.com/user-attachments/assets/95560a40-1dc9-4b64-bdc2-ded642502400

# Experiment 2 — Activity Lifecycle

## 🎯 Aim
To develop an Android application using Kotlin to understand and demonstrate the different Activity Lifecycle methods in Android.

---

## 🛠️ Procedure
1. Create a new Android application using Android Studio and Kotlin.
2. Create the main Activity and design a simple user interface.
3. Override the Activity lifecycle methods such as `onCreate()`, `onStart()`, `onResume()`, `onPause()`, `onStop()`, and `onDestroy()`.
4. Add log messages inside each lifecycle method to identify when the method is called.
5. Run the application on the Android Emulator.
6. Perform actions such as opening, minimizing, and closing the application to observe the lifecycle changes.

---

## 💻 Main Code Block

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    Log.d("ActivityLifecycle", "onCreate called")
}

override fun onStart() {
    super.onStart()
    Log.d("ActivityLifecycle", "onStart called")
}

override fun onResume() {
    super.onResume()
    Log.d("ActivityLifecycle", "onResume called")
}

override fun onPause() {
    super.onPause()
    Log.d("ActivityLifecycle", "onPause called")
}

override fun onStop() {
    super.onStop()
    Log.d("ActivityLifecycle", "onStop called")
}

override fun onDestroy() {
    super.onDestroy()
    Log.d("ActivityLifecycle", "onDestroy called")
}
```

---

## 💡 Explanation
The Activity Lifecycle represents the different states an Android Activity goes through during its lifetime. Lifecycle callback methods such as `onCreate()`, `onStart()`, `onResume()`, `onPause()`, `onStop()`, and `onDestroy()` are called at different stages of the Activity's execution. `Log.d()` is used to display messages in Logcat to observe the lifecycle events.

---

## 🎨 Wireframe / UI Design

```text
┌─────────────────────────┐
│   Activity Lifecycle    │
│                         │
│   Activity Lifecycle    │
│       Demo App          │
│                         │
│   Check Logcat to view  │
│   lifecycle events      │
└─────────────────────────┘
```

---

## 📹 Demo / Recording


---

## 📊 Result
The Android application was developed successfully using Kotlin to demonstrate the Activity Lifecycle methods. The lifecycle events were observed using Logcat.
