<img width="1920" height="1200" alt="Screenshot 2026-08-06 223847" src="https://github.com/user-attachments/assets/1efca23e-5b22-434b-88b4-2f034e8ed067" />
<img width="1920" height="1200" alt="Screenshot 2026-08-06 223752" src="https://github.com/user-attachments/assets/e938691f-70cd-45d7-b439-461f253e6515" />
<img width="1920" height="1200" alt="Screenshot 2026-08-06 223546" src="https://github.com/user-attachments/assets/e0d149e5-e50c-4c99-88b1-74ddb5f13419" />
Experiment 3: Fragments and Debugging in Android
Aim
To develop an Android application that uses Fragments to create a flexible user interface. The application contains two fragments:

The first fragment displays a list of items (Android, Java, Python, Kotlin, Flutter).
The second fragment displays the details of the selected item.
The application also demonstrates the use of Android Studio debugging tools, including normal and conditional breakpoints.
Objective
The objective of this experiment is to understand the working of Android Fragments and how they help create flexible user interfaces. The experiment also demonstrates Android Studio debugging features by inspecting fragment lifecycle, local variables, call stack, and breakpoint behavior.

🛠️ Technology Used
Android Studio
Kotlin
Android SDK
Android Emulator (Pixel 9)
Fragments
Android Debugger
Concepts Used
ListFragment
Displays a list of items using a ListView.

DetailFragment
Displays the details of the selected item.

Fragment Transaction
Used to switch between ListFragment and DetailFragment dynamically.

Bundle
Used to transfer data from one fragment to another.

Android Debugger
Used to inspect variable values, fragment lifecycle, and execution flow.

Code
MainActivity.kt
package com.example.fragments

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        if (savedInstanceState == null) {
            supportFragmentManager.beginTransaction()
                .replace(R.id.container, ListFragment())
                .commit()
        }
    }
}
ListFragment.kt
package com.example.fragments

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ArrayAdapter
import android.widget.ListView
import androidx.fragment.app.Fragment

class ListFragment : Fragment() {

    lateinit var listView: ListView

    val items = arrayOf(
        "Android",
        "Java",
        "Python",
        "Kotlin",
        "Flutter"
    )

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        val view = inflater.inflate(R.layout.fragment_list, container, false)

        listView = view.findViewById(R.id.listView)

        val adapter = ArrayAdapter(
            requireContext(),
            android.R.layout.simple_list_item_1,
            items
        )

        listView.adapter = adapter

        listView.setOnItemClickListener { _, _, position, _ ->

            val fragment = DetailFragment()

            val bundle = Bundle()
            bundle.putString("item", items[position])

            fragment.arguments = bundle

            parentFragmentManager.beginTransaction()
                .replace(R.id.container, fragment)
                .addToBackStack(null)
                .commit()
        }

        return view
    }
}
DetailFragment.kt
package com.example.fragments

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.fragment.app.Fragment

class DetailFragment : Fragment() {

    lateinit var textView: TextView

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        val view = inflater.inflate(R.layout.fragment_detail, container, false)

        textView = view.findViewById(R.id.textDetails)

        val item = arguments?.getString("item")

        textView.text = "Selected Item:\n\n$item"

        return view
    }
}
activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
fragment_list.xml
<?xml version="1.0" encoding="utf-8"?>

<ListView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/listView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
fragment_detail.xml
<?xml version="1.0" encoding="utf-8"?>

<TextView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/textDetails"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:textSize="25sp"
    android:gravity="center"/>
Output
Screenshot 2026-08-06 151054 Screenshot 2026-08-06 151112 Screenshot 2026-08-06 151129 ---
Result
The Android application was successfully developed using Fragments. The application displays a list of items in one fragment and the details of the selected item in another fragment. Android Studio debugging tools were successfully used to inspect fragment lifecycle, local variables, call stack, and compare the behavior of normal and conditional breakpoints.
