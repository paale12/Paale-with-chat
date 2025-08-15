# Private 2-Person Realtime Chat — Firebase

This project is a **minimal single-file HTML chat app** built on **Firebase Firestore** for real-time messaging between exactly two users.

---
## Features
- **Realtime** 2-user chat with room support
- Email/password authentication
- Room invite codes
- Export chat as JSON
- Recent rooms list in local storage
- Secure with **Firestore rules** (only your accounts can read/write)
- Single HTML file — easy to host on GitHub Pages or locally

---
## Setup Instructions
1. **Create a Firebase project** at https://console.firebase.google.com/
2. **Enable Authentication** → Sign-in Method → Email/Password.
3. **Enable Firestore Database** in test mode, then switch to rules (see below).
4. Go to **Project Settings** → **General** → **Your apps** → **Web app** → Copy config JSON.
5. Paste your config JSON into the app's **Firebase Config** textarea and click **Apply Config**.
6. Sign in or create two accounts (yourself + your friend).
7. Create a room code (e.g. `room-paale`) and share with your friend.
8. Both join the same room — start chatting!

---
## Hosting on GitHub Pages
1. Download `index.html` from this project.
2. Create a new GitHub repository and push `index.html`.
3. Go to **Settings** → **Pages** → Set branch to `main` (root) → Save.
4. Your chat app will be live at `https://<username>.github.io/<repo>/`.

---
## Firestore Security Rules (2-User Private Room)
Replace `UID_USER_1` and `UID_USER_2` with your actual Firebase Auth UIDs.

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /rooms/{roomId} {
      allow read, write: if request.auth != null && (
        request.auth.uid == 'UID_USER_1' ||
        request.auth.uid == 'UID_USER_2'
      );
      match /messages/{msgId} {
        allow read, create: if request.auth != null && (
          request.auth.uid == 'UID_USER_1' ||
          request.auth.uid == 'UID_USER_2'
        );
        allow update, delete: if false;
      }
    }
  }
}
```

---
## License
This project is provided "as is" for educational purposes.
