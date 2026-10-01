# Experiment 09 – Data Persistence using SharedPreferences and SQLite

## 📌 Experiment Title

**Develop an Android application that demonstrates data persistence by using SharedPreferences for storing key-value pairs and SQLite for creating and managing databases.**

---

## 🎯 Objective

To develop an Android application that demonstrates persistent data storage
using **SharedPreferences** and **SQLite Database**.

The application allows the user to:

- Save username and password persistently.
- Automatically restore saved credentials.
- Store every successful login in SQLite.
- Maintain multiple login records.
- Display stored records from the SQLite database.
- Logout without deleting saved database records.

---

## 📱 Application Overview

**Application Name:** Experiment 9 – Data Persistence

This Android application demonstrates two different methods of data
persistence:

### SharedPreferences
Used for storing the latest username and password as key-value pairs and
restoring them automatically when the application is opened again.

### SQLite Database
Used for permanently maintaining multiple login records. Every successful
login is stored as a separate database record.

---

## ✨ Features

### 🔐 1. Persistent Login Credentials

The application saves the entered username and password using
**SharedPreferences**.

When the application is reopened, the saved credentials are automatically
restored into the login fields.

### ⚡ 2. Automatic Credential Fill

Previously saved username and password are automatically filled in the
login screen.

### 🗄️ 3. SQLite Database

Every successful login is inserted into the SQLite database.

Each record contains:

- ID
- Username
- Password
- Saved Date and Time

Previous records are not replaced when a new login is performed.

### 🚪 4. Logout

A Logout button is available on the dashboard.

Logging out returns the user to the login screen while the SQLite records
remain stored.

### 📊 5. Database Records

The dashboard displays all login records stored in the SQLite database.

The total number of stored records is also displayed.

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Android Studio | Application development |
| Kotlin | Programming language |
| XML | User interface design |
| SharedPreferences | Key-value persistent storage |
| SQLite | Local database |
| SQLiteOpenHelper | Database creation and management |
| Android SDK | Application framework |

---

## 🧩 Main Components

| Component | Function |
|---|---|
| `MainActivity.kt` | Handles login and application logic |
| `SharedPreferences` | Saves and restores credentials |
| `SQLiteOpenHelper` | Creates and manages SQLite database |
| `EditText` | Accepts username and password |
| `Button` | Handles Login and Logout actions |
| SQLite Table | Stores multiple login records |
| Dashboard | Displays persisted database records |

---

## 🔄 Application Workflow

```text
                START
                  │
                  ▼
             Login Screen
                  │
                  ▼
      Restore saved credentials
       from SharedPreferences
                  │
                  ▼
        Enter Username/Password
                  │
                  ▼
       Login & Save Credentials
                  │
          ┌───────┴────────┐
          ▼                ▼
  SharedPreferences       SQLite
          │                │
   Save latest data    Insert new record
          │                │
          └───────┬────────┘
                  ▼
              Dashboard
                  │
                  ▼
        Display SQLite Records
                  │
                  ▼
               Logout
                  │
                  ▼
            Login Screen
