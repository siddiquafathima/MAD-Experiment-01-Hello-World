# 🍔 MAD Experiment 08: FoodOrder – Campus Cafeteria Application

A campus cafeteria ordering Android application developed in **Kotlin** using standard Android Views, **Intent navigation**, **runtime Status Bar Notifications**, and **Activity Lifecycle tracking using Logcat**.

---

## 🎯 Aim

To develop an Android application named **FoodOrder** for a campus cafeteria that uses basic Android UI components such as **TextView, EditText, CheckBox, RadioButton, RadioGroup, Button, and ImageView** across multiple activities.

The application demonstrates:

* User input using Android Views
* Navigation between activities using **Explicit Intents**
* Passing data using `Intent.putExtra()`
* Runtime notification permission handling
* Status bar notifications using `NotificationCompat`
* Activity Lifecycle monitoring using **Logcat**

---

## 🏗️ Application Architecture

The application consists of three activities:

```text
HomeActivity
     ↓
MenuActivity
     ↓
ConfirmationActivity
```

### 1. HomeActivity

Displays the welcome screen of the campus cafeteria and provides a button to navigate to the menu.

### 2. MenuActivity

Allows the user to:

* Select a food item
* Enter quantity
* Select vegetarian/non-vegetarian food
* Select meal type
* Place the order

### 3. ConfirmationActivity

Displays the order summary and sends a notification confirming that the order has been placed.

---

## 🛠️ Technologies Used

| Technology          | Purpose                   |
| ------------------- | ------------------------- |
| Kotlin              | Application development   |
| Android Studio      | Development environment   |
| XML                 | UI layout design          |
| Android Views       | User interface components |
| Explicit Intent     | Activity navigation       |
| Intent Extras       | Data transfer             |
| NotificationChannel | Notification management   |
| NotificationCompat  | Notification creation     |
| Logcat              | Lifecycle monitoring      |

---

# 📋 Procedure

### Step 1: Create the Application

Create an Android Studio project named **FoodOrder** using Kotlin.

### Step 2: Create Activities

Create three activities:

* `HomeActivity`
* `MenuActivity`
* `ConfirmationActivity`

### Step 3: Design Home Screen

Create `activity_home.xml` containing:

* `ImageView`
* Cafeteria title
* Welcome message
* Navigation `Button`

The button opens `MenuActivity`.

### Step 4: Design Menu Screen

Create `activity_menu.xml` containing:

* `RadioGroup` for food selection
* `RadioButton` options for Vegetarian and Non-Vegetarian items
* `EditText` for quantity
* `CheckBox` for vegetarian/diet tagging
* `RadioGroup` for meal type
* `RadioButton` options for Dine In and Takeaway
* `Button` to place the order

### Step 5: Capture User Input

In `MenuActivity`, retrieve the selected food item, quantity, vegetarian status, and meal type.

### Step 6: Validate Input

Check whether the user has entered a quantity and selected the required food and meal options.

If information is missing, display a `Toast` message.

### Step 7: Transfer Data Using Intent

Use an explicit Intent and `putExtra()` to send the order information from `MenuActivity` to `ConfirmationActivity`.

### Step 8: Display Order Summary

In `ConfirmationActivity`, retrieve the values using `getStringExtra()` and display the order summary using a `TextView`.

### Step 9: Create Notification

Create a `NotificationChannel` with high importance and use `NotificationCompat.Builder` to generate an order confirmation notification.

### Step 10: Handle Notification Permission

For Android 13 and above, request the `POST_NOTIFICATIONS` runtime permission before displaying the notification.

### Step 11: Monitor Activity Lifecycle

Override the following lifecycle methods:

```text
onCreate()
onStart()
onResume()
onPause()
onStop()
onDestroy()
```

Use `Log.d()` to display lifecycle transitions in Logcat.

---

# 💻 Main Source Code

## 1. MenuActivity.kt

```kotlin
package com.example.foodiecafe

import android.content.Intent
import android.os.Bundle
import android.util.Log
import android.widget.*
import androidx.appcompat.app.AppCompatActivity

class MenuActivity : AppCompatActivity() {

    private val tag = "MenuActivity_LifeCycle"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_menu)

        Log.d(tag, "onCreate called")

        val rgFoodItems = findViewById<RadioGroup>(R.id.rgFoodItems)
        val etQuantity = findViewById<EditText>(R.id.etQuantity)
        val cbVeg = findViewById<CheckBox>(R.id.cbVeg)
        val rgMealType = findViewById<RadioGroup>(R.id.rgMealType)
        val btnPlaceOrder = findViewById<Button>(R.id.btnPlaceOrder)

        // Automatically update vegetarian checkbox
        rgFoodItems.setOnCheckedChangeListener { _, checkedId ->
            cbVeg.isChecked =
                checkedId == R.id.rbVegBurger ||
                checkedId == R.id.rbVegPizza
        }

        btnPlaceOrder.setOnClickListener {

            val quantity = etQuantity.text.toString().trim()

            if (quantity.isEmpty()) {
                Toast.makeText(
                    this,
                    "Please enter quantity",
                    Toast.LENGTH_SHORT
                ).show()
                return@setOnClickListener
            }

            if (rgFoodItems.checkedRadioButtonId == -1) {
                Toast.makeText(
                    this,
                    "Please select a food item",
                    Toast.LENGTH_SHORT
                ).show()
                return@setOnClickListener
            }

            if (rgMealType.checkedRadioButtonId == -1) {
                Toast.makeText(
                    this,
                    "Please select meal type",
                    Toast.LENGTH_SHORT
                ).show()
                return@setOnClickListener
            }

            val selectedFoodId =
                rgFoodItems.checkedRadioButtonId

            val foodItemName =
                findViewById<RadioButton>(selectedFoodId)
                    .text.toString()

            val selectedMealId =
                rgMealType.checkedRadioButtonId

            val mealType =
                findViewById<RadioButton>(selectedMealId)
                    .text.toString()

            val isVeg =
                if (cbVeg.isChecked)
                    "Yes (Veg)"
                else
                    "No (Non-Veg)"

            val intent =
                Intent(this, ConfirmationActivity::class.java).apply {

                    putExtra("ITEM", foodItemName)
                    putExtra("QTY", quantity)
                    putExtra("VEG", isVeg)
                    putExtra("MEAL", mealType)
                }

            startActivity(intent)
        }
    }

    override fun onStart() {
        super.onStart()
        Log.d(tag, "onStart called")
    }

    override fun onResume() {
        super.onResume()
        Log.d(tag, "onResume called")
    }

    override fun onPause() {
        super.onPause()
        Log.d(tag, "onPause called")
    }

    override fun onStop() {
        super.onStop()
        Log.d(tag, "onStop called")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d(tag, "onDestroy called")
    }
}
```

---

# 🔔 2. ConfirmationActivity.kt

```kotlin
package com.example.foodiecafe

import android.app.NotificationChannel
import android.app.NotificationManager
import android.content.Context
import android.content.pm.PackageManager
import android.os.Build
import android.os.Bundle
import android.util.Log
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.ActivityCompat
import androidx.core.app.NotificationCompat
import androidx.core.content.ContextCompat

class ConfirmationActivity : AppCompatActivity() {

    private val tag = "Confirm_LifeCycle"
    private val channelId = "food_order_channel"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_confirmation)

        Log.d(tag, "onCreate called")

        val item = intent.getStringExtra("ITEM") ?: ""
        val qty = intent.getStringExtra("QTY") ?: "1"
        val veg = intent.getStringExtra("VEG") ?: "No"
        val meal = intent.getStringExtra("MEAL") ?: "Dine In"

        val summary =
            "Item: $item\n" +
            "Quantity: $qty\n" +
            "Vegetarian: $veg\n" +
            "Type: $meal"

        findViewById<TextView>(
            R.id.tvOrderSummary
        ).text = summary

        createNotificationChannel()

        checkPermissionAndNotify(
            "Order Placed Successfully",
            "Your order for $qty x $item is being prepared!"
        )
    }

    private fun createNotificationChannel() {

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {

            val channel = NotificationChannel(
                channelId,
                "Food Orders",
                NotificationManager.IMPORTANCE_HIGH
            )

            val manager =
                getSystemService(
                    Context.NOTIFICATION_SERVICE
                ) as NotificationManager

            manager.createNotificationChannel(channel)
        }
    }

    private fun checkPermissionAndNotify(
        title: String,
        msg: String
    ) {

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {

            if (
                ContextCompat.checkSelfPermission(
                    this,
                    android.Manifest.permission.POST_NOTIFICATIONS
                ) != PackageManager.PERMISSION_GRANTED
            ) {

                ActivityCompat.requestPermissions(
                    this,
                    arrayOf(
                        android.Manifest.permission.POST_NOTIFICATIONS
                    ),
                    1
                )

                return
            }
        }

        showNotification(title, msg)
    }

    override fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<out String>,
        grantResults: IntArray
    ) {

        super.onRequestPermissionsResult(
            requestCode,
            permissions,
            grantResults
        )

        if (
            requestCode == 1 &&
            grantResults.isNotEmpty() &&
            grantResults[0] == PackageManager.PERMISSION_GRANTED
        ) {

            showNotification(
                "Order Placed Successfully",
                "Your order has been submitted!"
            )
        }
    }

    private fun showNotification(
        title: String,
        msg: String
    ) {

        val builder =
            NotificationCompat.Builder(
                this,
                channelId
            )
                .setSmallIcon(
                    android.R.drawable.ic_dialog_info
                )
                .setContentTitle(title)
                .setContentText(msg)
                .setPriority(
                    NotificationCompat.PRIORITY_HIGH
                )
                .setAutoCancel(true)

        val manager =
            getSystemService(
                Context.NOTIFICATION_SERVICE
            ) as NotificationManager

        manager.notify(
            101,
            builder.build()
        )
    }

    override fun onStart() {
        super.onStart()
        Log.d(tag, "onStart called")
    }

    override fun onResume() {
        super.onResume()
        Log.d(tag, "onResume called")
    }

    override fun onPause() {
        super.onPause()
        Log.d(tag, "onPause called")
    }

    override fun onStop() {
        super.onStop()
        Log.d(tag, "onStop called")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d(tag, "onDestroy called")
    }
}
```

---

# 🔄 Intent Data Transfer

The order details are transferred from `MenuActivity` to `ConfirmationActivity` using explicit Intent extras.

### Sending Data

```kotlin
putExtra("ITEM", foodItemName)
putExtra("QTY", quantity)
putExtra("VEG", isVeg)
putExtra("MEAL", mealType)
```

### Receiving Data

```kotlin
val item = intent.getStringExtra("ITEM") ?: ""
val qty = intent.getStringExtra("QTY") ?: "1"
val veg = intent.getStringExtra("VEG") ?: "No"
val meal = intent.getStringExtra("MEAL") ?: "Dine In"
```

---

# 🔔 Notification

The application creates a notification channel named **Food Orders**.

```kotlin
NotificationChannel(
    channelId,
    "Food Orders",
    NotificationManager.IMPORTANCE_HIGH
)
```

The notification is generated using:

```kotlin
NotificationCompat.Builder(
    this,
    channelId
)
```

For Android 13 and above, the application requests:

```text
POST_NOTIFICATIONS
```

permission before displaying the notification.

---

# 🔄 Activity Lifecycle

The following lifecycle methods are monitored:

```text
onCreate()
      ↓
onStart()
      ↓
onResume()
      ↓
   Activity Running
      ↓
onPause()
      ↓
onStop()
      ↓
onDestroy()
```

Lifecycle events are displayed in **Logcat** using `Log.d()`.

Example:

```kotlin
Log.d(tag, "onCreate called")
Log.d(tag, "onStart called")
Log.d(tag, "onResume called")
```

---

# 📱 Screenshots

## 1. Home Screen

The Home screen displays the campus cafeteria welcome interface and navigation button.

![Home Screen](screenshots/home-screen.png)

---

## 2. Menu Selection Screen

The Menu screen allows the user to select food, enter quantity, select vegetarian status, and choose the meal type.

![Menu Selection Screen](screenshots/menu-selection.png)

---

## 3. Order Confirmation & Notification

The Confirmation screen displays the selected order details and generates a successful order notification.

![Order Confirmation](screenshots/order-confirmation.png)

![Notification Output](screenshots/notification.png)

---

## 4. Lifecycle Logcat

The Activity lifecycle transitions can be observed in Android Studio Logcat.

![Lifecycle Logcat](screenshots/lifecycle-logcat.png)

---

# 🧪 Test Cases

| Test Case | Input/Action                  | Expected Result                |
| --------- | ----------------------------- | ------------------------------ |
| 1         | Open application              | Home screen appears            |
| 2         | Click Order/Menu button       | Menu screen opens              |
| 3         | Select food item              | Food item is selected          |
| 4         | Enter quantity                | Quantity is accepted           |
| 5         | Select meal type              | Meal type is selected          |
| 6         | Click Place Order             | Confirmation screen opens      |
| 7         | Allow notification permission | Order notification appears     |
| 8         | Leave quantity empty          | Toast asks for quantity        |
| 9         | Do not select food            | Toast asks to select food      |
| 10        | Do not select meal type       | Toast asks to select meal type |
| 11        | Open Logcat                   | Lifecycle events are displayed |

---

# 📂 Project Structure

```text
FoodOrder/
│
├── app/
│   └── src/
│       └── main/
│           ├── java/
│           │   └── com.example.foodiecafe/
│           │       ├── HomeActivity.kt
│           │       ├── MenuActivity.kt
│           │       └── ConfirmationActivity.kt
│           │
│           └── res/
│               ├── layout/
│               │   ├── activity_home.xml
│               │   ├── activity_menu.xml
│               │   └── activity_confirmation.xml
│               │
│               └── drawable/
│
├── screenshots/
│   ├── home-screen.png
│   ├── menu-selection.png
│   ├── order-confirmation.png
│   ├── notification.png
│   └── lifecycle-logcat.png
│
└── README.md
```

---

# 🎓 Concepts Demonstrated

This experiment demonstrates the following Android concepts:

* Android Activity
* XML Layout
* TextView
* ImageView
* EditText
* CheckBox
* RadioButton
* RadioGroup
* Button
* Toast
* Explicit Intent
* `putExtra()`
* `getStringExtra()`
* Notification Channel
* Runtime Notification Permission
* `NotificationCompat`
* Activity Lifecycle
* Logcat
* Event Handling

---

# ✅ Result

The **FoodOrder Campus Cafeteria Application** was successfully developed and executed using Kotlin.

The application successfully:

* Displays a cafeteria home screen.
* Accepts food-order details using standard Android Views.
* Validates required user inputs.
* Transfers order information between activities using **Explicit Intents**.
* Displays the order summary on the confirmation screen.
* Creates and displays a **Status Bar Notification**.
* Handles notification permission for Android 13 and above.
* Tracks Activity Lifecycle events using **Logcat**.

Therefore, the objectives of **MAD Experiment 08** were successfully achieved.
<img width="218" height="421" alt="Screenshot 2026-09-10 111923" src="https://github.com/user-attachments/assets/6e6019d2-3d18-4bba-8910-ffafec7d7ad7" />
<img width="221" height="488" alt="Screenshot 2026-09-10 111908" src="https://github.com/user-attachments/assets/4d61f65d-4995-4712-b19c-8216d73136a8" />
<img width="227" height="427" alt="Screenshot 2026-09-10 111857" src="https://github.com/user-attachments/assets/d142c28f-b201-40c1-876e-9673456913d9" />
