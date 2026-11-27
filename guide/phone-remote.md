# Use Rin Chat from Your Phone's Browser

> Read this on [rin.chat](https://rin.chat/guide/phone-remote/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

## Phone remote access

Rin Chat can serve its own chat page to a phone or tablet. Turn it on in Settings → Remote Access, set a **PIN** there (the PIN is always chosen at the computer, never from the phone), then on the phone open the address with numbers in it (like `https://192.168.1.5:8787/`), or scan the QR code beside it with the phone's camera, and enter that PIN. Never use the `localhost` address on a phone: there, localhost means the phone itself, so it can't reach your computer. Your computer does all the work: API keys, characters and chats stay on it. Closing the app stops the server outright; signing out leaves it running but serving nothing except an "app locked" page. The app does not keep the computer awake, so if it sleeps the phone loses the connection until you wake it — check your power settings if you want to reach it from bed.

**Off by default**, port `8787` (change it while it's switched off).

**What a paired phone can do** — quite a lot, so only approve devices you trust:

- Browse the whole library — search, folders, tags, a *Jump back in* row of recent chats, and **Load more** for big libraries.
- Read a character's full fields, including its system prompt and post-history instructions (**Fields** tab) — and **edit cards, folders, tags and lorebooks** too: a connected phone is a real client, not a viewer. What stays desktop-only, permanently: your vault and account, provider API keys, the data folder, and administration of the remote server itself.
- Start a new chat or resume a saved one, send messages with live streaming, and **Stop** a reply mid-flight.
- Per message: swipe between alternates, **Retry** the last reply, **Edit**, and **Delete** (tap once to confirm).
- **Hold the Send button** for *Suggest replies*, *Impersonate* (it drafts your next message — type instructions first if you like), and *Continue the reply*. With a hardware keyboard, `Ctrl`/`⌘`+`Enter` sends.
- Group chats get a cast row for "have this one respond now"; pictures generated in the chat show up too. **Aa** in the header cycles the text size, and **pull down** anywhere to reload.

**"Why does my browser say the connection isn't private?"** That's expected and it isn't a fault. The connection *is* encrypted; the certificate is one the app made for itself, which browsers can't verify because a verifiable one needs a public web address and your home network hasn't got one. Choose **Advanced → Proceed**. If you want proof you're talking to your own computer, compare the certificate's fingerprint with the one shown in Settings → Remote Access.

**"My phone keeps asking for the PIN."** The phone isn't keeping its cookie. Leave private/incognito browsing, allow cookies for that address, and don't clear cookies on exit. Also check you're opening the **same address** every time: if your computer's IP changed, that's a different site as far as the browser is concerned, and the session doesn't follow. A VPN address (below) doesn't change.

**The PIN** — one PIN covers every device, and it is set (or changed) only at the computer in Settings → Remote Access. At least 4 characters (letters and symbols work, digits are easiest on a phone). Changing it signs every device out.

- Three wrong tries are free, then each further attempt has to wait longer — and after **ten** wrong attempts the app assumes someone is guessing and **locks the whole vault**: the server stops serving, the desktop drops back to its login screen, and nothing is readable again until the account password is typed at the computer — the one thing a guesser on the network can't do. If that happens to you (a cat on the keyboard counts), it isn't broken: unlock at the desktop and carry on.
- **Ask for it again after**: 5, 15 or 30 minutes, 2 hours or 12 hours of *not using it*. It's 30 minutes by default and it slides: reading and sending push it out, so it never interrupts a conversation. Locking your phone or switching apps doesn't end the session; the timer and the phone's **Sign out** (in its `☰` menu) do.
- **Forget the PIN** in the settings panel locks *every* device out until a new one is chosen.

**What the phone keeps** — chats and the character list are **never** stored on the device. Character art is cached for an hour so a long chat doesn't re-download the same face on every message; the *Let devices keep character art for 3 days* option extends that beyond the session.

**"It stopped working when I left home."** Expected: the numbered address only works while the phone is on the same Wi-Fi as the computer. Away from home you need a VPN address, below.

**Reaching it from outside the house** — install **Tailscale** (or another private VPN) on both devices, sign both into the same account, and a **VPN address** appears in the panel — open that one from anywhere and everything works exactly as it does at home. A note on exposure: the server listens on all of the computer's network interfaces, so on a typical home network behind a router it is reachable only from your own devices — but on a network you don't control (public IP, hotel or coworking Wi-Fi) treat it as reachable by anyone on that network, and either leave remote access off there or use the VPN address. Never port-forward it to the open internet.

**On iPhone:** Share → **Add to Home Screen** — it opens without the address bar and behaves like an app. There's no reload button then, so **pull down** to refresh.

**The phone can't connect at all?** That's almost always the firewall, not Rin Chat — the request never arrives. Windows usually marks home Wi-Fi as a *Public* network and blocks incoming connections whatever you've allowed: set it to *Private* (Windows Settings → Network & internet → your Wi-Fi). Some routers also have *AP isolation* or a guest network that stops devices seeing each other; a VPN sidesteps both. The Remote Access panel has the full walkthrough.

**While the desktop is locked or signed out**, the phone gets a page saying so and picks up by itself once you unlock. Use **Lock screen** (in the `☰` menu, or on the phone icon while remote access is on) when you want the computer covered but the phone still working. Revoke a device any time in Settings → Remote Access.
