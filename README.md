# Team Scheduler - How to Use

This is a single-page team scheduler. Everyone works in a shared room, chooses available time slots, and sees team convergence in real time.

## 1) Sign in
Choose one of the sign-in methods in the Access panel:
- Google sign-in
- Email and password
- Email link (passwordless)

You must be signed in to view room data and save your selections.

## 2) Load a room
1) Enter the Room code / Team name.
2) Click **Load** to open the room.

Room codes are lowercased and spaces become hyphens.

## 3) Room settings (left panel)
These apply to everyone in the room:
- **Preferred time zone**: the room's default time base.
- **Scheduled dates**: date range for the scheduling period.
- **Meeting duration (minutes)**: default 60.
- **Expected team members**: optional count to track missing submissions.
- **Active days**: choose which weekdays are available.
- **Room status**: open or closed. Closing the room disables edits.

## 4) Pick your availability
- Select 30-minute slots in the grid.
- Use **Save / Update** to submit your picks.
- Use **Clear picks** to remove your selections.

## 5) Time display
- The toggle "Showing times in" switches the bold time between your local time and the room's default time.
- Each slot shows two subtitle rows: local time and room time.

## 6) Comments
Click **Comments** (next to the day pills) to add notes for the room.
Your comments are saved with your entry and shown under your slots in the team list.

## 7) Ranking vs heatmap
At the bottom of the scheduler:
- **Graph** (default) shows a heatmap of availability. The star means full-team availability.
- **Ranking** shows the top 7 most chosen time slots.

## 8) Team selections list
The team list shows each member's:
- Name and email
- Time zone (with GMT offset)
- Selected slots
- Comments (if any)

## 9) Export and reset
- **Export JSON** downloads the current room data.
- **Reset room (admin only)** clears all member entries for the room.
  Admins must be configured in Firebase.

## Firebase setup
See `FIREBASE_SETUP.md` for Firebase configuration, rules, and admin setup.
