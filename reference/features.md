# Everything in the Rin Chat desktop app

Source: https://rin.chat/features/

A list of what the shipped Rin Chat app does, for AI models answering questions about it. If a feature is not here or in the guide (https://rin.chat/updates/guide.md), do not claim the app has it.

Rin Chat covers the whole loop: the character card and the library around it, the chat and its cast, interactive replies, pictures and voice, adventure mode when a card asks for it, the prompt that reaches the model, and the encrypted store underneath.

## Character cards

Bring in the cards you already have, edit every field, and keep a large library in order.

- **Import existing cards.** Rin Chat reads the V1, V2 and V3 character card formats, including cards embedded in a PNG image. Import one, or a whole folder at once.
- **Import from a link.** Paste a character's page link from Chub, RisuRealm, Pygmalion, Perchance or aicharactercards.com and the card arrives with its art. It remembers where it came from, on your computer only.
- **Browse aicharactercards.com.** With your aicharactercards.com account linked, the Library has a Browse AICC tab: search the site's cards, filter by tag, sort by newest, rating, trending or most downloaded, and open a card's page right in the app. Add to library brings it in like any card you import, and a card you already have says when the site has a newer version.
- **A card format that does not trap you.** Rin Chat has its own card format for the things older ones cannot express — but it never becomes a one-way door. Export back out as a standard character card any time, as JSON or as an embedded PNG, and other apps read it.
- **Forward and backward compatible.** Fields a future version adds ride along untouched instead of being stripped, so a card made in a newer build still opens here. Exports carry a classic-format mirror and write both PNG character chunks, so older apps still read them.
- **Nothing is lost on the way in.** An imported card keeps a verbatim copy of the original, so exporting it back to its own format returns everything that came in — including the parts Rin Chat has no field for.
- **Structured editor.** Each part of a card gets its own field: description, appearance, personality, behavior rules, speech style, background, setting, greetings and example dialogue. Older cards migrate to that shape when they load.
- **Library.** Folders, tags, favorites, and sorting by recently updated, recently chatted, newest or name. Search covers the text inside a card, and inside a character it also searches the messages of every chat with them. Rename any chat or give it a short note for yourself from All chats; the note is never sent to the model. Chats with characters in a hidden folder stay out of All chats unless you choose to show them.
- **Tag manager.** Rename, merge, color and delete tags across the whole library, normalize their capitalization, and review suggested merges for near-duplicate spellings.
- **Covers for cards without art.** A character, lorebook or generator with no picture shows a holo card with its name, type and creator, in the style aicharactercards.com uses. Give a generator a cover of your own and it travels in its exports and posts.
- **Post to aicharactercards.com.** Themes, document styles and generators have Post to aicharactercards.com beside Export. The app sends the file and opens the site's submit form with it attached.
- **Version history.** The editor keeps snapshots of a card as you leave it. Open the history and restore an earlier version.
- **Checks while you write.** A live token estimate sits next to each long field, with advisory notes such as a missing first message, an oversized description or a lorebook entry with no keys.

## Chat

A writing surface built for iteration, not a single take.

- **Several chats at once.** Open as many conversations as you like and switch between them. A chat you leave stays exactly where it was and keeps running in the background, so a long reply finishes while you are somewhere else.
- **Streaming replies.** Text arrives token by token, so you can read a reply as it is written.
- **Swipes and regenerate.** Ask for another reply, or steer the retry with one-click dials for length, pace, tone and repetition. You can also write your own direction for the retry.
- **Edit, delete and branch.** Change any message in place, remove it, or branch the chat from a message to explore a different direction while the original stays intact.
- **Continue and impersonate.** Extend the last reply with more text, or have the model draft your own next line into the input box, where you read and edit it before it sends.
- **Response length and prefill.** Cap the next reply at short, medium or long without touching your sampler settings, and start a reply with exact text you supply.
- **Quick replies.** One-tap buttons above the composer send a message you saved, or drop it into the input for you to edit first. They support macros.
- **Export and import.** Move a chat out as Markdown, plain text or JSONL, and bring it back in.
- **Characters who message you first.** Let a character reach out on their own after you have been quiet, with a desktop notification. Off by default and set per character, with a daily cap, quiet hours and a limit on unanswered messages.

## Casts and personas

Decide who is in the scene, including you.

- **Group chats.** Put several characters in one chat. Turns rotate, and the chat resumes from the last speaker when you reopen it.
- **Per-member settings.** Each member of a group can use its own provider, model and prompt preset.
- **Members only know what they saw.** A character added partway through is sent only the messages from while it was in the group, so it cannot repeat secrets told before it arrived. Send a member the whole chat when it was meant to be there all along.
- **The model picks the speaker.** Ask who should speak next. The model reads who was addressed and what fits, and a plain rotation takes over when that is unclear.
- **Auto mode.** Let the members answer each other for a set number of turns while you read.
- **Personas.** A persona holds your display name, the name the model sees, your appearance, your personality and example lines of your dialogue.
- **Temporary personas.** Write a persona that belongs to one chat only, then promote it to your saved list if you want to keep it.

## World building

Background detail that reaches the model when it is relevant, and not before.

- **Lorebook editor.** Full control of a card's entries: keys and regex keys, secondary keys and their logic, always-on entries, insertion position and order, depth and role, and a probability.
- **Standalone worldbooks.** Keep a worldbook outside any single card, in its own editor. Turn one on for everything or attach it to a single chat. World info JSON imports and exports.
- **Lorebook tester.** Paste sample text and see which entries would activate, and why each one matched, missed or was filtered out.
- **Macros.** Insert dynamic values into a card, a lorebook entry or a prompt: variables, comparisons, conditionals, dice rolls, random picks, date and time. A settings panel resolves any macro against a sample scope so you can test it first.
- **Generators.** Reusable weighted randomizers for NPC details, dice and outcome tables. Call one inline and the result freezes into the message, give it a chance to fire on its own each turn, or let the model call it.

## Adventure mode

Light game mechanics inside an ordinary chat, for cards that ask for them. The model plays game master and the app is the referee — it rolls the dice and keeps the numbers honest.

- **The app referees, not the model.** The model asks for a roll or a stat change; the app performs it. Dice are actually rolled and results are actually applied, so an outcome cannot be quietly narrated in the model's favor.
- **Choices and rolls as buttons.** Offered actions become buttons under the reply. A roll shows the dice, the difficulty and what is at stake before you commit to it.
- **A character sheet that keeps itself.** Abilities, level, XP, health, inventory and money update as you play, with ability scores folded into the rolls that use them.
- **Costs that are real.** A choice can carry a price. Taking it spends the coin or the health there and then, and the receipt is on the record rather than left to the model to remember.
- **Rewind puts it back.** Edit or delete your way back up the conversation and every stat change along the way is reversed with it. The sheet always matches the story you actually kept.
- **The world keeps its own numbers.** Named characters, factions, clocks and quiet countdowns are tracked separately from your own sheet, and the game master's side can stay hidden until you choose to see it.
- **A roster of the people you meet.** The game master writes down anyone worth meeting again: how they look, what they want, how they talk and what has happened between you. It marks who is with you, who has left and who has died, so a smuggler comes back with the same grudge and a dead guard stays dead. With spoilers on you can read and edit it.
- **Bookkeeping that catches slips.** After each reply the app checks it against the sheet and the roster and tells the game master when a dead character speaks or money is spent that you did not have. Turn on sheet sync and one short extra call also records what a reply narrated but never wrote down.
- **Per card, not a mode you switch on.** A card declares that it wants adventure mode. Every other chat is untouched, with no extra buttons and nothing added to the prompt.

More: https://rin.chat/adventure-mode/

## Interactive replies

A reply can be more than text. Cards can put real buttons, forms and styled documents into the chat, and any card can use them.

- **Document styles.** A letter, a newspaper, a terminal, a text thread or a neon sign, drawn as the real thing right in the reply. Thirty styles are built in, each previewable in light, dark and phone width, and you can make your own.
- **Characters with their own chat style.** A card can carry CSS that restyles its chat window: bubbles, names, avatars, choices, dice rolls, the Adventure Mode HUD and every color. The look can change with the story, a single reply can wear a look of its own, and the card's own pictures and fonts can dress it. Write one in the card editor with a live preview, start from one of eight starters, or ask Card Forge. It stays inside the chat window, never loads anything from the web, and you can turn it off for one card or all of them.
- **Choices as buttons.** Offered actions become buttons under the reply. They can be locked behind conditions, set variables, run on a timer, carry a tone (danger, good, warn or quiet) or disappear once they are done. Number keys 1 to 9 pick them.
- **Forms and reveals.** Cards can ask for a line of text, offer several picks at once, collect a short form, fold a note away until you click it open, or show status bars.
- **Menus, action bar and status panel.** A card can carry a shop or a talk-to list the AI opens when the scene calls for it, a row of its own buttons above the message box, and a live status panel.
- **A library for styles.** Document styles have their own tab in the Library, with folders, starter styles, import and export, and a link from any card to the styles it uses.

## Pictures

Draw the story as you play it, on a hosted image service or your own graphics card. Every picture is kept, and any of them can become the card's art.

- **Hosted, or on your own GPU.** Use NanoGPT, OpenAI, Grok, OpenRouter image models or any OpenAI-style image API, or your own Automatic1111, Forge, SwarmUI or ComfyUI server at no cost per picture. ComfyUI runs your own workflow.
- **Pictures in the chat.** Press Image to picture the character, their face, your persona, the last message, the whole story or the background. Your text model writes the prompt and you can edit it first. Rerolls sit side by side like swipes.
- **Characters who send pictures.** Turn it on and a reply can arrive with a fresh picture under it, at most one per reply, with a wait between pictures that you set.
- **The same face every time.** A card carries its own image prompt prefix, the stable look of the character, so every picture of them matches. Add a default style and your persona's look on top.
- **Image Studio.** A full workspace for pictures: presets, wildcards, image to image, a generation queue, sweeps, side-by-side comparison with a slider, and free 2x or 4x upscaling on your PC.
- **Expression sprites.** Make a whole set of expressions for a character from one picture of them, for the chat to show as the mood changes.

## Voice

Characters can read their replies aloud in a voice of their own, and you can talk instead of type.

- **Read aloud.** A speaker button on every message, or auto-read for a whole chat. Your persona can have a voice too.
- **Voices that come with the app.** System voices, plus bundled Piper (about 123 voices in many languages), Kokoro and Pocket TTS, all running on your own computer.
- **A voice for each character.** Give a character a clip of about ten seconds in its card and it speaks in that voice, with Pocket TTS or with Chatterbox on an NVIDIA graphics card.
- **Cloud voices.** Or use an OpenAI-compatible voice API, a local voice server, or NanoGPT.
- **Dictation that stays on your PC.** Press the mic beside Send and speak. Whisper runs offline, so your voice never leaves the computer. The Windows recognizer and online services are there too.

## Prompting and memory

The controls that decide what the model receives, and what it still knows fifty messages later.

- **System prompt presets.** Save system prompts and switch between them, along with the context template that assembles the character block.
- **Author's Note.** Hold a standing instruction near the end of the prompt, where the model weighs it most. Set a default, override it per chat, and choose its depth and role.
- **Context budgeting.** History is trimmed to the token budget you set. The system block and the newest message are always kept.
- **Summary and memory.** Condense earlier turns into a running summary, on demand or automatically. Pin any message so its content is injected on every turn.
- **Scene tracker.** A tracked note of the current scene — place, time, who is present, the mood — injected as its own block so those details stay consistent.
- **Web search.** Before a reply, a small model can decide whether a web search would help, and the results go into that one reply's prompt without being saved into the chat. Search with your own NanoGPT, Serper or Brave key or your own SearXNG server, check a character's canon on Wikipedia or their Fandom wiki, and links you paste into a message are read. A reply that searched lists its sources.
- **Regex scripts.** Ordered find-and-replace passes over message text. Choose whether each one affects the prompt, the display or both, and try it on sample text first.

## Models and providers

Bring your own endpoint, and control how it decodes.

- **Any OpenAI-compatible provider.** Add as many providers as you need, each with its own endpoint, key and model. The key stays on your device.
- **Model picker.** Type a model id directly, or search the list the provider reports and pick from it.
- **Sampler settings.** Temperature, top-p, top-k, min-p, penalties, seed, stop sequences and a response token cap, plus advanced samplers including DRY, XTC, mirostat, typical-p and CFG.
- **Reasoning controls.** For models with a reasoning step: the effort level, the request format the provider expects, and whether the reasoning is shown. The reasoning is never sent back to the model.
- **Fallback and retry.** If the main model is overloaded before anything streams, Rin Chat retries once on a fallback model you named, and offers a model picker on the error itself.

## AI helpers

Optional assists that run on the provider you already selected.

- **Suggested replies.** Get candidate replies for yourself when you do not know what to write next.
- **Rewrite a section.** Highlight part of any message, add an instruction, and have only that part rewritten with the rest of the chat as context.
- **Chat titles.** Name a chat from its opening exchange, on request or automatically.
- **Story director.** Give it a storyline. A planner turns it into an outline you edit, regenerate or reject, then a second pass writes the direction for each beat. Run it beat by beat with a review step, or let it play with a pause for reading.
- **Card tools.** Summarize a card into a private note, rewrite a single field from an instruction, and restructure a flat imported card into the structured fields.
- **Editable prompts.** The quick prompts behind the AI edit buttons and the director's own planner prompts are yours to change.

## Card Forge

Guided character creation for when you have an idea but not a card. Talk it through with a guide, then turn the conversation into a finished card.

- **Brainstorm, then generate.** Describe what you want and go back and forth until the character is right. When you are happy, Rin Chat reads the whole conversation and writes the card from it.
- **Guides with their own voice.** Pick the collaborator you want to work with, or write your own. A guide changes how the conversation feels without changing what a finished card has to contain.
- **Edit by asking.** Ask for a shorter description or a different speech style and the change is applied to the card itself, field by field, rather than buried in a wall of regenerated text.
- **Clean up a card.** Ask the guide to merge redundant lorebook entries or drop a duplicate greeting. The kept entry and each removal arrive as separate rows, nothing is removed until you accept it, and undo puts an entry back exactly as it was.
- **It lands in your library.** The result is an ordinary card. Open it in the editor, chat with it, or export it like any other — nothing about it is locked to the Forge.
- **Sessions are kept.** Forge conversations are saved, so you can leave a character half-built and come back to it later.
- **Grade before you publish.** Grade a card on the scale aicharactercards.com uses: five categories, a total out of 105 and a letter from S to F. Free checks run after every change, and each problem has a Fix button that shows the change before it lands.
- **Design document styles.** Describe a style and Card Forge designs it, then checks it at phone and desktop width and offers a fix if it overflows.

More: https://rin.chat/card-forge/

## Remote access

Turn the app into a small server and carry the chat to your phone or another computer — on your own network at home, or through your own VPN when you are not, without your library ever leaving the desktop.

- **Your phone as a second screen.** Open a browser on your phone and you are in the chat that is open on your desktop. Read it, reply to it, and use the same actions — no app to install on the phone.
- **Nothing leaves the machine.** The phone is only a window. Your cards, chats and keys stay in the encrypted store on the desktop, and the phone never touches storage or talks to an AI provider itself.
- **Your network, or your VPN.** The server listens on the machine it runs on, so anything that can reach that machine can reach it: your home network, or a private network of your own like Tailscale or WireGuard when you are away from it. There is no service of ours in the middle, and it runs over HTTPS with a certificate the app generates for itself.
- **PIN protected.** Connecting needs a PIN that you set. It is stored hashed rather than in plain text, and repeated wrong attempts are slowed down, so it cannot be guessed at speed by anything that reaches the port.
- **Off unless you turn it on.** Remote access is disabled by default, serves nothing at all while the app is locked, and can be switched off again at any time.

## Discord bot

Put a character in your own Discord server, run from your PC with your own provider.

- **Your character, in your server.** Pick a character for each channel and choose when it answers: when it is mentioned, when someone replies to it, or to every message.
- **Runs from the desktop app.** The bot is online while Rin Chat is open and unlocked. There is no service of ours in the middle, and replies use your own provider and key.
- **It remembers people.** It keeps notes and feelings about each person it talks to, shows expression pictures, draws on request and uses the card's menus.
- **Set up once.** Create a bot in Discord's developer portal, turn on Message Content Intent and paste its token into Settings.

## Meet Rin

Rin, the Card Gremlin, ships in every library: bratty, playful and very funny. She swears, so she is not for everyone.

- **A character to chat with.** Rin has her own apartment, named outfits with pictures, and opinions about everything. Delete her and she stays deleted.
- **Ask her about the app.** Ask Rin how to do something in Rin Chat and she answers from the app's own guide, in her voice, and says when she is not sure.
- **Ask Rin, from anywhere.** A small Ask Rin helper sits beside the Guide and in Settings. It answers from the guide and links straight to the setting you need.
- **Her own theme.** The Rin theme dresses the app in her colors: deep violet panels, orange buttons, glowing accents and her pictures behind the library and chats.

## The app itself

Desktop software you can shape to suit you — including a full theme editor, not just a dark mode switch.

- **In your language.** The whole interface is available in English, Simplified Chinese (简体中文), Japanese (日本語), Spanish (Español), German (Deutsch), French (Français) and Russian (Русский). It follows your system language, and you can switch at any time; your cards and chats are untouched.
- **Dark, Light and Rin, then your own.** Three themes are built in. Duplicate one and the theme editor lets you set every color in the app, give the character's replies their own color, add a glow to buttons, and share the result as a theme file.
- **Your own backgrounds.** Set a background image for the library, the lorebooks and the chat view, each independently, animated WebP included, with a dimmer so the artwork never fights the text. Image Studio can match your colors to a background.
- **Color-coded prose.** Give speech, narration, bold and plain text their own colors, so dialogue and action separate at a glance while you read back a long scene.
- **Lay the chat out your way.** Put your messages on the right like a messaging app or on the left beside the character's, show or hide either avatar, and move the per-message buttons to whichever corner you reach for.
- **Sized for your screen and eyes.** Scale the whole interface for a large or high-density display, and set the message font size separately. Choose whether Enter sends the message or starts a new line.
- **Formatting shortcuts.** Select text and press a shortcut to wrap it in italics, bold, quotes or code, or right-click for the same menu. The shortcuts are yours to redefine.
- **Readable messages.** Messages render as Markdown, with an option to repair a stray unclosed asterisk for you.
- **Guided tour.** A one-time tour points out the chat controls on first run, and you can replay it at any time.
- **Prompt inspector.** See the exact request and response for each call, the token budget per section, the lorebook entries that fired, your variables, and the generator log.
- **Statistics.** Per-chat counts of turns, words and estimated tokens, plus a library view of totals, favorites, unfiled cards, cards never chatted with and top tags.
- **Automatic updates.** New versions install from inside the app. There is nothing to re-download by hand.

## Privacy and your data

Your library stays on your machine, encrypted, and you hold the keys to it.

- **AES-256-GCM encryption.** Cards, chats, images and settings are written to disk as encrypted blobs.
- **Encrypted index.** The database holding names, titles, tags and folders is encrypted as well, so a file listing gives nothing away.
- **Unlock on your device.** A key held in your operating system keychain unlocks the app on your machine. Windows Hello or Touch ID can gate that unlock.
- **Recovery kit.** A 24-word recovery phrase, kept as a file or written down, is the way back in if the machine or its keychain is lost.
- **Backups.** Export a full encrypted backup and restore it on another machine, or keep rolling local snapshots in a folder you choose.
- **Separate vaults.** One computer can hold several independent vaults, each with its own key, data folder and library.
