# MAD Experiment 08: Implementing Options Menu and WebView in Android

A Mobile Application Development lab experiment demonstrating Android Options Menus and an embedded browser using `WebView` and `WebViewClient` in Kotlin.

---

## 1. Aim

To develop an Android application using Kotlin that implements an Options Menu and displays web pages within the application using WebView.

## 2. Concept and Technology Overview

- **Options Menu:** Displays menu items that allow users to perform actions within an Android application.
- **WebView:** Displays web pages inside the application without opening an external browser.
- **WebViewClient:** Handles web navigation within the WebView.
- **Menu Inflation:** Uses `onCreateOptionsMenu()` to display menu items defined in an XML resource.
- **Menu Selection:** Uses `onOptionsItemSelected()` to handle user selections.
- **Reload:** Refreshes the currently displayed webpage.
- **Toast:** Displays short messages to provide feedback to the user.

### Scenario

The application acts as a simple academic web portal browser. Users can open Google and GitHub, reload the current webpage, and view student information through the Options Menu.

---

## 3. Project File and Folder Structure

```text
MenusWebViewApp/
├── app/
│   ├── manifests/
│   │   └── AndroidManifest.xml
│   ├── java/
│   │   └── com/example/menuswebviewapp/
│   │       └── MainActivity.kt
│   └── res/
│       ├── layout/
│       │   └── activity_main.xml
│       └── menu/
│           └── main_menu.xml
├── build.gradle.kts
└── README.md
```

### File Description

- `MainActivity.kt` — Handles menu actions and WebView navigation.
- `activity_main.xml` — Defines the student information header and WebView.
- `main_menu.xml` — Defines the Options Menu items.
- `AndroidManifest.xml` — Declares the required Internet permission and application configuration.

---

## 4. Main Source Code

### MainActivity.kt

```kotlin
package com.example.menuswebviewapp

import android.annotation.SuppressLint
import android.os.Bundle
import android.view.Menu
import android.view.MenuItem
import android.webkit.WebView
import android.webkit.WebViewClient
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    private lateinit var webView: WebView

    @SuppressLint("SetJavaScriptEnabled")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val tvStudentUSN = findViewById<TextView>(R.id.tvStudentUSN)
        tvStudentUSN.text = "USN: 25MCAR0121"

        webView = findViewById(R.id.webView)
        webView.settings.javaScriptEnabled = true
        webView.webViewClient = WebViewClient()
        webView.loadUrl("https://www.google.com")
    }

    override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.main_menu, menu)
        return true
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return when (item.itemId) {
            R.id.menu_student_info -> {
                val studentInfo =
                    "Name: Siddiqua Fathima | USN: 25MCAR0121"

                Toast.makeText(
                    this,
                    studentInfo,
                    Toast.LENGTH_LONG
                ).show()
                true
            }

            R.id.menu_google -> {
                webView.loadUrl("https://www.google.com")
                Toast.makeText(
                    this,
                    "Navigating to Google",
                    Toast.LENGTH_SHORT
                ).show()
                true
            }

            R.id.menu_github -> {
                webView.loadUrl("https://github.com")
                Toast.makeText(
                    this,
                    "Navigating to GitHub",
                    Toast.LENGTH_SHORT
                ).show()
                true
            }

            R.id.menu_reload -> {
                webView.reload()
                Toast.makeText(
                    this,
                    "Reloading Page...",
                    Toast.LENGTH_SHORT
                ).show()
                true
            }

            else -> super.onOptionsItemSelected(item)
        }
    }

    @Deprecated("Deprecated in Java")
    override fun onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack()
        } else {
            super.onBackPressed()
        }
    }
}
```

**Important:** The code above assumes the IDs in your XML files match those used in the Kotlin code. Keep your actual student USN consistent throughout your project.

---

## 5. Internet Permission

Ensure that `AndroidManifest.xml` contains the Internet permission before the `<application>` element:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

This permission allows the WebView to access online webpages.

---

## 6. Test Cases and Execution Verification

### Test Case 1: Student Information

**Action:** Launch the application, open the three-dot Options Menu, and select **Student Info**.

**Expected Output:** A Toast displays the student's name and USN. The student verification header remains visible in the application.

**Screenshot 1:** Student information and Toast message.

### Test Case 2: WebView Navigation

**Action:** Open the Options Menu and select **GitHub**.

**Expected Output:** GitHub loads inside the application's WebView without redirecting to an external browser.

**Screenshot 2:** GitHub displayed inside the application.

### Test Case 3: Reload Webpage

**Action:** Open the Options Menu and select **Reload**.

**Expected Output:** The current webpage refreshes, and a Toast displays `Reloading Page...`.

**Screenshot 3:** Reload action and its Toast message.

---

## 7. Output Screenshots

### Screenshot 1: Student Information

![Student Information and Toast](https://github.com/user-attachments/assets/7a2a5270-fca6-48f8-a239-faf87a0ac388)

### Screenshot 2: WebView Navigation

![GitHub displayed in WebView](https://github.com/user-attachments/assets/19aa8165-4c4e-4991-9ec5-56477312d0a9)

### Screenshot 3: Reload Functionality

![WebView reload action](https://github.com/user-attachments/assets/6aae283e-092f-4c7e-b682-d9216b437606)

---

## 8. Result

The Android application demonstrating Options Menu handling and WebView integration was developed using Kotlin. The application supports webpage navigation, page reloading, and student information display through menu actions. The functionality was verified using the specified test cases.
