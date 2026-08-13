
Experiment 04: Linking Activities Using Intents
Objective
Implement an Android application to demonstrate linking activities using Intents.

Description
This Android application demonstrates how to link two activities using an Explicit Intent.

The application contains a login screen where the user enters a username and password. After clicking the Login button, the application navigates from MainActivity to HomeActivity using an Intent.

The username entered by the user is passed from MainActivity to HomeActivity using putExtra() and displayed on the home screen.

Features
Attractive login interface
Username input
Password input
Input validation
Login button
Activity navigation using Explicit Intent
Passing data between activities
Personalized welcome message
Technologies Used
Android Studio
Kotlin
XML
Android SDK
Explicit Intent
AppCompatActivity
Application Flow
Login Screen
↓
Enter Username & Password
↓
Click Login
↓
Explicit Intent
↓
HomeActivity
↓
Display Username

Activities
MainActivity
Handles the login screen, validates the username and password, and starts HomeActivity using an Explicit Intent.

val intent = Intent(this, HomeActivity::class.java)
intent.putExtra("username", userName)
startActivity(intent)
Demo Video
🎥 Click here to watch the Experiment 04 Demo

https://github.com/user-attachments/assets/b2e23ef5-8d6e-4dda-bba9-bd336a5f0dde

