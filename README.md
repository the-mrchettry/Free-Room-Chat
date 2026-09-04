# Free Room Chat

A room-based, peer-to-peer chat app. Anyone who enters the same room code joins the same live conversation — no backend, no database. Built with plain HTML/CSS/JS and [PeerJS](https://peerjs.com/) (WebRTC).

## Features

- Join or create a room with a username + room code
- Live chat with message reactions and replies
- Typing indicator
- Members list with active/left tracking (updates instantly on disconnect)
- Clear chat for everyone in the room
- Leave-room confirmation

## Running locally

No build step needed. Just open `index.html` in a browser, or serve it with any static file server:

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

This app uses PeerJS's free public signaling server, which supports up to ~50 concurrent WebRTC connections. If you expect much heavier traffic, self-host a [PeerServer](https://github.com/peers/peerjs-server) instance (e.g. on Render or Railway) and point the app to it instead of the default cloud service.
