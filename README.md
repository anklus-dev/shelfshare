# ShelfShare: One Shared Shelf

A neighborhood book-lending app demo (HackATL). One static page, no build step. Your Firebase URL is already set in `index.html`.

## Updating the live site

1. Replace everything in your repo folder with the files from this zip (`index.html`, `README.md`, `.nojekyll`).
2. `git add -A && git commit -m "Add location, clubs, leaderboard" && git push`
3. Wait about a minute, then hard refresh the site (Ctrl+Shift+R).
4. Recommended: in the Firebase console, delete the `ss` node on the **Data** tab so old test accounts (which have no location) don't clutter the leaderboard.

## What's in this version

- **Real location.** At sign-up the app finds your approximate area from your connection (IP). A "Use precise location" button upgrades it with the browser's GPS (asks permission). Only a rounded position (about 1 km) and a city/state name are stored.
- **Real neighbors only.** The demo neighbors and their books are gone. Discover lists books from actual accounts, sorted by distance, filtered by the range chip (1/10/30/50 mi). Each new account still gets three classics.
- **Swap matches.** If you have a book on your want list that its owner also has, and the owner wants one of your books (want lists are on Me → Want to read), a swap banner appears. Approving a swap creates both loans.
- **Shared feed.** Thoughts, likes and replies are now stored in the database. Thoughts on books you lent show "On your copy". Replies and new thoughts show up in Activity → Thoughtstream.
- **Books read.** Mark books as read on your shelf (or "Finished reading" on a borrowed book).
- **Book clubs.** Chats sorted by distance. Each area gets starter clubs automatically (city, neighborhood, classics, sci-fi, swap, statewide), each with a welcome message from ShelfShare. If you enter a building or neighborhood name at sign-up, you're added to a club for it (neighbors typing the same name share one). Anyone can start a club.
- **Leaderboard** (Community → Leaderboard): books read or books lent, for Neighborhood (3 mi), Town (10 mi), City (30 mi) or State (same state).

- **Activity** (requests, loans, Thoughtstream) is now the bell at the top right, which keeps the bottom dock balanced.

## Camera scanning
- **Add** opens the camera and reads the ISBN barcode on the back cover (hold it steady, fill the frame; the light button helps in dim rooms). Two matching reads are required and the ISBN checksum is verified, so a misread is very unlikely.
- Decoder: the browser's built-in BarcodeDetector where it works (Chrome/Android), otherwise a bundled ZXing WebAssembly decoder in `vendor/` (iPhone Safari, Firefox, desktop). No external CDN. Keep the `vendor/` folder next to `index.html`.
- Lookup: **Open Library** (book data, ISBN-10/13, and its search index) and **Google Books** are asked at the same time. If a book isn't found, the screen shows which catalog said what, and separates "not listed" from "a catalog didn't answer". Covers are saved with the book and show everywhere it appears.
- Adding by title: you can take a photo of the cover, which replaces the generated one (stored as a small ~10 KB image in the database).
- Stock classics (Frankenstein, etc.) get real covers automatically: the first device to open the app looks each one up on Open Library and shares it through the database (`covers/` node), so there are no repeat lookups.
- Backups if the camera can't be used (blocked permission, in-app browser): **take a photo** of the barcode, **type the ISBN** (10 or 13 digits), or **add by title**.
- The camera needs https, which GitHub Pages provides. The browser will ask for camera permission the first time.
- Open Library is community-maintained, so a brand-new or obscure edition can be missing. That is why Google Books is the backup and manual entry always works.

## Location details

- IP lookup tries ipwho.is, then get.geojs.io, then ipapi.co (free, no keys). Reverse lookup for GPS uses api.bigdatacloud.net. If every lookup fails (blocked network, VPN, ad blocker), the sign-up screen shows a city picker (Georgia cities) and defaults to Atlanta.
- IP location is approximate: it can be tens of miles off, and venue or school Wi-Fi often reports the network's location. Everyone on the same Wi-Fi will appear "nearby". For the demo, that's a feature.
- Geolocation only works on https (GitHub Pages is https).

## Accounts and security
- Sign-up and log-in use **Firebase Authentication** (email + password). The Web API key is set in `index.html` (`CFG.API_KEY`); it is a public identifier, not a secret.
- Security rules live in `firebase-rules.json`. Paste them in the Firebase console: Realtime Database, Rules tab, replace everything, Publish. After that only signed-in users can read, you can only change your own profile and books, only the lender can approve a request, and nobody can post as someone else.
- Accounts are identified by Firebase user IDs. If you upgrade from the older name-only version, delete the `ss` node once (Data tab) so old test data goes away.

## Resetting and limits

- **Reset between demos:** delete the `ss` node in Firebase (Data tab).
- **Passwords can't be recovered by us.** Use the Forgot password link on the log-in screen (Firebase sends the email). Emails and passwords live in Firebase Authentication, never in the shared database.
- Firebase limits how many accounts one internet connection can create per hour. On shared venue wifi, create a couple of demo accounts ahead of time.
- The app polls the database every ~2.5 seconds (only while the tab is visible). Chat and requests arrive with that small delay.

## Demo script (two phones)

1. Phone 1: create `Alex`, building "Maple Court Apartments". Phone 2: create `Sam`, same building.
2. Sam's Discover shows Alex's books. Open Community → the building club and chat.
3. Sam requests a book; Alex approves in Activity. Sam shares a thought; it shows up in Alex's Thoughtstream. Alex replies.
4. Each marks a book as read, then check Community → Leaderboard.
