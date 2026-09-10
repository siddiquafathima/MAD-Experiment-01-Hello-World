# MAD Experiment 08: FoodOrder - Campus Cafeteria Application

A campus cafeteria ordering Android application demonstrating standard Android Views, inter-activity Intent navigation, runtime Status Bar Notifications, and Activity Lifecycle tracking using Logcat in Kotlin.

---

## Aim
To develop an Android application named **FoodOrder** for a campus cafeteria that utilizes basic UI views (TextView, EditText, CheckBox, RadioButton, RadioGroup, Button) across multiple activities, passes data using explicit **Intents**, triggers **Status Bar Notifications** upon order placement, and monitors the **Activity Lifecycle** transitions via Logcat.

---

## Procedure
1. **Application Architecture:** Structure the application with three distinct activities: `HomeActivity` (welcome & navigation), `MenuActivity` (item selection & form entry), and `ConfirmationActivity` (order summary & notification dispatch).
2. **Layout & View Design:** 
   * Design `activity_home.xml` with an `ImageView`, cafeteria titles, and a navigation button.
   * Design `activity_menu.xml` containing categorized `RadioButton` options for Vegetarian and Non-Vegetarian food items, an `EditText` for quantity input, a `CheckBox` for diet tagging, and a `RadioGroup` for meal type selection (Dine In / Takeaway).
   * Design `activity_confirmation.xml` using a `TextView` to present the generated summary.
3. **Intent Data Transfer:** Capture user inputs in `MenuActivity` and pass the values to `ConfirmationActivity` using `Intent.putExtra()`.
4. **Notification Setup:** Create a `NotificationChannel` with high importance in `ConfirmationActivity` and issue a status bar banner using `NotificationCompat.Builder` to alert the user of successful order submission.
5. **Lifecycle Logging:** Override the activity lifecycle callbacks (`onCreate`, `onStart`, `onResume`, `onPause`, `onStop`, `onDestroy`) in each activity to log state transitions to Logcat under dedicated tags.

---

## Main Source Code

### 1. `MenuActivity.kt`
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

        // Auto-toggle Vegetarian checkbox based on selection
        rgFoodItems.setOnCheckedChangeListener { _, checkedId ->
            cbVeg.isChecked = (checkedId == R.id.rbVegBurger || checkedId == R.id.rbVegPizza)
        }

        btnPlaceOrder.setOnClickListener {
            val quantity = etQuantity.text.toString().trim()
            if (quantity.isEmpty()) {
                Toast.makeText(this, "Please enter quantity", Toast.LENGTH_SHORT).show()
                return@setOnClickListener
            }

            val selectedFoodId = rgFoodItems.checkedRadioButtonId
            val foodItemName = findViewById<RadioButton>(selectedFoodId).text.toString()

            val selectedMealId = rgMealType.checkedRadioButtonId
            val mealType = findViewById<RadioButton>(selectedMealId).text.toString()
            val isVeg = if (cbVeg.isChecked) "Yes (Veg)" else "No (Non-Veg)"

            val intent = Intent(this, ConfirmationActivity::class.java).apply {
                putExtra("ITEM", foodItemName)
                putExtra("QTY", quantity)
                putExtra("VEG", isVeg)
                putExtra("MEAL", mealType)
            }
            startActivity(intent)
        }
    }

    override fun onStart() { super.onStart(); Log.d(tag, "onStart called") }
    override fun onResume() { super.onResume(); Log.d(tag, "onResume called") }
    override fun onPause() { super.onPause(); Log.d(tag, "onPause called") }
    override fun onStop() { super.onStop(); Log.d(tag, "onStop called") }
    override fun onDestroy() { super.onDestroy(); Log.d(tag, "onDestroy called") }
}<img width="218" height="421" alt="Screenshot 2026-09-10 111923" src="https://github.com/user-attachments/assets/509f181d-afe9-4f14-a8ca-ed81f4986a9c" />
<img width="221" height="488" alt="Screenshot 2026-09-10 111908" src="https://github.com/user-attachments/assets/4274abe4-d465-4887-b178-ab2f3e20c090" />
<img width="227" height="427" alt="Screenshot 2026-09-10 111857" src="https://github.com/user-attachments/assets/49843f93-087d-4cd7-8560-b70face4eb93" />
---
##ConfirmationActivity.kt
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

        val summary = "Item: $item\nQuantity:$qty\nVegetarian: $veg\nType:$meal"
        findViewById<TextView>(R.id.tvOrderSummary).text = summary

        createNotificationChannel()
        checkPermissionAndNotify("Order Placed Successfully", "Your order for $qty x$item is being prepared!")
    }

    private fun createNotificationChannel() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(
                channelId, "Food Orders", NotificationManager.IMPORTANCE_HIGH
            )
            val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
            manager.createNotificationChannel(channel)
        }
    }

    private fun checkPermissionAndNotify(title: String, msg: String) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
            if (ContextCompat.checkSelfPermission(this, android.Manifest.permission.POST_NOTIFICATIONS)
                != PackageManager.PERMISSION_GRANTED) {
                ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.POST_NOTIFICATIONS), 1)
                return
            }
        }
        showNotification(title, msg)
    }

    private fun showNotification(title: String, msg: String) {
        val builder = NotificationCompat.Builder(this, channelId)
            .setSmallIcon(android.R.drawable.ic_dialog_info)
            .setContentTitle(title)
            .setContentText(msg)
            .setPriority(NotificationCompat.PRIORITY_HIGH)
            .setAutoCancel(true)

        val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        manager.notify(101, builder.build())
    }

    override fun onStart() { super.onStart(); Log.d(tag, "onStart called") }
    override fun onResume() { super.onResume(); Log.d(tag, "onResume called") }
    override fun onPause() { super.onPause(); Log.d(tag, "onPause called") }
    override fun onStop() { super.onStop(); Log.d(tag, "onStop called") }
    override fun onDestroy() { super.onDestroy(); Log.d(tag, "onDestroy called") }
}
----
----
##Screenshots
1. Home Screen


2. Menu Selection Screen


3. Order Confirmation & Notification Output

-----

##Result
The FoodOrder campus cafeteria application was developed and executed successfully. Form selections were captured via basic UI views, passed through explicit Intents to the confirmation activity, a heads-up order notification was posted, and activity lifecycle transitions were monitored in Logcat.
---
