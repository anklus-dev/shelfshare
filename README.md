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

## Location details

- IP lookup tries ipwho.is, then get.geojs.io, then ipapi.co (free, no keys). Reverse lookup for GPS uses api.bigdatacloud.net. If every lookup fails (blocked network, VPN, ad blocker), the sign-up screen shows a city picker (Georgia cities) and defaults to Atlanta.
- IP location is approximate: it can be tens of miles off, and venue or school Wi-Fi often reports the network's location. Everyone on the same Wi-Fi will appear "nearby". For the demo, that's a feature.
- Geolocation only works on https (GitHub Pages is https).

## Resetting and limits

- **Reset between demos:** delete the `ss` node in Firebase (Data tab).
- The database rules are fully open and there are no passwords. Fine for a hackathon; don't enter real personal information.
- The app polls the database every ~2.5 seconds (only while the tab is visible). Chat and requests arrive with that small delay.

## Demo script (two phones)

1. Phone 1: create `Alex`, building "Maple Court Apartments". Phone 2: create `Sam`, same building.
2. Sam's Discover shows Alex's books. Open Community → the building club and chat.
3. Sam requests a book; Alex approves in Activity. Sam shares a thought; it shows up in Alex's Thoughtstream. Alex replies.
4. Each marks a book as read, then check Community → Leaderboard.
