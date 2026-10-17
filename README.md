# ShelfShare: One Shared Shelf

A neighborhood book-lending app demo (HackATL). One static page, no build step.

## 1. Put it on GitHub Pages (about 3 minutes)

1. Create a new GitHub repository (public), e.g. `shelfshare`.
2. Upload `index.html`, `.nojekyll` and this README to the repository root.
3. In the repo: **Settings → Pages → Build and deployment**. Source: **Deploy from a branch**. Branch: `main`, folder `/ (root)`. Save.
4. After a minute your site is live at `https://<your-username>.github.io/shelfshare/`.

At this point the app works in **local mode**: accounts are saved in the visitor's own browser only. It is a good fallback if the venue Wi-Fi is bad.

## 2. Make accounts shared (so judges see each other)

GitHub Pages can't store data, so the app talks to a free Firebase Realtime Database.

1. Go to https://console.firebase.google.com and create a project (turn Analytics off).
2. **Build → Realtime Database → Create database**. Pick any region and start in **test mode**.
3. Open the **Rules** tab, paste this, and publish (test mode expires after 30 days; this does not):
   ```json
   { "rules": { ".read": true, ".write": true } }
   ```
4. Copy the database URL shown at the top of the **Data** tab (it looks like `https://your-project-default-rtdb.firebaseio.com`).
5. Open `index.html`, find the line `const CFG={FIREBASE_URL:''};` and paste the URL between the quotes. Commit.
6. Reload the site. The sign-up screen now says "Live". Open it on two devices, create two accounts, and request each other's books.

**Reset between demos:** in the Firebase console, delete the `ss` node on the Data tab.

## How it behaves

- Signing up asks for a name and an optional location. No password: typing the same name logs you back in.
- Each new account gets three classics on its shelf (The Odyssey, Pride and Prejudice and 1984 for the first, then the next unused ones for the next accounts), so books differ between neighbors.
- Other accounts' available books appear on Discover within a few seconds. A borrow request pops up in the owner's Activity tab with Approve/Decline; the borrower sees the result, the loan moves to "Borrowed by you" and "Lent out", and the owner can mark it returned.
- Fictional neighbors (Maya, Rosa, Dev...) are still there so the app feels alive with only one account.
- Shared across devices: accounts, shelves, borrow requests. Local to each visitor and not shared: the feed, thoughts and replies.

## Caveats

- The database rules are fully open and there are no passwords. Fine for a hackathon demo; do not put real personal information in it.
- Syncing is by polling every ~2.5 seconds, so changes show up with a short delay.
