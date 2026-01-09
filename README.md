# Team Scheduler (Single-File)

## Firebase setup (first time)
1) Go to https://console.firebase.google.com and sign in with a Google account.
2) Click "Add project", enter a name, optionally disable Google Analytics, then click "Create project".
3) In the left sidebar, go to Build -> Firestore Database, click "Create database", choose "Start in test mode" (or "Production" if you will paste the open rules below), pick a region, then click "Enable".
4) Back in Project Overview, click the web icon (`</>`) to add a Web App.
5) Give the app a nickname, leave hosting unchecked, and click "Register app".
6) Copy the firebaseConfig object shown and paste it into `index.html` at:
   `const firebaseConfig = { /* TODO: paste Firebase config here */ };`
7) Save `index.html` and reload the page.

## Firestore rules (open read/write)
Use only for small, trusted teams. Open rules are not safe for production.
If you started in test mode, rules expire after 30 days, so switch to explicit rules for ongoing use.

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /rooms/{roomId}/members/{memberId} {
      allow read, write: if true;
    }
  }
}
```

## GitHub Pages hosting
1) Push this repo to GitHub.
2) In GitHub, go to Settings -> Pages.
3) Select "Deploy from a branch", choose your branch (e.g., `main`) and `/root`.
4) Save, then wait for the Pages URL to appear.
