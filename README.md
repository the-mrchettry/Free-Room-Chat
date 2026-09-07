# Free Room Chat

A room-based chat app. Anyone who enters the same room code joins the same conversation — messages are saved to a free Firebase backend, so history survives refreshes and works the same from any device. Built with plain HTML/CSS/JS and [Firebase Firestore](https://firebase.google.com/docs/firestore).

## Features

- Join or create a room with a username + room code
- Persistent chat history — survives page refresh and works across devices with the same room code
- Live chat with message reactions and replies
- Typing indicator
- Members list with online/left tracking
- Clear chat for everyone in the room
- Leave-room confirmation
- Fully responsive — old phones, iPads/tablets, laptops, desktops, and TVs (sidebar becomes a slide-over panel on small screens, content is centered with a max width on very large ones)

## Project structure

```
index.html
firebase-config.js
firestore.rules
favicon/          ← put favicon-96x96.png, favicon.svg, favicon.ico, apple-touch-icon.png, site.webmanifest here
images/           ← put Freeroomchat_logo.png here (used in the join screen + sidebar)
```

`index.html` already references `favicon/...` and `images/Freeroomchat_logo.png` as relative
paths — just make sure those two folders exist alongside `index.html` with the matching filenames.

## 1. Create a free Firebase project

1. Go to https://console.firebase.google.com → **Add project** (free, no credit card needed for Spark plan).
2. Once created, go to **Build → Firestore Database → Create database** → start in **production mode** (any region close to you) — click through to create it.
3. In the left sidebar, go to **Project settings** (⚙ icon) → **General** tab → scroll to **Your apps** → click the **Web** icon (`</>`) → register an app (no need for Firebase Hosting here).
4. Firebase will show you a `firebaseConfig` object with your keys.

## 2. Add your keys

Open `firebase-config.js` and replace the placeholder values with the ones from step 1:

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```

## 3. Add the security rules

Go to **Firestore Database → Rules** in the Firebase console, and paste in the
contents of `firestore.rules` from this project, then click **Publish**.

(These rules let anyone who knows a room code read/write that room — same trust
model as the original app, where the room code itself is the "access key". Don't
use guessable room codes for private chats.)

## 4. Host it (GitHub Pages)

1. Push `index.html`, `firebase-config.js` (with your real keys filled in), and
   `firestore.rules` (optional, just for reference — it's not used at runtime)
   to a GitHub repo.
2. Repo → **Settings → Pages** → set source to your default branch, root folder.
3. Your chat will be live at `https://<username>.github.io/<repo>/`.

That's it — no server, no build step, everything runs client-side against Firestore.

## Notes on the free tier

Firestore's free Spark plan gives **50,000 reads / 20,000 writes / 20,000 deletes
per day** — plenty for casual/small-group use. If a room gets heavy, ongoing costs
scale but the free quota resets daily. Things kept deliberately light to stay
inside it:

- Only the last 200 messages per room load into the live view.
- Typing status writes are throttled to ~1 per 1.5s per person, and auto-expire.
- "Online" heartbeat only writes once every 25s per active member.