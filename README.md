# CipherChat P2P — Installable Web App (PWA)

Direct device-to-device encrypted chat — no server, no accounts, no storage, no cost.

**Live app:** https://legalwithdev.github.io/cipherchat/

## What's in this repo

| File | Purpose |
|---|---|
| `index.html` | The whole app (PWA-ready, with install + WhatsApp invite feature) |
| `manifest.json` | Tells browsers this is an installable app (name, icons, colors) |
| `sw.js` | Service worker — caches the app files so it opens offline; users always get the latest version when online |
| `icons/` | App icons (regular + maskable, 192px + 512px) |

Only the APP FILES are cached on the user's device. Messages and keys are never written to disk anywhere.

## How it works

- Two people open the same page on their devices.
- Person A creates an invite code and sends it to Person B (WhatsApp/SMS/call — any channel).
- Person B pastes it, generates a reply code, sends it back.
- Person A pastes the reply — a direct WebRTC (DTLS-encrypted) channel opens between the two devices.
- Both devices show a verification code computed from the live encryption certificates — compare it over a voice call; a mismatch means a man-in-the-middle.

Messages travel directly between the two browsers. No server, database, or account exists anywhere in the pipeline.

## The invite button

On the start screen and the waiting screen, the "Bulao unhe WhatsApp par" button opens a pre-filled WhatsApp message (or the phone's share sheet) with the app link. Nothing is sent automatically — the sender picks the contact and presses send.

## Updating the app

Edit `index.html` in this repo (pencil icon) and commit. Users get the new version on their next visit (network-first service worker).

## Honest limitations

- Both people must be online at the same time — no offline message delivery.
- Offline mode only caches the APP SHELL; chatting still needs internet.
- Some strict corporate/campus networks block P2P — home Wi-Fi and mobile data work reliably.
- Not security-audited. For everyday messaging, audited apps (Signal) remain the right tool.
