<img width="238" height="500" alt="Screenshot 2026-10-14 202933" src="https://github.com/user-attachments/assets/c6d0ca84-3bbb-4c2c-809e-1595548293eb" />
# MAD Experiment 09: Data Persistence Using SharedPreferences and SQLite

A Mobile Application Development lab experiment demonstrating local data persistence in Android using **SharedPreferences** for key-value storage and **SQLiteOpenHelper** for structured relational database operations using Kotlin.

---

## 1. Aim

To develop an Android application that stores and retrieves user session data using SharedPreferences and performs database operations using SQLite, including inserting, viewing, and deleting student course records.

---

## 2. Concept & Technology Overview

- **SharedPreferences:** Stores small amounts of key-value data in private app storage. The saved values remain available after the application is closed and reopened.
- **SQLite Database:** A lightweight relational database integrated into Android. It stores structured data in tables and supports operations such as inserting, retrieving, and deleting records.
- **SQLiteOpenHelper:** Manages database creation and version upgrades through `onCreate()` and `onUpgrade()`.
- **Scenario:** The application saves a session value using SharedPreferences and stores student details and course names in an SQLite database.

---

## 3. Project File & Folder Structure

```text
DataPersistenceDemo/
├── app/
│   ├── manifests/
│   │   └── AndroidManifest.xml
│   ├── java/com/example/datapersistencedemo/
│   │   ├── DatabaseHelper.kt
│   │   └── MainActivity.kt
│   └── res/
│       └── layout/
│           └── activity_main.xml
├── build.gradle.kts
└── README.md
```

### File Descriptions

- `MainActivity.kt` — Handles the user interface, SharedPreferences, and SQLite operations.
- `DatabaseHelper.kt` — Creates and manages the SQLite database and student records.
- `activity_main.xml` — Defines the application's user interface.

---

## 4. Main Source Code

### 4.1 `DatabaseHelper.kt`

This class creates the SQLite database and provides functions to insert, retrieve, and delete student records.

```kotlin
package com.example.datapersistencedemo

import android.content.ContentValues
import android.content.Context
import android.database.Cursor
import android.database.sqlite.SQLiteDatabase
import android.database.sqlite.SQLiteOpenHelper

class DatabaseHelper(context: Context) :
    SQLiteOpenHelper(context, DATABASE_NAME, null, DATABASE_VERSION) {

    companion object {
        private const val DATABASE_NAME = "StudentDB.db"
        private const val DATABASE_VERSION = 1

        const val TABLE_NAME = "student_records"
        const val COL_ID = "id"
        const val COL_NAME = "student_name"
        const val COL_USN = "student_usn"
        const val COL_COURSE = "course_name"
    }

    override fun onCreate(db: SQLiteDatabase) {
        val createTable = """
            CREATE TABLE $TABLE_NAME (
                $COL_ID INTEGER PRIMARY KEY AUTOINCREMENT,
                $COL_NAME TEXT,
                $COL_USN TEXT,
                $COL_COURSE TEXT
            )
        """.trimIndent()

        db.execSQL(createTable)
    }

    override fun onUpgrade(
        db: SQLiteDatabase,
        oldVersion: Int,
        newVersion: Int
    ) {
        db.execSQL("DROP TABLE IF EXISTS $TABLE_NAME")
        onCreate(db)
    }

    fun insertRecord(
        name: String,
        usn: String,
        course: String
    ): Boolean {
        val values = ContentValues().apply {
            put(COL_NAME, name)
            put(COL_USN, usn)
            put(COL_COURSE, course)
        }

        val result = writableDatabase.insert(
            TABLE_NAME,
            null,
            values
        )

        return result != -1L
    }

    fun getAllRecords(): Cursor {
        return readableDatabase.rawQuery(
            "SELECT * FROM $TABLE_NAME",
            null
        )
    }

    fun clearAllRecords() {
        writableDatabase.delete(TABLE_NAME, null, null)
    }
}
```

### 4.2 `MainActivity.kt`

This activity handles session storage through SharedPreferences and student course records through SQLite.

```kotlin
package com.example.datapersistencedemo

import android.content.Context
import android.content.SharedPreferences
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    private lateinit var dbHelper: DatabaseHelper
    private lateinit var sharedPreferences: SharedPreferences

    private val prefFileName = "UserSessionPref"
    private val studentName = "Siddiqua Fathima"
    private val studentUsn = "1JN25MC000" // Replace with your actual USN

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        dbHelper = DatabaseHelper(this)
        sharedPreferences = getSharedPreferences(
            prefFileName,
            Context.MODE_PRIVATE
        )

        findViewById<TextView>(R.id.tvStudentUSN).text =
            "USN: $studentUsn"

        val etUserSession = findViewById<EditText>(R.id.etUserSession)
        val btnSavePref = findViewById<Button>(R.id.btnSavePref)
        val btnLoadPref = findViewById<Button>(R.id.btnLoadPref)
        val tvPrefResult = findViewById<TextView>(R.id.tvPrefResult)

        val etCourseName = findViewById<EditText>(R.id.etCourseName)
        val btnInsertSql = findViewById<Button>(R.id.btnInsertSql)
        val btnViewSql = findViewById<Button>(R.id.btnViewSql)
        val btnClearSql = findViewById<Button>(R.id.btnClearSql)
        val tvSqliteRecords =
            findViewById<TextView>(R.id.tvSqliteRecords)

        // SharedPreferences: Save session
        btnSavePref.setOnClickListener {
            val sessionText = etUserSession.text.toString().trim()

            if (sessionText.isNotEmpty()) {
                sharedPreferences.edit()
                    .putString("ACTIVE_SESSION", sessionText)
                    .apply()

                Toast.makeText(
                    this,
                    "Session saved to SharedPreferences",
                    Toast.LENGTH_SHORT
                ).show()

                etUserSession.text.clear()
            } else {
                Toast.makeText(
                    this,
                    "Enter a session value",
                    Toast.LENGTH_SHORT
                ).show()
            }
        }

        // SharedPreferences: Retrieve session
        btnLoadPref.setOnClickListener {
            val retrieved = sharedPreferences.getString(
                "ACTIVE_SESSION",
                "No session saved"
            )

            tvPrefResult.text = "Saved Pref: $retrieved"
        }

        // SQLite: Insert course record
        btnInsertSql.setOnClickListener {
            val course = etCourseName.text.toString().trim()

            if (course.isNotEmpty()) {
                val inserted = dbHelper.insertRecord(
                    studentName,
                    studentUsn,
                    course
                )

                if (inserted) {
                    Toast.makeText(
                        this,
                        "Row inserted into SQLite",
                        Toast.LENGTH_SHORT
                    ).show()

                    etCourseName.text.clear()
                    displayRecords(tvSqliteRecords)
                } else {
                    Toast.makeText(
                        this,
                        "Failed to insert record",
                        Toast.LENGTH_SHORT
                    ).show()
                }
            } else {
                Toast.makeText(
                    this,
                    "Enter a course name",
                    Toast.LENGTH_SHORT
                ).show()
            }
        }

        // SQLite: View all records
        btnViewSql.setOnClickListener {
            displayRecords(tvSqliteRecords)
        }

        // SQLite: Delete all records
        btnClearSql.setOnClickListener {
            dbHelper.clearAllRecords()
            tvSqliteRecords.text = "Database is currently empty."

            Toast.makeText(
                this,
                "All SQLite records deleted",
                Toast.LENGTH_SHORT
            ).show()
        }
    }

    private fun displayRecords(outputView: TextView) {
        val cursor = dbHelper.getAllRecords()

        if (cursor.count == 0) {
            outputView.text = "Database is currently empty."
            cursor.close()
            return
        }

        val stringBuilder = StringBuilder()

        while (cursor.moveToNext()) {
            val id = cursor.getInt(
                cursor.getColumnIndexOrThrow(DatabaseHelper.COL_ID)
            )
            val name = cursor.getString(
                cursor.getColumnIndexOrThrow(DatabaseHelper.COL_NAME)
            )
            val usn = cursor.getString(
                cursor.getColumnIndexOrThrow(DatabaseHelper.COL_USN)
            )
            val course = cursor.getString(
                cursor.getColumnIndexOrThrow(DatabaseHelper.COL_COURSE)
            )

            stringBuilder.append(
                "ID: $id | Course: $course\n" +
                "Student: $name ($usn)\n" +
                "----------------------------\n"
            )
        }

        cursor.close()
        outputView.text = stringBuilder.toString()
    }

    override fun onDestroy() {
        dbHelper.close()
        super.onDestroy()
    }
}
```

**Important:** Replace `1JN25MC000` with your correct USN if this is only a placeholder. Make sure the IDs referenced in `MainActivity.kt` match those in your `activity_main.xml`.

---

## 5. Test Cases & Execution Verification

### Test Case 1: SharedPreferences Persistence

- **Action:** Enter `Active_Lab_Session_09` and tap **Save Pref**. Close and reopen the app, then tap **Load Pref**.
- **Expected Output:** The saved session value is displayed correctly after reopening the app.

### Test Case 2: SQLite Record Insertion

- **Action:** Enter `Mobile Application Development` and tap **Insert Row**.
- **Expected Output:** A success toast appears, and the record is displayed with its ID, course name, student name, and USN.

### Test Case 3: View and Clear SQLite Records

- **Action:** Insert multiple courses, tap **View All**, and then tap **Clear DB**.
- **Expected Output:** All inserted records are displayed. After clearing, a confirmation toast appears and the records area displays `Database is currently empty.`

---

## 6. Screenshots

### Screenshot 1: Student Header and SharedPreferences

![SharedPreferences and student header](https://github.com/user-attachments/assets/e1d422d1-e72b-45e6-95e4-4dc3d42067ed)

### Screenshot 2: SQLite Record Insertion

Add a screenshot showing a successfully inserted course record with the student details.

### Screenshot 3: SQLite View and Clear Operations

Add a screenshot showing the records before clearing and the empty database state after clearing.

---

## 7. Result

The Android data persistence application was implemented using Kotlin, SharedPreferences, and SQLite. The application demonstrates saving and retrieving session data, inserting and displaying student course records, and deleting all stored database records.


