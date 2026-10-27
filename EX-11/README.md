# EXP11: Google Maps Android Application

A Mobile Application Development lab experiment demonstrating the integration of Google Maps into an Android application using Kotlin and the Google Maps SDK for Android. The application displays an interactive map with a custom location marker, camera positioning, zoom controls, and compass functionality.

---

## 1. Aim

To develop an Android application that integrates Google Maps and displays an interactive map with custom markers, zoom controls, compass, and appropriate edge-to-edge UI handling.

## 2. Features

- **Interactive Google Map:** Displays map tiles using Google Maps SDK for Android.
- **Custom Marker:** Places a marker at a specified location, such as Sydney.
- **Camera Positioning:** Centers the map on the selected location with a customized zoom level.
- **Zoom Controls:** Allows users to zoom in and out.
- **Compass:** Displays the compass control for map orientation.
- **Edge-to-Edge UI:** Supports modern Android system bar and window inset handling.

## 3. Technologies Used

- **Language:** Kotlin
- **IDE:** Android Studio
- **Maps SDK:** Google Maps SDK for Android
- **UI Design:** XML Layouts
- **Map Component:** `SupportMapFragment`
- **Callback:** `OnMapReadyCallback`
- **Version Control:** Git and GitHub

## 4. Project Structure

```text
EXP11-GoogleMaps-App/
├── app/
│   ├── src/
│   │   └── main/
│   │       ├── java/
│   │       │   └── com/example/googlemapsapp/
│   │       │       └── MainActivity.kt
│   │       ├── res/
│   │       │   └── layout/
│   │       │       └── activity_main.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts
├── build.gradle.kts
├── settings.gradle.kts
└── README.md
```

*Note: The package name and project structure may differ from your actual Android Studio project.*

## 5. Google Maps API Configuration

A Google Maps API key is required to display map tiles.

1. Open the [Google Cloud Console](https://console.cloud.google.com/).
2. Create or select a Google Cloud project.
3. Enable **Maps SDK for Android**.
4. Navigate to **APIs & Services → Credentials**.
5. Create an API key.
6. Add the following metadata inside the `<application>` tag in `AndroidManifest.xml`:

```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="YOUR_ACTUAL_API_KEY" />
```

Replace `YOUR_ACTUAL_API_KEY` with your own API key. Configure appropriate application restrictions and API restrictions. Google Maps may also require billing to be enabled for the Cloud project.

**Security:** Do not publish your actual API key in a public GitHub repository.

## 6. Building and Running

1. Open the project in Android Studio.
2. Sync the project using **File → Sync Project with Gradle Files**.
3. Ensure the Google Maps API key is configured correctly.
4. Verify that the Maps SDK dependency is included in the app-level Gradle file.
5. Run the application on an Android emulator with Google Play services or a compatible physical Android device.
6. Check that the map loads correctly and the marker and map controls appear.

## 7. Screenshot

### Google Maps Application Output

The screenshot below shows the application running with its Google Maps interface.

![Google Maps Android Application Output](https://github.com/user-attachments/assets/ca685802-65b0-410f-847c-ca0856371511)

## 8. Git & GitHub

Follow these commands to push the project to your GitHub repository.

```bash
git init
git add .
git commit -m "Add EXP11 Google Maps Android Application"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/EXP11-GoogleMaps-App.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username. If a remote repository is already configured, skip `git remote add origin` and verify the existing remote using:

```bash
git remote -v
```

## 9. Result

The Google Maps Android application was developed using Kotlin and the Google Maps SDK for Android. The application demonstrates interactive map integration, marker placement, camera positioning, zoom controls, and compass functionality.

