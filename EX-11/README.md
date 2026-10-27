EXP11 - Google Maps Android Application
A modern Android application developed in Kotlin that integrates Google Maps to display an interactive map with custom markers, zoom controls, compass, and edge-to-edge window insets handling.

Features
Interactive Google Map: Powered by SupportMapFragment and Google Play Services Maps.
Custom Markers & Camera Control: Displays a marker at a designated location (e.g., Sydney) with customized zoom and camera positioning.
Map UI Controls: Enabled zoom controls and compass.
Edge-to-Edge UI: Designed with modern Android edge-to-edge system bar insets handling.
Project Structure
MainActivity.kt: Hosts the map fragment and implements OnMapReadyCallback for map initialization and marker configuration.
activity_main.xml: Contains the FragmentContainerView layout for the map.
AndroidManifest.xml: Configures internet permissions, location permissions, and the Google Maps API key metadata.
Setup & Configuration
1. Google Maps API Key
To view map tiles, you must provide your own Google Maps API Key:

Go to the Google Cloud Console.
Create a project and enable the Maps SDK for Android.
Generate an API Key under APIs & Services > Credentials.
Open app/src/main/AndroidManifest.xml and insert your API key:
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="YOUR_ACTUAL_API_KEY" />
Building and Running
Open the project in Android Studio.
Sync the project with Gradle files (File > Sync Project with Gradle Files).
Run the app on an Android Emulator or physical device supporting Google Play Services.
Version Control (Git & GitHub)
To push this project to GitHub:

git init
git add .
git commit -m "Initial commit - Google Maps Android App"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/EXP11-GoogleMaps-App.git
git push -u origin main
images
