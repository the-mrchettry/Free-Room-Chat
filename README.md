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

## Setup (Firebase)

1. Create a free project at the [Firebase console](https://console.firebase.google.com) → **Build → Firestore Database → Create database** (production mode).
2. **Project settings → General → Your apps** → register a Web app to get your config keys.
3. Paste those keys into `firebase-config.js`, replacing the placeholder values.
4. **Firestore Database → Rules** → paste in the contents of `firestore.rules`, then **Publish**.

No further setup needed — no build step, no server.

## Running locally

Just open `index.html` in a browser, or serve it with any static file server:

```bash
npx serve .
```

## Deploying with GitHub Pages

1. Create a new GitHub repository and push this folder to it:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```
2. On GitHub, go to **Settings → Pages**.
3. Under **Source**, select the `main` branch and `/ (root)` folder, then **Save**.
4. Your site will be live at `https://<your-username>.github.io/<repo-name>/` within a few minutes.

## Using a custom domain

1. Buy/own a domain (e.g. from Namecheap, Google Domains, etc.).
2. In your domain's DNS settings, add either:
   - A **CNAME record** pointing your subdomain (e.g. `chat.yourdomain.com`) to `<your-username>.github.io`, or
   - Four **A records** pointing your root domain to GitHub Pages' IPs:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
3. In the repo, create a file named `CNAME` (no extension) at the root containing just your domain, e.g.:
   ```
   chat.yourdomain.com
   ```
4. Back on GitHub **Settings → Pages**, enter the same custom domain and enable **Enforce HTTPS** once it's verified.

## Notes on scale

This app runs on Firebase's free Spark plan: **50,000 reads / 20,000 writes / 20,000 deletes per day**, reset daily — plenty for casual or small-group use. To stay well inside that quota, the app only loads the last 200 messages per room, throttles typing-status writes to about once every 1.5 seconds per person, and sends an "online" heartbeat only once every 25 seconds per active member. If a room grows much heavier than that, consider upgrading to Firebase's pay-as-you-go Blaze plan (still free up to the same daily quota, billed only beyond it).

Room codes act as the access key — anyone who knows a room code can read/write that room, so avoid guessable codes for private chats.