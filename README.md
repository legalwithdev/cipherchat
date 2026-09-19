# CipherChat P2P — Installable Web App (PWA)

Direct device-to-device encrypted chat — no server, no accounts, no storage, no cost.

## Security model (v2 — forward secrecy)

Every message is sealed with its own fresh key using a Signal-style **double ratchet**:

- Identity keys (ECDH P-256) are generated on each device and exchanged inside the two connection codes.
- Each conversation turn rotates the DH key (ECDH P-256 + HKDF-SHA256), and every single message advances a symmetric chain to derive a unique AES-256-GCM message key.
- This runs *on top of* WebRTC's DTLS transport — two independent layers.
- The verification code (compare over a call) mixes both parties' identity keys with the DTLS certificates.
- Replayed, duplicated, out-of-order and tampered messages are rejected.

Honest limitations: this is a hobby project, **not independently audited**; both people must be online at the same time; some strict office/campus networks block direct P2P connections. For everyday messaging, audited apps like Signal remain the right tool.

## What's in this package

| File | Purpose |
|---|---|
| `index.html` | The whole app (PWA-ready, with install + WhatsApp invite feature) |
| `manifest.json` | Tells browsers this is an installable app (name, icons, colors) |
| `sw.js` | Service worker — caches the app files so it opens offline; users always get the latest version when online |
| `icons/` | App icons (regular + maskable, 192px + 512px) |
| `README.md` | This guide |

Only the APP FILES are cached on the user's device. Messages and keys are never written to disk anywhere.

## Test it locally first (optional)

The service worker and install prompt need HTTPS or localhost — opening the file by double-click will NOT trigger them (the chat itself still works from file://).

```bash
cd this-folder
python3 -m http.server 8000
# open http://localhost:8000 in Chrome
```

## Deploy on GitHub Pages — 0 cost, ~5 minutes

1. Create a GitHub account (free) at github.com if you don't have one.
2. Click **+** (top right) → **New repository**.
   - Name: e.g. `cipherchat` (lowercase, hyphens, no spaces)
   - Visibility: **Public** (free GitHub Pages needs public)
   - Do NOT add README/gitignore
3. On the empty repo page click **"uploading an existing file"**.
4. Drag `index.html`, `manifest.json`, `sw.js` and the `icons` folder into the browser. Commit.
5. **Settings → Pages** → Source: **Deploy from a branch** → Branch: **main**, folder **/(root)** → **Save**.
6. Wait 1–2 minutes → your site is live at `https://YOUR-USERNAME.github.io/cipherchat/`

Share that URL — click, connect, chat.

## Updating the app later

Open the repo → edit icon on `index.html` → paste new version → commit.
Users get the new version on the next visit (network-first service worker).

## The "Bulao unhe WhatsApp par" (invite) button

On the start screen and the waiting screen there is an invite button:
- On phones it opens the share sheet — pick WhatsApp / Telegram / SMS.
- On desktop it opens WhatsApp Web with a pre-filled message containing your app link.
- Nothing is ever sent automatically — you choose the contact and press send yourself.
- This does NOT change the architecture: the chat is still 100% serverless. The invite is just a normal message you send yourself.

## How users install it

- **Android (Chrome):** open the URL → banner or menu (⋮) → **Install app / Add to Home screen**.
- **iPhone/iPad (Safari):** open the URL → **Share → Add to Home Screen**.
- **Desktop (Chrome/Edge):** install icon in the address bar, or the "Install as app" button in the app.

## Verify your deployment (2-minute checklist)

- [ ] URL opens with a padlock (HTTPS)
- [ ] CipherChat icon shows on the tab
- [ ] After one visit, airplane-mode + reopen still loads the app shell
- [ ] Android shows the install button after a couple of visits
- [ ] Two devices complete a connection and exchange messages

## Honest limitations

- Both people must be online at the same time — no offline message delivery.
- Offline mode only caches the APP SHELL; chatting still needs internet.
- Some strict corporate/campus networks block P2P — home Wi-Fi and mobile data work reliably.
- Not security-audited. For everyday messaging, audited apps (Signal) remain the right tool.
