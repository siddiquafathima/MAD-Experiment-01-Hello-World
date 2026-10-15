# MAD Experiment 10: Data Sharing Using Contacts Content Provider

A Mobile Application Development lab experiment demonstrating data sharing in Android using the **Contacts Content Provider**, runtime permissions, `ContentResolver`, and Kotlin.

---

## 1. Aim

To develop an Android application that retrieves device contacts using the Contacts Content Provider and allows users to add contacts through the system contact-creation screen.

---

## 2. Concept & Technology Overview

- **Content Provider:** An Android component that manages and exposes application data to other applications through standard content URIs.
- **ContentResolver:** Provides methods to interact with content providers, including querying data using `query()`.
- **ContactsContract:** Defines standard constants for accessing contact information, including contact names and phone numbers.
- **Runtime Permissions:** Allows users to grant or deny access to sensitive data, such as device contacts.
- **Intent:** Opens the system contact-creation screen with the entered name and phone number pre-filled.

### Scenario

The application displays student details, requests permission to read device contacts, displays retrieved contacts, and opens the system contact-creation screen when the user wants to add a contact.

---

## 3. Project File & Folder Structure

```text
ContactsProviderApp/
├── app/
│   ├── manifests/
│   │   └── AndroidManifest.xml
│   ├── java/com/example/contactsproviderapp/
│   │   └── MainActivity.kt
│   └── res/
│       └── layout/
│           └── activity_main.xml
├── build.gradle.kts
└── README.md
```

### File Descriptions

- `MainActivity.kt` — Handles runtime permissions, contact insertion intents, and contact retrieval.
- `activity_main.xml` — Defines the student information banner, contact input fields, buttons, and contact display area.
- `AndroidManifest.xml` — Declares the required contacts permission.

---

## 4. Main Source Code

### `MainActivity.kt`

The activity uses `ContactsContract` and `ContentResolver` to retrieve contacts and an implicit intent to open the contact-creation screen.

```kotlin
package com.example.contactsproviderapp

import android.Manifest
import android.content.Intent
import android.content.pm.PackageManager
import android.database.Cursor
import android.os.Bundle
import android.provider.ContactsContract
import android.widget.Button
import android.widget.EditText
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.ActivityCompat
import androidx.core.content.ContextCompat

class MainActivity : AppCompatActivity() {

    private val permissionReqCode = 101

    private lateinit var tvContactList: TextView
    private lateinit var etContactName: EditText
    private lateinit var etContactPhone: EditText

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val tvStudentUSN = findViewById<TextView>(R.id.tvStudentUSN)
        tvStudentUSN.text = "USN: 25MCAR0121"

        etContactName = findViewById(R.id.etContactName)
        etContactPhone = findViewById(R.id.etContactPhone)
        tvContactList = findViewById(R.id.tvContactList)

        val btnAddContact = findViewById<Button>(R.id.btnAddContact)
        val btnFetchContacts = findViewById<Button>(R.id.btnFetchContacts)

        btnAddContact.setOnClickListener {
            val name = etContactName.text.toString().trim()
            val phone = etContactPhone.text.toString().trim()

            if (name.isNotEmpty() && phone.isNotEmpty()) {
                val intent =
                    Intent(ContactsContract.Intents.Insert.ACTION).apply {
                        type = ContactsContract.RawContacts.CONTENT_TYPE
                        putExtra(
                            ContactsContract.Intents.Insert.NAME,
                            name
                        )
                        putExtra(
                            ContactsContract.Intents.Insert.PHONE,
                            phone
                        )
                    }

                startActivity(intent)
            } else {
                Toast.makeText(
                    this,
                    "Enter both name and phone number",
                    Toast.LENGTH_SHORT
                ).show()
            }
        }

        btnFetchContacts.setOnClickListener {
            if (checkPermission()) {
                fetchContacts()
            } else {
                requestPermission()
            }
        }
    }

    private fun checkPermission(): Boolean {
        return ContextCompat.checkSelfPermission(
            this,
            Manifest.permission.READ_CONTACTS
        ) == PackageManager.PERMISSION_GRANTED
    }

    private fun requestPermission() {
        ActivityCompat.requestPermissions(
            this,
            arrayOf(Manifest.permission.READ_CONTACTS),
            permissionReqCode
        )
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

        if (requestCode == permissionReqCode) {
            if (
                grantResults.isNotEmpty() &&
                grantResults[0] == PackageManager.PERMISSION_GRANTED
            ) {
                Toast.makeText(
                    this,
                    "Permission Granted",
                    Toast.LENGTH_SHORT
                ).show()

                fetchContacts()
            } else {
                Toast.makeText(
                    this,
                    "Permission denied to read contacts",
                    Toast.LENGTH_SHORT
                ).show()
            }
        }
    }

    private fun fetchContacts() {
        val uri = ContactsContract.CommonDataKinds.Phone.CONTENT_URI

        val projection = arrayOf(
            ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME,
            ContactsContract.CommonDataKinds.Phone.NUMBER
        )

        val cursor: Cursor? = contentResolver.query(
            uri,
            projection,
            null,
            null,
            "${ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME} ASC"
        )

        if (cursor != null && cursor.count > 0) {
            val builder = StringBuilder()

            val nameIndex = cursor.getColumnIndexOrThrow(
                ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME
            )

            val numberIndex = cursor.getColumnIndexOrThrow(
                ContactsContract.CommonDataKinds.Phone.NUMBER
            )

            var count = 1

            while (cursor.moveToNext()) {
                val name = cursor.getString(nameIndex)
                val number = cursor.getString(numberIndex)

                builder.append(
                    "$count. $name\nPhone: $number\n\n"
                )

                count++
            }

            cursor.close()
            tvContactList.text = builder.toString()

            Toast.makeText(
                this,
                "Fetched ${count - 1} contacts successfully",
                Toast.LENGTH_SHORT
            ).show()
        } else {
            cursor?.close()
            tvContactList.text =
                "No contacts found in device phonebook."

            Toast.makeText(
                this,
                "Contacts list is empty",
                Toast.LENGTH_SHORT
            ).show()
        }
    }
}
```

### Required Permission in `AndroidManifest.xml`

Add this permission above the `<application>` tag:

```xml
<uses-permission android:name="android.permission.READ_CONTACTS" />
```

**Note:** The code uses the system contact-creation intent, so the user completes the addition in the Contacts application. Directly inserting contacts into the provider would require a different implementation and appropriate write permission.

---

## 5. Test Cases & Execution Verification

### Test Case 1: Student Identity and Runtime Permission

- **Action:** Launch the application and tap **Fetch Contacts**.
- **Expected Output:** The student information banner is displayed, and Android requests permission to read contacts.

### Test Case 2: Contact Insertion Using Intent

- **Action:** Enter a contact name and phone number, then tap **Add Contact**.
- **Expected Output:** The system contact-creation screen opens with the entered details pre-filled. Save the contact there to complete the operation.

### Test Case 3: Fetching Contacts Using ContentResolver

- **Action:** Grant contact permission and tap **Fetch Contacts**.
- **Expected Output:** The application displays available contact names and phone numbers, along with a toast showing the number of fetched phone entries.

---

## 6. Screenshots

### Screenshot 1: Student Identity and Contacts Interface

![Student identity and contacts interface](https://github.com/user-attachments/assets/f0121042-a124-414d-b1ac-2746e9678f8f)

### Screenshot 2: Contact Creation Screen

![Contact creation screen](https://github.com/user-attachments/assets/354c4161-3a25-44e7-964d-962fef939c8b)

### Screenshot 3: Contacts Display

![Fetched contacts display](https://github.com/user-attachments/assets/93152c74-c96f-4493-af13-5d6f07c879d9)

---

## 7. Result

The Android data-sharing application was implemented using Kotlin and the Contacts Content Provider. Runtime permission handling, contact creation through an implicit intent, and contact retrieval using `ContentResolver` were demonstrated successfully.
