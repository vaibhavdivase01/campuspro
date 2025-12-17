# CampusPro (Frontend only)

Pure HTML + Bootstrap + JavaScript app wired to your Firebase Realtime Database via REST.

Database URL used: https://campuspro-891e7-default-rtdb.firebaseio.com/

## Run
- Open `index.html` in your browser (no build step needed).
- Optionally host with any static server.

## Optional Auth
If your Firebase rules require auth, expose a REST token before loading JS:
```html
<script>
  window.CAMPUSPRO_DB_AUTH = "YOUR_FIREBASE_REST_TOKEN";
</script>
<script src="assets/js/app.js"></script>
```

## Data Model (paths)
- `/teachers/{teacherId}` → `{ name }`
- `/classes/{classId}` → `{ name, teacherId }`
- `/teachers/{teacherId}/classes/{classId}` → `true`
- `/students/{classId}/{roll}` → `{ name }`
- `/notes/{classId}/{noteId}` → `{ title, link, createdAt }`
- `/results/{classId}/{roll}/{exam}` → `{ score, updatedAt }`
- `/attendance/{classId}/{yyyy-mm-dd}/{roll}` → `true|false`
- `/attendanceSummary/{classId}/{yyyy-mm}/{roll}` → `{ present, total }`

## Workflows
- Admin: create teacher and class, see counts and lists.
- Teacher: load by `Teacher ID` and `Class ID`; add students; add notes/links; add results; mark daily attendance; publish monthly summaries.
- Student: login via `Class ID`, `Roll`, `Name` (must match teacher entry); view notes, results; load attendance summary for a month.

## Monthly Summary
- Teacher presses "Publish Monthly Summary". App aggregates `/attendance/{classId}/{yyyy-mm-*}` and writes totals to `/attendanceSummary/{classId}/{yyyy-mm}`.

## Firebase Rules (example for testing)
For open testing (NOT for production):
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
For secured use, require auth and set `window.CAMPUSPRO_DB_AUTH`.

## Notes
- No file uploads; notes are saved as title + URL links (e.g., Google Drive).
- All logic is client-side; ensure rules align with your security needs.
