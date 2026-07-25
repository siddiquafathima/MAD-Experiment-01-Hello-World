Experiment No. 1



Title:

Develop a Simple "Hello World" Android Application Using Kotlin



Aim:



To develop a simple Android application using Android Studio and Kotlin that displays the message "Hello World!" on the application screen.



Procedure:



1. Open Android Studio and create a new Android project.
2. Select Empty Views Activity as the project template.
3. Enter the project name as HelloWorldApp.
4. Select Kotlin as the programming language.
5. Configure the minimum SDK and create the project.
6. Open the MainActivity.kt file and configure the Activity to load the application layout.
7. Open the activity\_main.xml file located in the res/layout directory.
8. Create a LinearLayout as the main user interface container.
9. Add a TextView to display the message "Hello World!".
10. Set the layout gravity to center the message on the screen.
11. Build and run the application using an Android Emulator.
12. Verify that the application successfully displays the "Hello World!" message.
13. Add the student's name and USN to the application for identification.
14. Execute the test cases and capture screenshots of the application output.


Main Code Block:

override fun onCreate(savedInstanceState: Bundle?) {

&#x20;   super.onCreate(savedInstanceState)

&#x20;   setContentView(R.layout.activity\_main)

}

Explanation:



The onCreate() method is called when the MainActivity is created. The setContentView() method connects the MainActivity with the activity\_main.xml layout file. This allows the user interface designed in XML to be displayed on the application screen.

core XML snippet:

<TextView

&#x20;   android:layout\_width="wrap\_content"

&#x20;   android:layout\_height="wrap\_content"

&#x20;   android:text="Hello World!"

&#x20;   android:textSize="32sp"

&#x20;   android:textStyle="bold" />

Explanation:



The TextView widget is used to display text on the Android application screen. The android:text attribute specifies the message to be displayed, while android:textSize and android:textStyle control the appearance of the text.


TEP 23 — Draw the Wireframe



In your Observation Book, under Wireframe / UI Design, draw this:



┌───────────────────────────────┐

│          HelloWorldApp        │

├───────────────────────────────┤

│                               │

│                               │

│         Hello World!          │

│                               │

│       Name: Siddiqua Fathima  │

│         USN: 25MCAR0121       │

│                               │

│                               │

└───────────────────────────────┘



You can draw this neatly by hand using a ruler.



Below it, write:



Figure 1: Wireframe of Hello World Android Application



STEP 24 — Write the 3 Test Cases in Your Record



Write the following:



Test Case 1 — Application Launch



Test Case ID: TC01



Test: Verify that the application launches successfully.



Steps:



Launch the HelloWorldApp.

Observe the application screen.



Expected Result:

The application should launch successfully and display "Hello World!".



Actual Result:

The application launched successfully and displayed "Hello World!".



Status: PASS



Screenshot: test-case-1.png



Test Case 2 — Application Relaunch



Test Case ID: TC02



Test: Verify that the application can be relaunched successfully.



Steps:



Launch the application.

Close the application.

Launch the application again.



Expected Result:

The application should relaunch successfully and display "Hello World!".



Actual Result:

The application successfully relaunched and displayed "Hello World!".



Status: PASS



Screenshot: test-case-2.png



Test Case 3 — Name and USN Verification



Test Case ID: TC03



Test: Verify that the application displays the student's name and USN.



Steps:



Launch the application.

Observe the application screen.

Verify the displayed name and USN.



Expected Result:

The application should display the Hello World message along with the student's name and USN.



Actual Result:

The application successfully displayed Siddiqua Fathima and USN: 25MCAR0121 along with the Hello World message.



Status: PASS



Screenshot: test-case-3.png



STEP 25 — Verify Your Screenshot Folder



Open:



C:\\MAD\_LAB\\screenshots



Make sure you have these three files:



C:\\MAD\_LAB\\screenshots

│

├── test-case-1.png

├── test-case-2.png

└── test-case-3.png



Your test-case-3.png should be the screenshot we just verified showing:



Hello World!



Name: Siddiqua Fathima

USN: 25MCAR0121




