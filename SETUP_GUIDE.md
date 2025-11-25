# Setup Instructions

This guide will help you set up and run the EarlySignal project locally.

---

## Prerequisites

Before you begin, ensure you have the following installed:
- **Flutter SDK** (3.0 or higher) - [Install Flutter](https://docs.flutter.dev/get-started/install)
- **Python** (3.9 or higher) - [Download Python](https://www.python.org/downloads/)
- **Node.js & npm** - [Download Node.js](https://nodejs.org/)
- **Firebase CLI** - Install via `npm install -g firebase-tools`
- **Google Cloud Platform (GCP) account** - [Create account](https://cloud.google.com/)
- **Firebase account** - [Create account](https://firebase.google.com/)
- **Android Studio** - [Install Android Studio](https://developer.android.com/studio)

---

## Required Credentials

You'll need to obtain several API keys and configuration files before running the app.

### 1. Google Cloud Platform (GCP)

#### Service Account Key (`service_account_key.json`)

1. Go to [GCP Console](https://console.cloud.google.com/)
2. Select or create a project
3. Navigate to **IAM & Admin** → **Service Accounts**
4. Click **Create Service Account**
5. Give it a name (e.g., `earlysignal-backend`)
6. Grant the following roles:
   - **BigQuery Admin**
   - **Cloud Datastore User**
7. Click **Done**, then click on the service account you just created
8. Go to **Keys** tab → **Add Key** → **Create New Key**
9. Choose **JSON** format and download
10. Rename the file to `service_account_key.json`

---

### 2. Firebase

#### Firebase Service Account Key (`firebase_service_account.json`)

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project (or create a new one)
3. Click the gear icon → **Project Settings**
4. Navigate to **Service Accounts** tab
5. Click **Generate New Private Key**
6. Download the JSON file
7. Rename it to `firebase_service_account.json`

#### Firebase Config Files for Flutter

**For Android** (`google-services.json`):
1. In Firebase Console → **Project Settings**
2. Scroll down to **Your apps** section
3. Click on your Android app (or click **Add app** if you haven't registered one)
4. Follow the setup wizard:
   - **Android package name**: `com.example.early_signal` (or your app's package name)
   - **App nickname**: EarlySignal (optional)
   - **Debug signing certificate SHA-1**: (optional, for Google Sign-In)
5. Click **Register app**
6. Download `google-services.json`

**For iOS** (`GoogleService-Info.plist`):
1. In Firebase Console → **Project Settings**
2. Scroll down to **Your apps** section
3. Click on your iOS app (or click **Add app** to register one)
4. Follow the setup wizard:
   - **iOS bundle ID**: `com.example.earlySignal` (or your app's bundle ID)
   - **App nickname**: EarlySignal (optional)
5. Click **Register app**
6. Download `GoogleService-Info.plist`

> **Important:** Enable **Authentication** (Email/Password) and **Firestore Database** in your Firebase project.

---

### 3. Google Maps & Places API

#### API Key (for Geocoding, Places, and Maps)

1. Go to [GCP Console](https://console.cloud.google.com/)
2. Select your project
3. Navigate to **APIs & Services** → **Credentials**
4. Click **Create Credentials** → **API Key**
5. Copy the API key
6. Click **Restrict Key** (recommended):
   - **API restrictions**: Select these APIs:
     - Maps SDK for Android
     - Maps SDK for iOS
     - Geocoding API
     - Places API
   - **Application restrictions**: Set based on your needs (Android/iOS apps)
7. Click **Save**

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/SumaRagi07/Early_Signal.git
cd Early_Signal_Backend      # For backend setup and running
cd Early_Signal_Frontend     # Open this folder in Android Studio

```

---

### 2. Backend Setup

#### Step 1: Add Credential Files

Place the following files in the `Early_Signal_Backend/` directory:

```
Early_Signal_Backend/
├── service_account_key.json       # GCP service account key
├── firebase_service_account.json  # Firebase service account key
└── ...
```

#### Step 2: Install Python Dependencies

```bash
cd Early_Signal_Backend
pip install -r requirements.txt
```

#### Step 3: Run the Backend Server

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

The backend API should now be running at `http://localhost:8000`


---

### 3. Frontend Setup

#### Step 1: Add Firebase Config Files

Place the downloaded Firebase config files in the correct locations:

```
Early_Signal_Frontend/
├── android/app/google-services.json          # Android config
├── ios/Runner/GoogleService-Info.plist       # iOS config
└── ...
```

#### Step 2: Configure Google Maps API Key

Open `Early_Signal_Frontend/functions/index.js` and replace `"paste_key_here"` with your actual API key:

```javascript
// Before:
const GOOGLE_MAPS_API_KEY = "paste_key_here";

// After:
const GOOGLE_MAPS_API_KEY = "AIz.......";  // Your actual key
```

#### Step 3: Install Flutter Dependencies

```bash
cd Early_Signal_Frontend
flutter pub get
```

#### Step 4: Configure Firebase for Flutter

```bash
flutterfire configure
```

This command will:
- Auto-generate `lib/firebase_options.dart`
- Link your Flutter app to your Firebase project

#### Step 5: Deploy Cloud Functions

```bash
cd functions
firebase login          # Login to Firebase (first time only)
firebase deploy --only functions
```

#### Step 6: Run the Flutter App

Connect a device or start an emulator, then run:

```bash
cd ..  # Go back to Early_Signal_Frontend directory
flutter run
```

---

## Security Best Practices

### Files to NEVER Commit to GitHub

The following files contain sensitive credentials and should **never** be committed:

```
Early_Signal_Backend/service_account_key.json
Early_Signal_Backend/firebase_service_account.json
Early_Signal_Frontend/android/app/google-services.json
Early_Signal_Frontend/ios/Runner/GoogleService-Info.plist
Early_Signal_Frontend/functions/index.js (if it contains hardcoded API keys)
```

### Update Your `.gitignore`

Make sure your `.gitignore` includes:

```gitignore
# Backend credentials
Early_Signal_Backend/service_account_key.json
Early_Signal_Backend/firebase_service_account.json

# Frontend credentials
Early_Signal_Frontend/android/app/google-services.json
Early_Signal_Frontend/ios/Runner/GoogleService-Info.plist
Early_Signal_Frontend/lib/firebase_options.dart

# Environment files
*.env
.env.local
```

---

## Quick Setup Checklist

Use this checklist to ensure you've completed all steps:

**GCP & Firebase Setup:**
- [ ] Created GCP project
- [ ] Downloaded `service_account_key.json`
- [ ] Downloaded `firebase_service_account.json`
- [ ] Downloaded `google-services.json` (Android)
- [ ] Downloaded `GoogleService-Info.plist` (iOS)
- [ ] Created Google Maps API key
- [ ] Enabled required APIs (BigQuery, Firestore, Geocoding, Places, Maps)

**Backend Setup:**
- [ ] Cloned repository
- [ ] Placed credential files in `Early_Signal_Backend/`
- [ ] Installed Python dependencies (`pip install -r requirements.txt`)
- [ ] Started backend server (`uvicorn main:app --reload`)
- [ ] Verified backend is running at `http://localhost:8000`

**Frontend Setup:**
- [ ] Placed Firebase config files in `Early_Signal_Frontend/`
- [ ] Updated Google Maps API key in `functions/index.js`
- [ ] Installed Flutter dependencies (`flutter pub get`)
- [ ] Ran `flutterfire configure`
- [ ] Deployed Cloud Functions (`firebase deploy --only functions`)
- [ ] Ran Flutter app (`flutter run`)

---

## Project Structure

```
Early_Signal/
├── Early_Signal_Backend/              # Python FastAPI backend
│   ├── main.py                        # Main API application
│   ├── requirements.txt               # Python dependencies
│   ├── service_account_key.json       # ADD THIS (GCP credentials)
│   ├── firebase_service_account.json  # ADD THIS (Firebase credentials)
│   └── ...
│
├── Early_Signal_Frontend/             # Flutter mobile app
│   ├── lib/                           # Dart source code
│   ├── android/
│   │   └── app/
│   │       └── google-services.json   # ADD THIS (Android Firebase config)
│   ├── ios/
│   │   └── Runner/
│   │       └── GoogleService-Info.plist  # ADD THIS (iOS Firebase config)
│   ├── functions/                     # Firebase Cloud Functions
│   │   └── index.js                   # UPDATE API KEY HERE
│   └── ...
│
├── Misc_Documents/                    # Project documentation
└── README.md                          # Main project README
```

---

## Troubleshooting

### Backend Issues

**"Module not found" errors:**
```bash
pip install -r requirements.txt --upgrade
```

**Port 8000 already in use:**
```bash
# Use a different port
uvicorn main:app --reload --port 8001
```

### Frontend Issues

**"google-services.json not found":**
- Ensure the file is placed in `android/app/` directory
- Clean and rebuild: `flutter clean && flutter pub get`

**Firebase deployment fails:**
```bash
firebase login --reauth
firebase use --add  # Select your project
```

**Flutter build errors:**
```bash
flutter clean
flutter pub get
flutter pub upgrade
```

---

### You're All Set!

## Happy coding!
