<img width="1920" height="1200" alt="login_screen" src="https://github.com/user-attachments/assets/e1ba659c-bf62-4732-96eb-bb9ed09b575f" />
<img width="1920" height="1200" alt="Screenshot 2026-08-06 223546" src="https://github.com/user-attachments/assets/ca0d171e-f736-42ff-98c9-4220a386bf57" />
# MAD Experiment: Login Intent and Notification

Mobile Application Development Lab experiment demonstrating explicit screen navigation using Intents and status bar alerts via NotificationManager in Kotlin.

---

## Aim
To develop an Android application using Kotlin and Android Studio that performs user authentication navigation across activities using explicit **Intents** and creates interactive **Status Bar Notifications** using `NotificationCompat.Builder` and `NotificationChannel`.

---

## Procedure
1. **Design UI Layouts:** Create `activity_main.xml` with username/password inputs and a login button. Create `activity_second.xml` as the destination screen.
2. **Configure Permissions:** Add `POST_NOTIFICATIONS` permission in `AndroidManifest.xml` for Android 13+ compatibility.
3. **Register Notification Channel:** Build a channel with ID, name, and importance level required for API level 26+.
4. **Implement Navigation:** Set an `OnClickListener` on the login button to launch `SecondActivity` using an explicit `Intent`.
5. **Trigger Notification:** Construct and issue a notification using `NotificationCompat.Builder` and dispatch it through `NotificationManager`.

---

## Source Code

### 1. `MainActivity.kt`
```kotlin
package com.example.loginintentnotification

import android.app.NotificationChannel
import android.app.NotificationManager
import android.content.Context
import android.content.Intent
import android.content.pm.PackageManager
import android.os.Build
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.ActivityCompat
import androidx.core.app.NotificationCompat
import androidx.core.content.ContextCompat

class MainActivity : AppCompatActivity() {

    private val channelId = "login_channel"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        createNotificationChannel()
        requestNotificationPermission()

        val etUsername = findViewById<EditText>(R.id.etUsername)
        val btnLogin = findViewById<Button>(R.id.btnLogin)

        btnLogin.setOnClickListener {
            val username = etUsername.text.toString()
            sendNotification("Login Successful", "Welcome back, $username!")
            
            val intent = Intent(this, SecondActivity::class.java)
            startActivity(intent)
        }
    }

    private fun createNotificationChannel() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(
                channelId,
                "Login Notification",
                NotificationManager.IMPORTANCE_DEFAULT
            ).apply {
                description = "Channel for login status notifications"
            }
            val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
            manager.createNotificationChannel(channel)
        }
    }

    private fun requestNotificationPermission() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
            if (ContextCompat.checkSelfPermission(this, android.Manifest.permission.POST_NOTIFICATIONS)
                != PackageManager.PERMISSION_GRANTED) {
                ActivityCompat.requestPermissions(
                    this,
                    arrayOf(android.Manifest.permission.POST_NOTIFICATIONS),
                    101
                )
            }
        }
    }

    private fun sendNotification(title: String, message: String) {
        val builder = NotificationCompat.Builder(this, channelId)
            .setSmallIcon(android.R.drawable.ic_dialog_info)
            .setContentTitle(title)
            .setContentText(message)
            .setPriority(NotificationCompat.PRIORITY_DEFAULT)
            .setAutoCancel(true)

        val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        manager.notify(1, builder.build())
    }
}
