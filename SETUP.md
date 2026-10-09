# Setting up My Planner

About 20 minutes, done once on a laptop. Everything here is free and needs no card.

You'll use two free services:

- **Firebase** (Google) stores your data and keeps your phone and iPad in sync.
- **GitHub Pages** hosts the app so it has its own web address.

---

## 0. Save your data from the Claude version (if you've used it)

Open the Claude version → **Customise** → **Sync & backup** → **Save a backup**. Keep the file; you'll restore it at the end.

---

## 1. Create a Firebase project

1. Go to **console.firebase.google.com** and sign in with a Google account.
2. Click **Create a project**, name it something like `my-planner`.
3. Turn **Google Analytics off** (not needed), then create it.

This puts you on the free **Spark** plan automatically.

## 2. Turn on email sign-in

1. In the left menu: **Build → Authentication → Get started**.
2. Under **Sign-in method**, choose **Email/Password**, switch the first toggle on, and **Save**.

## 3. Create the database

1. Left menu: **Build → Firestore Database → Create database**.
2. If asked for an edition, pick **Standard**.
3. Location: **europe-west2 (London)**. This can't be changed later.
4. Start in **production mode** and create it.
5. Open the **Rules** tab, delete everything there, paste in the contents of `firestore.rules` from this folder, and click **Publish**.

## 4. Connect the app to Firebase

1. Click the **gear icon → Project settings**.
2. Under **Your apps**, click the **web icon `</>`**, give it a nickname (e.g. `planner`), leave Hosting unticked, and **Register app**.
3. Firebase shows a block of code with `const firebaseConfig = { ... }`. You only need the part inside the curly brackets.
4. Open `index.html` from this folder in a text editor. Near the top you'll find:

   ```
   window.FIREBASE_CONFIG = {
     apiKey: "PASTE_YOUR_API_KEY",
     ...
   };
   ```

   Replace the lines between `{` and `}` with the ones Firebase gave you, then save.

These values aren't secret. They only identify your project. Your data is protected by the sign-in and the rules from step 3.

## 5. Put it online with GitHub Pages

1. Make a free account at **github.com**.
2. Click **+ → New repository**. Name it `planner`, set it to **Public**, tick **Add a README**, and create it.
3. Click **Add file → Upload files**. Drag in **everything inside this folder** (the files themselves, not the folder), then click **Commit changes**.
4. Go to the repository's **Settings → Pages**. Under **Build and deployment**, set Source to **Deploy from a branch**, branch **main**, folder **/ (root)**, and **Save**.
5. After a minute or two, the page shows your address: `https://YOUR-USERNAME.github.io/planner/`

## 6. Allow that address in Firebase

Back in Firebase: **Authentication → Settings → Authorised domains → Add domain**, and enter `YOUR-USERNAME.github.io` (no `https://`, no `/planner`).

## 7. Install it on your phone and iPad

On each device:

1. Open your address in **Safari**.
2. Tap **Share → Add to Home Screen**.
3. Open it from the home screen. It runs full screen, like a normal app.
4. The first time, enter your email and a password and tap **Create account**. On the other device, use the same details and tap **Sign in**.

The home-screen app keeps its own sign-in, separate from Safari, so sign in from the home-screen icon.

## 8. Bring your data across

In the new app: **Customise → Sync & backup → Restore from file**, and choose the backup from step 0.

---

## Updating the app later

When you get a new `index.html`, copy your `window.FIREBASE_CONFIG` block into it first. Then, in GitHub, open the repository, click **Add file → Upload files**, drag in the new `index.html`, and commit. Your data isn't touched. Close and reopen the app to see the change.

## Optional: lock it to just you

Right now anyone who found your address could create their own (separate, empty) account on your project. They could never see your data, but you can stop sign-ups entirely:

1. In **Authentication → Users**, copy your **User UID**.
2. In **Firestore → Rules**, change `request.auth.uid == uid` to `request.auth.uid == uid && uid == "YOUR-UID"` and **Publish**.

## Costs

- **Firebase Spark plan:** free, with daily limits far above what one person's planner uses. You'd have to upgrade on purpose to ever be charged.
- **GitHub Pages:** free for public repositories. Only the app's code is public, not your data.
