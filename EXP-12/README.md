# MAD Experiment 12: Access Web Services Using HTTP Request

A Mobile Application Development lab experiment demonstrating asynchronous web service consumption using HTTP/HTTPS requests, Kotlin Coroutines, `Dispatchers.IO`, and JSON parsing in Android.

---

## 1. Aim

To develop an Android application that sends an HTTP GET request to a RESTful web service, retrieves JSON data, parses the response, and displays the user details on the screen using Kotlin Coroutines.

## 2. Concept & Technology Overview

- **RESTful Web Services:** Allow applications to exchange data using HTTP methods such as GET.
- **HTTP GET Request:** Retrieves information from a remote server.
- **Kotlin Coroutines:** Perform network operations asynchronously without blocking the main UI thread.
- **Dispatchers.IO:** Executes network operations on a background thread.
- **Dispatchers.Main:** Updates the user interface on the main thread.
- **JSON Parsing:** Uses `JSONObject` to extract information from the JSON response.
- **Error Handling:** Displays an error message if the network request fails.

### Scenario

The application fetches user details from a remote REST API and displays the user's name, email address, and city.

**API endpoint:** [JSONPlaceholder – User 1](https://jsonplaceholder.typicode.com/users/1)

## 3. Technologies Used

- Language: Kotlin
- IDE: Android Studio
- Networking: Java URL API
- Asynchronous Processing: Kotlin Coroutines
- Data Format: JSON
- API: JSONPlaceholder
- UI: Android XML Layouts

## 4. Project File & Folder Structure

```text
WebServiceHttpApp/
├── app/
│   ├── src/
│   │   └── main/
│   │       ├── AndroidManifest.xml
│   │       ├── java/com/example/webservicehttpapp/
│   │       │   └── MainActivity.kt
│   │       └── res/
│   │           └── layout/
│   │               └── activity_main.xml
│   └── build.gradle.kts
├── build.gradle.kts
├── settings.gradle.kts
└── README.md
```

### File Descriptions

- `AndroidManifest.xml` — Declares internet access permission.
- `MainActivity.kt` — Executes the HTTP request, parses JSON, and displays the result.
- `activity_main.xml` — Defines the fetch button and result display area.
- `build.gradle.kts` — Configures the required dependencies.

## 5. Main Source Code

### 5.1 `AndroidManifest.xml`

Add the internet permission above the `<application>` element.

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### 5.2 `MainActivity.kt`

```kotlin
package com.example.webservicehttpapp

import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import org.json.JSONObject
import java.net.URL

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val btnFetch = findViewById<Button>(R.id.btnFetch)
        val tvResult = findViewById<TextView>(R.id.tvResult)

        btnFetch.setOnClickListener {
            CoroutineScope(Dispatchers.IO).launch {
                try {
                    val rawJson = URL(
                        "https://jsonplaceholder.typicode.com/users/1"
                    ).readText()

                    val json = JSONObject(rawJson)
                    val address = json.getJSONObject("address")

                    val result = """
                        Name: ${json.getString("name")}
                        Email: ${json.getString("email")}
                        City: ${address.getString("city")}
                    """.trimIndent()

                    withContext(Dispatchers.Main) {
                        tvResult.text = result
                    }
                } catch (e: Exception) {
                    withContext(Dispatchers.Main) {
                        tvResult.text = "Error: ${e.message}"
                    }
                }
            }
        }
    }
}
```

### 5.3 Required Dependency

Add the following dependency to the app-level `build.gradle.kts` if it is not already included:

```kotlin
implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.10.2")
```

## 6. Test Cases & Execution Verification

### Test Case 1: Initial Application Screen

- **Action:** Launch the application on an Android emulator or physical device.
- **Expected Output:** The application displays the fetch button and the initial result area.

### Test Case 2: HTTP GET Request and JSON Parsing

- **Action:** Tap **Fetch Web Data** while internet connectivity is available.
- **Expected Output:** The application fetches the JSON response and displays the user's name, email, and city.

### Test Case 3: Network Error Handling

- **Action:** Disable Wi-Fi/mobile data and tap **Fetch Web Data**.
- **Expected Output:** The application displays an error message instead of crashing. The exact message depends on the network error.

## 7. Screenshots

### Screenshot 1: Initial Application Interface

![MAD Experiment 12 - Initial Application Interface](https://github.com/user-attachments/assets/de32ae59-48d4-4a27-a185-25f75810ee41)

### Screenshot 2: Web Service Response

![MAD Experiment 12 - Fetched Web Service Data](https://github.com/user-attachments/assets/c452d825-1e1b-42a9-acdc-67065fef9412)

## 8. Result

The Android application was developed using Kotlin Coroutines to perform an asynchronous HTTP GET request to a RESTful web service. The JSON response was parsed using `JSONObject`, and the retrieved information was displayed on the user interface. Network errors were handled without blocking the main UI thread.

