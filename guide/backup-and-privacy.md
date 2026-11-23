# Backup, Export, Account Security and Privacy

> Read this on [rin.chat](https://rin.chat/guide/backup-and-privacy/), with screenshots. Generated from the guide in Rin Chat 1.0.4; changes made here are overwritten by the next release.

## Export, import & backup

**Make backups.** The app is local-only: if this computer dies and you have no backup, your recovery kit gets you a working — but empty — vault. The key and the library are two separate things to save.

**Full backup** — Settings → Data & Backup → Export full backup writes a `.rcbak` file to your **Downloads** folder containing every card (with its art and edit history), every chat, lorebooks, folders, and your settings. Large libraries make large files. If anything can't be read — during export or restore — it's skipped and listed for you rather than failing the whole job.

> **Tip:** **Pictures generated inside chats are in the backup too** — every image a chat, Direct Chat or Forge session refers to is packed alongside the card art, and restoring puts them back where they were.

- **A `.rcbak` is not encrypted.** It's a ZIP, and anyone who opens it can read your cards and chats. Keep it somewhere you'd be comfortable keeping the chats themselves.
- **API keys are never included**: in this or any other export. Importing keeps the keys already on this device.
- **Import full backup** restores onto this device. Records with the same id are overwritten, folders with the same name are reused rather than duplicated, and original dates are kept. Anything unreadable is skipped and counted rather than failing the whole restore.

**Moving to another computer** — two steps, in this order:

- Export a full backup on the old machine.
- On the new machine, at first run choose **Restore from a recovery phrase** and use your recovery phrase or file (or **Linked AICC account**, if you linked one). That restores your *key*. Then use **Import full backup** to bring in the library.

> **Tip:** Setting up a fresh account and importing the backup works too — you don't have to keep the same key. Use the recovery route when you want the same vault, not just the same content.

**Smaller exports**

- **Settings only** (the **Settings backup** card: *Export settings* and *Import settings*, which overwrites the matching settings on this device): every setting as a shareable JSON: providers (keys stripped), configs, prompts, regex, macros, quick replies, themes, shortcuts, voice setup, Card Forge guides and sessions, Direct Chats. Safe to hand to someone: no keys, no cards, no chats. The only things left out are machine-specific: the remote-access setup, the AICC login token, the debug log, trained-voice file paths and telemetry ids. (Personas and adventure characters are cards, so they travel in a full backup, not here.)
- **One card**: native AICC JSON (everything intact) or a SillyTavern-compatible export.
- **One chat**: Markdown, plain text, or JSONL (from **Export chat** in the chat's sidebar).

**Importing a chat, or moving one to another character.** A JSONL chat, exported from Rin Chat or from SillyTavern, goes into any character from that character's page: **Tools → Import a chat**. It arrives as a new chat on that character and opens. That is also how to carry a conversation over to a duplicated or reworked character: export it as JSONL from the old one, then import it into the new one. The messages and their alternate replies come across; the chat's summary, pins and variables start fresh.

**Debug log** (in Settings → Diagnostics), **on by default**. It records the raw requests and replies exchanged with your AI provider, plus errors, so you can send a diagnostics file when something misbehaves. API keys are redacted, but **your prompts, card text and messages are in there** — so read an exported log before you share it, or switch the log off and clear it if you'd rather it never existed. It's kept encrypted in your vault; the exported file is plain JSON in Downloads.

## Account & security

Your library is encrypted on this device. Card and chat files are **AES-256-GCM** blobs and the index database is **SQLCipher**, both keyed by a random 32-byte key that is never written to disk in the clear. That key is protected by your password (Argon2id), and optionally by the OS keychain for silent unlock.

**Your local account**

- **Username + password**: set at first run; the password derives the key that opens your vault. Change either in Settings → Account & Security. Changing the username *renames your data folder* and reloads the app.
- **Remember me**: off by default. On, the app unlocks silently on this device using the OS keychain (Windows Credential Manager / macOS Keychain / libsecret). Off, you type your password each launch.
- **Windows Hello / Touch ID**: optional, in Settings → Account & Security. It's a consent prompt in front of the *same* stored key, not a second password, so it keeps "Remember me" switched on. Your password always still works — cancel the prompt and you get the password screen.
- **Sign out** (the log-out icon, top-right) closes the vault so the password is needed to return. With Hello / Touch ID enabled the stored key is kept, since that's what the prompt releases — so the next unlock is the biometric prompt rather than nothing at all.

**More than one account on one computer**

- Each account is a **completely separate encrypted vault** — its own key, library, settings and (optional) linked website account. One can't read another.
- Add one with **+ Add another account** on the unlock screen. With two or more, the unlock screen shows an **Account** dropdown and *always* asks for a password — nothing opens silently.
- Each account's data lives in its own folder: `…/rin-chat/<username>/`. Only the account names and folder locations are stored unencrypted, so the picker can be drawn before anything is unlocked.

**Recovery kit — read this before you need it**

- It's **24 words** and a **recovery file**, and they are the **same key in two forms** — the file is plain text containing those same words. Keep **either one** somewhere safe; you don't need both.
- Anyone who has either one can open your vault. Treat it like a house key: not in the folder it protects, and not in a shared drive you wouldn't hand someone.
- View it any time in Settings → Data & Backup → Recovery kit. It is always the *same* kit — showing it again doesn't invalidate an older copy, and there is no way to rotate it.
- **It restores the key, not your library.** On a new computer the kit gets you back into an empty vault; your cards and chats come from a full backup (see Export, import & backup).
- *Older 12-word kits still work, but they need their phrase and their file together.*

**"I forgot my password"** — the unlock screen offers three ways back, in this order:

- **Use recovery phrase**: paste the 24 words *or* load the recovery file, then choose a new username and password for this device.
- **Reset with your AICC account**: only if you linked one *while unlocked*. Sign in through your browser and set a new local password.
- **Neither?** Then the data cannot be recovered — not by us, not by support, not by anyone. There is no master key and no reset link. This is the cost of a vault nobody else can open.

**Linking your aicharactercards.com account** (optional, in onboarding or Settings → Account & Security) — done **through your browser**, so the app never handles your website password and Google/Discord sign-in works. Be clear about what it does before you turn it on:

- It gives you a second way to reset a forgotten local password, and a way to recover your vault key on another computer by signing in.
- To do that it **uploads a copy of your vault key to aicharactercards.com**, sealed with a secret the site issues for your account. Because the site holds both the sealed key and the secret that opens it, **a linked vault is recoverable by the site**. Your cards and chats are never uploaded — only the key.
- If you don't want that, don't link — the recovery kit works entirely without it. **Unlink** removes the reset path on this device; ask support if you want the uploaded copy deleted from the account.

**Card updates, ratings and reviews** (linked accounts only, same page) is for cards you got from aicharactercards.com. Only card IDs and a fingerprint of each card's name, description, personality and scenario are sent, never your chats or edits.

- **Check my cards automatically**: once a day, a while after you unlock. It first links cards downloaded before card IDs existed (once per card), then looks for newer versions. Cards with a newer version show up under **Card updates** in the library's sidebar. **Review update** shows each changed field to pick from; tick several (or **Select all**) and **Update checked** takes every change and the new picture without the review. Either way, the card's Tools → History can put the old version back.
- **Check for updates now** runs the same check by hand.
- **Match my cards again** looks again for cards that found no match before, which helps once a card has since been published on the site.

**Locking the screen** — the `☰` menu has **Lock screen** (pin it to the bar under Header buttons if you use it often). It covers the desktop behind your password while the vault stays open — useful on a shared computer even with no phone paired, and paired phones keep chatting. Everything under the cover is made unreachable — no clicking, no tab-focus, no text selection, no printing or saving the page — and pairing requests are held back until you unlock. It does not protect against someone reading the running program's memory; the key is in memory, because that's what the phone is being served from. To close the vault completely, sign out instead.

**Where things live, and what isn't encrypted**

- Pick the data folder at setup, or move it later in Settings → Data & Backup. Card artwork can also be encrypted at rest — on by default, same panel.
- **API keys** are stored encrypted, redacted in the debug log and Session Info, and stripped from every export.
- Not encrypted, on purpose: the list of account names and folder locations (needed before unlock), the remote-access certificate, and **anything you export** — backups, settings files, card and chat exports and debug logs are ordinary files in your **Downloads** folder.

**Starting over** — Settings → Data & Backup → Danger zone → Reset this device's vault asks you to type `RESET`, then **permanently erases this device's library** — cards, chats, lorebooks, images — along with the vault key, and returns you to first-run setup. There is no "keep my data" option. Export a full backup first.

## Terms of use & privacy

The short, honest version — this page is the terms of use referenced elsewhere in the app.

The full Terms of Service and Privacy Policy live online at rin.chat. This page summarizes them in plain language; where they differ, the online versions govern.

**The deal.** Rin Chat is provided free of charge during its beta, for personal use, as-is and without warranty. It is beta software: defects are possible, so keep backups of anything you care about. You may not redistribute or sell the app itself; everything you *create* with it — characters, chats, lorebooks — is yours, and you can export all of it at any time.

**Your responsibilities.** You bring your own AI provider: the account, its costs, and compliance with that provider's own terms are between you and them. You are responsible for the content you create and import, and for complying with the laws that apply to you.

**Privacy.** Rin Chat is local-first: your characters, chats, and API keys are stored encrypted on your device and are never uploaded to us. The complete list of what does leave your machine is in Getting started, and every field of the anonymous usage ping — the one mandatory transmission, which identifies an install, never a person — is documented in Usage statistics, alongside the optional categories and their switches. We have no account of yours to hold data against unless you link an AICC account, and no ability to read what the app encrypts.

**Changes.** These terms may be updated between versions; material changes will be called out in the release notes. Continuing to use the app after an update is acceptance of the then-current terms. Third-party components are credited in the THIRD-PARTY file that ships with the app.
