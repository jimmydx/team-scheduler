# Firebase Setup and Admin Guide

## Firebase setup (first time)
1) Go to https://console.firebase.google.com and sign in with a Google account.
2) Click "Add project", enter a name, optionally disable Google Analytics, then click "Create project".
3) In the left sidebar, go to Build -> Firestore Database, click "Create database", choose a mode (test or production), pick a region, then click "Enable".
4) Back in Project Overview, click the web icon (</>) to add a Web App.
5) Give the app a nickname, leave hosting unchecked, and click "Register app".
6) Copy the firebaseConfig object shown and paste it into `index.html` at:
   `const firebaseConfig = { /* TODO: paste Firebase config here */ };`
7) Save `index.html` and reload the page.

## Authentication setup (required)
1) In Firebase Console, go to Build -> Authentication and click "Get started".
2) Under Sign-in method, enable:
   - Email/Password
   - Email link (passwordless)
   - Google (choose a support email and save)
3) In Authentication -> Settings -> Authorized domains, add any domains you will use:
   - `localhost` (local testing)
   - your GitHub Pages domain (e.g., `yourname.github.io`)
4) Email link sign-in requires an http(s) URL. If you are testing locally, open the page via a local server (not `file://`), for example:
   `python3 -m http.server`
   then visit `http://localhost:8000`

## Firestore rules (auth required, allowed fields only)
These rules require sign-in and only allow the expected fields to be written.

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isSignedIn() {
      return request.auth != null;
    }
    function isRoomAdmin(roomId) {
      return isSignedIn()
        && request.auth.token.email != null
        && get(/databases/$(database)/documents/rooms/$(roomId)).data.admins != null
        && get(/databases/$(database)/documents/rooms/$(roomId)).data.admins.hasAny([request.auth.token.email]);
    }

    match /rooms/{roomId} {
      allow read: if isSignedIn();
      allow create: if isSignedIn()
        && request.resource.data.keys().hasOnly(['days','preferredTz','dateStart','dateEnd','meetingMinutes','expectedMembers','updatedAt','isOpen','statusUpdatedAt','statusByName','statusByEmail']);
      allow update: if isSignedIn()
        && request.resource.data.keys().hasOnly(['days','preferredTz','dateStart','dateEnd','meetingMinutes','expectedMembers','updatedAt','isOpen','statusUpdatedAt','statusByName','statusByEmail','admins'])
        && (!resource.data.keys().hasAny(['admins']) || request.resource.data.admins == resource.data.admins);
      allow delete: if isRoomAdmin(roomId);
    }
    match /rooms/{roomId}/members/{memberId} {
      allow read: if isSignedIn();
      allow create, update: if isSignedIn()
        && request.resource.data.keys().hasOnly(['name','email','tz','slots','comments','updatedAt']);
      allow delete: if isRoomAdmin(roomId);
    }
  }
}
```

Room days are saved in the room document (`rooms/{roomId}`) as `days: ["mon","tue",...]` and can be edited from the UI.
Room scheduling settings are saved as `preferredTz` (IANA time zone), `dateStart`/`dateEnd` (YYYY-MM-DD), `meetingMinutes` (integer minutes), and `expectedMembers` (integer).
Member submissions also include an optional `comments` field.
Room open/closed status is saved in the room document as `isOpen` with `statusByName`, `statusByEmail`, and `statusUpdatedAt`.

## Admin reset (optional)
To enable the "Reset room" button, add an `admins` array to the room document in Firestore (lowercase emails):

```json
admins: ["you@example.com"]
```

If you don't need admin controls, you can leave `admins` unset. Room settings updates will still work.

Sign in with one of those emails; the Reset button will be enabled and will delete all member entries for that room.
