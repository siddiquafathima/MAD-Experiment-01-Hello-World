<img width="960" height="600" alt="Screenshot 2026-09-17 111129" src="https://github.com/user-attachments/assets/b064a2c8-1e89-4fce-8000-54d09bc8ad90" />
<img width="960" height="600" alt="Screenshot 2026-09-17 111106" src="https://github.com/user-attachments/assets/3167b292-ff45-4f34-8573-05a9ce44b0bb" />
# MAD Experiment 07: Adaptive UI Using ListView and ImageView

A Mobile Application Development lab experiment demonstrating a dynamic user interface using `ListView` and `ImageView` with a custom `ArrayAdapter` in Kotlin.

---

## 1. Aim

To develop an Android application that displays a list of technologies using `ListView` and dynamically updates an `ImageView` and descriptive `TextView` when an item is selected.

---

## 2. Concept & Technology Overview

- **ListView:** Displays a list of technology items dynamically.
- **Custom ArrayAdapter:** Binds images, titles, and descriptions to each ListView row.
- **ImageView:** Displays the icon of the selected technology.
- **Dynamic Preview:** Updates the selected technology's image and description when the user clicks an item.
- **Toast:** Provides feedback about the selected item.
- **View Recycling:** Uses `convertView` to efficiently reuse ListView row layouts.

---

## 3. Project File & Folder Structure

```text
AdaptiveListViewApp/
├── app/
│   ├── manifests/
│   │   └── AndroidManifest.xml
│   ├── java/com/example/adaptivelistviewapp/
│   │   ├── MainActivity.kt
│   │   └── CustomAdapter.kt
│   └── res/
│       └── layout/
│           ├── activity_main.xml
│           └── list_item.xml
├── build.gradle.kts (Module :app)
└── README.md

4. Source Code
4.1 MainActivity.kt

package com.example.adaptivelistviewapp

import android.os.Bundle
import android.widget.ImageView
import android.widget.ListView
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val ivSelectedImage = findViewById<ImageView>(R.id.ivSelectedImage)
        val tvSelectedTitle = findViewById<TextView>(R.id.tvSelectedTitle)
        val tvSelectedDesc = findViewById<TextView>(R.id.tvSelectedDesc)
        val listView = findViewById<ListView>(R.id.listView)

        val techNames = arrayOf(
            "Android OS",
            "Kotlin",
            "Java",
            "Python",
            "Flutter"
        )

        val techDescriptions = arrayOf(
            "Open source mobile platform by Google",
            "Modern statically typed language for Android",
            "Object-oriented programming language",
            "Interpreted, high-level general language",
            "Cross-platform UI development framework"
        )

        val techIcons = arrayOf(
            android.R.drawable.sym_def_app_icon,
            android.R.drawable.ic_dialog_info,
            android.R.drawable.ic_menu_compass,
            android.R.drawable.ic_menu_manage,
            android.R.drawable.ic_menu_send
        )

        val adapter = CustomAdapter(
            this,
            techNames,
            techDescriptions,
            techIcons
        )

        listView.adapter = adapter

        // Set initial preview
        ivSelectedImage.setImageResource(techIcons[0])
        tvSelectedTitle.text = techNames[0]
        tvSelectedDesc.text = techDescriptions[0]

        // Update preview when an item is selected
        listView.setOnItemClickListener { _, _, position, _ ->

            val selectedName = techNames[position]
            val selectedDesc = techDescriptions[position]
            val selectedIcon = techIcons[position]

            ivSelectedImage.setImageResource(selectedIcon)
            tvSelectedTitle.text = selectedName
            tvSelectedDesc.text = selectedDesc

            Toast.makeText(
                this,
                "Selected: $selectedName",
                Toast.LENGTH_SHORT
            ).show()
        }
    }
}


4.2 CustomAdapter.kt

package com.example.adaptivelistviewapp

import android.content.Context
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ArrayAdapter
import android.widget.ImageView
import android.widget.TextView

class CustomAdapter(
    private val context: Context,
    private val titles: Array<String>,
    private val descriptions: Array<String>,
    private val imageIds: Array<Int>
) : ArrayAdapter<String>(context, R.layout.list_item, titles) {

    override fun getView(
        position: Int,
        convertView: View?,
        parent: ViewGroup
    ): View {

        val rowView = convertView ?: LayoutInflater
            .from(context)
            .inflate(R.layout.list_item, parent, false)

        val titleView =
            rowView.findViewById<TextView>(R.id.tvTitle)

        val subtitleView =
            rowView.findViewById<TextView>(R.id.tvSubtitle)

        val imageView =
            rowView.findViewById<ImageView>(R.id.ivIcon)

        titleView.text = titles[position]
        subtitleView.text = descriptions[position]
        imageView.setImageResource(imageIds[position])

        return rowView
    }
}

5. Test Cases & Execution Verification
Test Case 1: Student Identity Verification

Action: Launch the application.

Expected Output:
The application displays the student details such as Name and USN along with the technology list.

Test Case 2: Dynamic Image Selection

Action: Select a technology from the ListView, such as Kotlin.

Expected Output:
The preview section updates the ImageView, title, and description according to the selected technology.

Test Case 3: Toast Feedback

Action: Tap any technology item.

Expected Output:
A Toast message appears displaying:

Selected: [Item Name]
6. Result

The Android application was successfully developed using ListView, ImageView, TextView, and a custom ArrayAdapter. The application dynamically updates the preview information when a list item is selected and provides Toast feedback, successfully satisfying the experiment requirements.


### One more thing

Since you're keeping your MAD GitHub submissions **simple and academic**, I would **not add** the `Elicitations` section at the bottom. Remove everything starting from:

```text
<Elicitations message="What would you like to work on next?">

