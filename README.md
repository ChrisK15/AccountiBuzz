# AccountiBuzz

A React Native accountability app built with Expo and Firebase.

## Prerequisites

- [Node.js](https://nodejs.org) v18+
- [Expo Go](https://expo.dev/client) app on your phone (for testing)

## Getting Started

### 1. Clone & install

```bash
git clone <repo-url>
cd accountibuzz
npm install
```

### 2. Set up environment variables

```bash
cp .env.example .env
```

Open `.env` and fill in your Firebase credentials. You can find these in the [Firebase Console](https://console.firebase.google.com) under your project → Project Settings → Your apps.

```
EXPO_PUBLIC_FIREBASE_API_KEY=
EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN=
EXPO_PUBLIC_FIREBASE_PROJECT_ID=
EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET=
EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
EXPO_PUBLIC_FIREBASE_APP_ID=
```

### 3. Firebase setup

You need a Firebase project with the following enabled:

- **Authentication** → Email/Password sign-in
- **Firestore Database** → created in test mode
- **Storage** → created in test mode (requires Blaze plan)

### 4. Run the app

```bash
npm start
```

Scan the QR code with Expo Go on your phone, or press `a` for Android emulator / `i` for iOS simulator.

## Project Structure

```
src/
├── screens/       # All app screens
├── components/    # Reusable UI components
├── navigation/    # React Navigation setup
├── services/      # Firebase service layer
├── hooks/         # Custom React hooks
├── context/       # Auth and network context
├── types/         # TypeScript type definitions
└── utils/         # Helper functions
```

## Tech Stack

- **Framework:** React Native + Expo SDK 54
- **Language:** TypeScript
- **Auth & DB:** Firebase (Auth, Firestore, Storage)
- **Navigation:** React Navigation v7
- **Forms:** React Hook Form + Zod
