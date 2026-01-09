# Team Scheduler (Single-File)

## Firebase setup (first time)
1) Go to https://console.firebase.google.com and sign in with a Google account.
2) Click "Add project", enter a name, optionally disable Google Analytics, then click "Create project".
3) In the left sidebar, go to Build -> Firestore Database, click "Create database", choose a mode (test or production), pick a region, then click "Enable".
4) Back in Project Overview, click the web icon (`</>`) to add a Web App.
5) Give the app a nickname, leave hosting unchecked, and click "Register app".
6) Copy the firebaseConfig object shown and paste it into `index.html` at:
   `const firebaseConfig = { /* TODO: paste Firebase config here */ };`
7) Save `index.html` and reload the page.

## Authentication setup (required)
1) In Firebase Console, go to Build -> Authentication and click "Get started".
2) Under Sign-in method, enable:
   - **Email/Password**
   - **Email link (passwordless)** (enable the toggle)
   - **Google** (choose a support email and save)
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
    match /rooms/{roomId}/members/{memberId} {
      allow read, create, update: if request.auth != null
        && request.resource.data.keys().hasOnly(['name','email','tz','slots','updatedAt']);
      allow delete: if false;
    }
  }
}
```

## GitHub Pages hosting
1) Push this repo to GitHub.
2) In GitHub, go to Settings -> Pages.
3) Select "Deploy from a branch", choose your branch (e.g., `main`) and `/root`.
4) Save, then wait for the Pages URL to appear.
