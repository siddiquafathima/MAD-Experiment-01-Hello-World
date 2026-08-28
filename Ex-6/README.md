
AndroidProgram
Repository navigation
Code
Issues
Pull requests
AndroidProgram
/Experiment-6/
erikafdes
erikafdes
yesterday
AndroidProgram
/Experiment-6/
Name	Last commit date
..
README.md
yesterday
README.md
Experiment 6 – Develop an Android Application Using Basic Views
Experiment Title
Develop an Android Application Using Basic Views

Aim
To develop an Android application using basic Android Views and understand how different UI components are created and handled using Kotlin and XML.

Objective
The objective of this experiment is to understand and implement basic Android UI components such as:

TextView
EditText
RadioButton
RadioGroup
CheckBox
Spinner
ToggleButton
Button
Toast
The application demonstrates how user input can be collected, validated and processed using Kotlin.

Scenario
A Student Registration Form is developed to demonstrate the use of basic Android Views.

The application allows a student to:

Enter their Name
Enter their USN
Select Gender
Select multiple Technical Skills
Select Department
Select Agreement Status
Submit the form
After clicking the Submit button, the entered information is displayed using a Toast message.

Technology Used
Technology	Description
Android Studio	Integrated Development Environment
Kotlin	Programming language
XML	User interface design
Android SDK	Android development framework
Gradle	Build automation tool
Android Views	UI components
Android Views Used
Android View	Purpose
TextView	Displays the title and labels
EditText	Accepts student Name and USN
RadioButton	Allows the user to select Gender
RadioGroup	Groups the Gender RadioButtons
CheckBox	Allows selection of multiple Skills
Spinner	Allows selection of Department
ToggleButton	Allows selection of Agreement Status
Button	Submits the registration form
Toast	Displays the submitted information
Concept / Technology Behind the Application
Android Views are the basic building blocks used to create the user interface of Android applications.

In this experiment, XML is used to design the user interface, while Kotlin is used to implement the application logic and handle user interactions.

The findViewById() method is used to access the Views from the Kotlin activity.

The setOnClickListener() method is used to handle the Submit button click.

The application performs basic input validation to ensure that the Name and USN fields are not empty.

A Spinner is used to display a list of departments, while CheckBox allows the user to select multiple skills.

Application Features
Student Name input
USN input
Gender selection
Multiple skill selection
Department selection
Agreement status selection
Input validation
Submit button
Toast message displaying the result
Project Structure
activity6/
│
├── app/
│   ├── src/
│   │   ├── androidTest/
│   │   │
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/
│   │   │   │       └── example/
│   │   │   │           └── activity6/
│   │   │   │               └── MainActivity.kt
│   │   │   │
│   │   │   ├── res/
│   │   │   │   ├── drawable/
│   │   │   │   ├── layout/
│   │   │   │   │   └── activity_main.xml
│   │   │   │   ├── mipmap/
│   │   │   │   └── values/
│   │   │   │
│   │   │   └── AndroidManifest.xml
│   │   │
│   │   └── test/
│   │
│   └── build.gradle.kts
│
├── screenshots/
│   ├── output.png
│   ├── test-case-1.png
│   ├── test-case-2.png
│   └── test-case-3.png
│
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
├── gradlew
├── gradlew.bat
├── .gitignore
└── README.md
Working
Launch the Android application.
The Student Registration Form is displayed.
Enter the student's Name.
Enter the student's USN.
Select the student's Gender using the RadioButton.
Select one or more Skills using the CheckBox.
Select a Department from the Spinner.
Select the Agreement Status using the ToggleButton.
Click the Submit button.
The application validates the Name and USN fields.
If either field is empty, a Toast message displays "Please enter Name and USN".
If the required fields are entered, the selected information is collected.
A Toast message displays the complete student registration information.
MainActivity.kt
package com.example.activity6

import android.os.Bundle
import android.widget.ArrayAdapter
import android.widget.Button
import android.widget.CheckBox
import android.widget.EditText
import android.widget.RadioGroup
import android.widget.Spinner
import android.widget.Toast
import android.widget.ToggleButton
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContentView(R.layout.activity_main)

        val etName = findViewById<EditText>(R.id.etName)
        val etUsn = findViewById<EditText>(R.id.etUsn)

        val rgGender = findViewById<RadioGroup>(R.id.rgGender)

        val cbJava = findViewById<CheckBox>(R.id.cbJava)
        val cbKotlin = findViewById<CheckBox>(R.id.cbKotlin)
        val cbAndroid = findViewById<CheckBox>(R.id.cbAndroid)

        val spDepartment = findViewById<Spinner>(R.id.spDepartment)

        val toggleAgree = findViewById<ToggleButton>(R.id.toggleAgree)

        val btnSubmit = findViewById<Button>(R.id.btnSubmit)

        val departments = arrayOf(
            "Select Department",
            "Computer Science",
            "Information Science",
            "Electronics",
            "Mechanical"
        )

        val adapter = ArrayAdapter(
            this,
            android.R.layout.simple_spinner_item,
            departments
        )

        adapter.setDropDownViewResource(
            android.R.layout.simple_spinner_dropdown_item
        )

        spDepartment.adapter = adapter

        btnSubmit.setOnClickListener {

            val name = etName.text.toString().trim()
            val usn = etUsn.text.toString().trim()

            if (name.isEmpty() || usn.isEmpty()) {

                Toast.makeText(
                    this,
                    "Please enter Name and USN",
                    Toast.LENGTH_SHORT
                ).show()

                return@setOnClickListener
            }

            val gender = when (rgGender.checkedRadioButtonId) {

                R.id.rbMale -> "Male"

                R.id.rbFemale -> "Female"

                else -> "Not Selected"
            }

            val skills = mutableListOf<String>()

            if (cbJava.isChecked) {
                skills.add("Java")
            }

            if (cbKotlin.isChecked) {
                skills.add("Kotlin")
            }

            if (cbAndroid.isChecked) {
                skills.add("Android")
            }

            val selectedSkills = if (skills.isEmpty()) {
                "None"
            } else {
                skills.joinToString(", ")
            }

            val department = spDepartment.selectedItem.toString()

            val agreement = if (toggleAgree.isChecked) {
                "Agreed"
            } else {
                "Not Agreed"
            }

            val message = """
                Name: $name
                USN: $usn
                Gender: $gender
                Skills: $selectedSkills
                Department: $department
                Status: $agreement
            """.trimIndent()

            Toast.makeText(
                this,
                message,
                Toast.LENGTH_LONG
            ).show()
        }
    }
}
activity_main.xml
<?xml version="1.0" encoding="utf-8"?>

<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="20dp">

        <TextView
            android:id="@+id/tvTitle"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:text="Student Registration Form"
            android:textSize="24sp"
            android:textStyle="bold"
            android:layout_marginBottom="20dp" />

        <EditText
            android:id="@+id/etName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Enter your name"
            android:inputType="textPersonName" />

        <EditText
            android:id="@+id/etUsn"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="10dp"
            android:hint="Enter your USN"
            android:inputType="text" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp"
            android:text="Gender"
            android:textSize="18sp"
            android:textStyle="bold" />

        <RadioGroup
            android:id="@+id/rgGender"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <RadioButton
                android:id="@+id/rbMale"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Male" />

            <RadioButton
                android:id="@+id/rbFemale"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginStart="20dp"
                android:text="Female" />

        </RadioGroup>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="15dp"
            android:text="Skills"
            android:textSize="18sp"
            android:textStyle="bold" />

        <CheckBox
            android:id="@+id/cbJava"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Java" />

        <CheckBox
            android:id="@+id/cbKotlin"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Kotlin" />

        <CheckBox
            android:id="@+id/cbAndroid"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Android" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="15dp"
            android:text="Department"
            android:textSize="18sp"
            android:textStyle="bold" />

        <Spinner
            android:id="@+id/spDepartment"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="5dp" />

        <ToggleButton
            android:id="@+id/toggleAgree"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp"
            android:textOff="Not Agreed"
            android:textOn="Agreed" />

        <Button
            android:id="@+id/btnSubmit"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="25dp"
            android:text="Submit" />

    </LinearLayout>

</ScrollView>
AndroidManifest.xml
<?xml version="1.0" encoding="utf-8"?>

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Activity6">

        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">

            <intent-filter>

                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />

            </intent-filter>

        </activity>

    </application>

</manifest>
Screenshots
Add your application screenshots in the screenshots folder.

Recommended screenshots:

1. Application Interface
Show the complete Student Registration Form.

2. Filled Form
Show the form with:

Name
USN
Gender
Skills
Department
Agreement Status
selected. exp6-1

3. Toast Output
Show the Toast message containing the submitted student information.

Example:

Name: Erika Fernandes
USN: 25MCAR0092
Gender: Female
Skills: Kotlin, Android
Department: Computer Science
Status: Agreed
exp6-2
Result
The Android application was successfully developed using basic Android Views with Kotlin and XML.

The application successfully accepts student details through EditText, RadioButton, CheckBox, Spinner, and ToggleButton. It performs basic input validation and displays the submitted information using a Toast message.

Conclusion
This experiment demonstrates the implementation of basic Android UI components using Kotlin and XML.

The experiment provides practical understanding of:

Creating Android Views using XML
Accessing Views using findViewById()
Handling button click events
Collecting user input
Performing basic validation
Using RadioButton and RadioGroup
Using multiple CheckBox selections
Using Spinner for list selection
Using ToggleButton for binary status
Displaying output using Toast
The Student Registration Form demonstrates how these basic Android Views can be combined to create a simple interactive Android application.


