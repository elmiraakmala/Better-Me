# Better Me | 30-Day Challenge

A responsive HTML/CSS/JavaScript habit tracker for 12 Oct–10 Nov 2026. It works immediately with localStorage and optional Firebase cloud sync.

## Files
- `index.html`: UI and styles
- `app.js`: habit logic, curriculum, weekly review, backup/import, Firebase sync
- `firebase-config.js`: Firebase web config (null until configured)
- `firestore.rules`: secure per-user access rules

## Try it locally
Open `index.html` in a browser (local mode works without Firebase). For Firebase and a reliable local development setup, serve the folder using VS Code Live Server or `python -m http.server 8080` and open `http://localhost:8080`.

## Deploy to GitHub Pages
1. Create a **new repository** in GitHub (e.g. `better-me`).
2. Upload the four files from this folder to the repository root.
3. Go to **Settings > Pages > Build and deployment**. Select **Deploy from a branch**, branch **main**, folder **/(root)**, and Save.
4. Open the published `https://YOUR-USERNAME.github.io/better-me/` URL.
5. Initially, it works with browser-local storage. Use **Backup** to download JSON.

## Enable Firebase (free Spark plan)
1. Open https://console.firebase.google.com and create a Firebase project.
2. Under **Project settings > Your apps**, register a **Web app**. Copy the `firebaseConfig` object.
3. In `firebase-config.js`, replace `null` with the copied config object (not the `const firebaseConfig =` part).
4. In **Build > Authentication > Sign-in method**, enable **Email/Password**.
5. In **Build > Firestore Database**, create a database. For the location choose a nearby supported region. **Do not leave the database in test mode.**
6. In Firestore **Rules**, replace rules with the contents of `firestore.rules`, then Publish. This limits reads/writes to the authenticated user's own document.
7. In **Authentication > Settings > Authorized domains**, add `YOUR-USERNAME.github.io` if it is not already authorized.
8. Commit `firebase-config.js` to GitHub and open your site. In **Settings & Sync**, create an account or sign in. Cloud data will sync between devices using the same account.
9. If using it only yourself, **disable new sign-ups after creating your account** by changing your signup approach or restricting account creation; at minimum, keep secure Firestore rules. (The UI currently includes a Create account button for initial setup.)

**Important:** Firebase Web configuration is not a secret. Do not put admin service-account credentials, private keys, or GitHub tokens in the repository. Firestore security rules are mandatory. Firebase pricing/limits can change; review the Spark plan in the console.

## Tracking logic
- Modes are selected daily and control which optional activities are applicable.
- Foundation checkboxes: wake, five prayers, breakfast, sleep.
- Weekly success: >=80% average of 4 capped goal completions (English 5, Technical 2, Speaking 2, Exercise 4). English/technical/speaking are counted from completed curriculum lessons assigned to that week, not daily checkbox marks. Running counts toward exercise.
- Weekly streak counts consecutive **completed** successful weeks; closing days Nov 9–10 do not count as a full week.
- Daily progress is based on applicable checkboxes only. Lessons cycle Not Started → In Progress → Done → Not Started.
- A recovery day has only recovery as its optional applicable activity.
- For the Oct 26–Nov 1 release week, adjust targets manually in a future version if needed; this MVP retains standard weekly quotas.

## Privacy and backup
- Local mode saves to browser localStorage; clearing browser data removes it.
- Export a JSON backup regularly.
- Cloud mode stores one document at `users/{uid}/data/better-me`.
- When signing into a cloud account with existing local data, you will be asked whether to use local or cloud data. Export first if you need to preserve both.
