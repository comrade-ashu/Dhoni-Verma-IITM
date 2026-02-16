# Firebase Setup Guide

To make the Login and Database features work, you need to link this project to your Firebase account.

## Step 1: Create a Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/).
2. Click **"Add project"** and generate a name (e.g., "ViratIITM").
3. Disable Google Analytics (optional) and Create Project.

## Step 2: Register the App
1. Inside your project, click the **Web icon (</>)**.
2. Register the app (e.g., "ViratWebsite").
3. You will see a `const firebaseConfig = { ... }` object. **Copy this.**

## Step 3: Enable Authentication
1. Go to **Authentication** > **Get Started**.
2. Enable **Email/Password**.
3. Enable **Google** (Save the default settings).

## Step 4: Enable Firestore Database
1. Go to **Firestore Database** > **Create Database**.
2. Select a location (e.g., `asia-south1` or `us-central1`).
3. Start in **Test Mode** (for development) or **Production Mode**.

## Step 5: Update the Code
1. Open the file: `js/firebase-config.js`
2. Paste your copied `firebaseConfig` keys into the file, replacing the placeholders.

## Step 6: Set Secured Database Rules
To prevent the "security rules are defined as public" warning and protect your data, use these rules:
1. Go to **Firestore Database** > **Rules**.
2. Paste the following code:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Helper function to check if user is your specific email
    function isAdmin() {
      return request.auth != null && request.auth.token.email == 'aashishsinghsr@gmail.com';
    }

    // Resources & Videos: Everyone can read, Only Admin can write
    match /resources/{docId} {
      allow read: if true;
      allow write: if isAdmin();
    }
    
    match /videos/{docId} {
      allow read: if true;
      allow write: if isAdmin();
    }
  }
}
```
3. Click **Publish**.

## Done!
Reload the website. You should now be able to Login, and if you log in with your admin email (`aashishsinghsr@gmail.com`), you can add content safely.
