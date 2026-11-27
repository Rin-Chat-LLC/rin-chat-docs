# Import from SillyTavern: Cards, Lorebooks, Presets, Personas

> Read this on [rin.chat](https://rin.chat/guide/import-from-sillytavern/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

## Import from SillyTavern

Rin Chat reads SillyTavern's own files, so you can bring your characters, chats, lorebooks, personas and presets across in one go, or one file at a time. Nothing in your SillyTavern folder is changed, so SillyTavern keeps working as before.

#### Everything at once: Migrate from SillyTavern

Settings → Data & Backup → **Migrate from SillyTavern** (also offered once in first-run setup) asks for your SillyTavern user folder, the `data/default-user` folder inside SillyTavern, lists what is in it, and lets you choose what comes across:

- **Characters**, and with **Bring each ticked character's chats** on, every chat each of them has.
- **Group chats**, filed under the group's first member with the rest of the group as its members. A group none of whose characters came across is skipped and named in the summary.
- **Lorebooks**. A book that was global in SillyTavern is global here, and a character linked to a book stays linked to it.
- **Personas**, as persona cards with their avatar pictures. Your default SillyTavern persona becomes the active one if you hadn't picked one here yet.
- **Presets and templates**: Chat Completion, Text Completion and KoboldAI presets, context templates, system prompts and reasoning templates. Only the ones you were using start ticked, and **Review details** chooses which of a preset's prompt blocks come with it.

Open a section to tick individual items. Nothing already in Rin Chat is overwritten: a card you already have is skipped (its chats still come across), and so is a chat with the same title as one the card already has, so running it again adds only what's new. **Stop** keeps everything added so far. **API keys are not copied**; the summary shows the file they are in, so you can paste them into Settings → LLM Providers yourself.

#### One file at a time

- **Character cards**: SillyTavern V1, V2 and V3 cards, as PNG or JSON, from the Library's **Import** button or dragged onto the Characters tab (see Characters & the library). The card's embedded lorebook, alternate greetings, depth prompt and regex scripts come with it, and the fields Rin Chat doesn't use are kept and written back when you export it to SillyTavern again. A `.charx` file imports too, and an older RisuAI PNG card brings the expressions and pictures stored inside it.
- **Older characters**: TavernAI cards, Pygmalion and TavernAI JSON characters, and text-generation-webui (oobabooga) YAML characters. These usually sit beside a picture of the same name (`Mira.yaml` and `Mira.png`): import the two together, or their folder with **Import folder**, and the picture becomes the card's portrait.
- **Characters from other apps**: an Agnai character JSON, its persona written out in the format it was made in (W++, SBF, Boostyle or plain text), and a Backyard AI archive (`.byaf`). From a Backyard AI file the first scenario makes the card, the other scenarios' first messages become alternate greetings, its lore becomes the character's own lorebook, and its other images and scenario backgrounds go into the card's **Images**. Its chats don't come across.
- **Lorebooks**: a SillyTavern World Info JSON, or a lorebook from NovelAI (`.lorebook`), Agnai or RisuAI, imports on the Library's **Lorebooks** tab with its **Import** button, several files at once, whether its entries are SillyTavern's numbered map or a plain list (see Lorebooks).
- **Presets**: **Import from SillyTavern**, under the list in Settings → Generation Configs or Settings → System Prompts, reads a preset, context template, system prompt or reasoning template. Prompt Manager blocks are folded into the context template and post-history instructions, with a tick box for each. Before anything is added it shows what came across, what didn't, and any SillyTavern macros Rin Chat doesn't have (those print nothing). The result is new configs; nothing you use changes until you pick them.
- **Personas**: a persona here is a card. Import or make one like any character, then set its **Card type** in the editor header to **User Persona** (see Personas).
- **Chats**: a SillyTavern chat file (`.jsonl`) goes into a character from its page, **Tools → Import a chat**. The messages and their alternate replies come across (see Export, import & backup).
- **Sprite packs**: a SillyTavern expression pack, a zip or images named `joy.png`, `anger.png` and so on, goes in with **Import sprite pack** in the card editor's **Images** section. A sprite pack zip dropped on the library asks which card it's for; open **Show what's inside** there to see every expression first, then click a card to see which of its expressions the pack replaces and which are new before you add it (see the card format).

#### Seeing what's inside

Importing cards, bundles, sprite packs, lorebooks, generators, settings or a backup gives you a **Show what's inside** link, folded away so a quick import is still one click. After cards come in, it sits in the import's message: every file with what it became (imported, already in your library, a match for one of yours, a portrait for a character, or why it failed), and each card with its picture, creator, a rough token count and what came with it, such as lorebook entries, alternate greetings, expressions, pictures, a chat style or regex scripts. A batch of ten files or more shows the same list before anything is saved, with Import and Cancel; a card that matches one of yours is still yours to Replace, keep as New or Skip once you choose Import. Your phone does the same, and lists a bundle it can't import with the reason. A long list shows a page of rows at a time and adds more as you scroll. A bundle also lists its lorebooks, generators and document styles by name. The Lorebooks and Generators tabs list each book or generator they brought in, a lorebook with its first entries and the app it came from, and adding a lorebook to a character that already has one lists the incoming entries before you choose Add or Replace. Importing settings or a full backup shows what would change before you confirm: which settings differ from yours, and which characters and lorebooks replace yours or are new. Nothing from the file is run or styled in this list; it is shown as plain text and pictures.

#### Duplicates

A card you already have is caught on import: an identical one is skipped (**Import anyway** still brings it in), and one that matches by name and creator but differs is shown beside yours to **Replace**, keep as **New** or **Skip**. Lorebooks on the Lorebooks tab work the same way. An identical book is skipped, and one with the same name offers Replace, New or Skip.

#### Going back to SillyTavern

A card's **Export ▾** in the editor, and its right-click menu in the library, have **SillyTavern Format** as PNG or JSON: a V3 card that SillyTavern and most other apps read. Rin Chat's split-out fields are merged back into SillyTavern's flat ones under labels (what that loses is in the card format). A lorebook's menu on the Lorebooks tab has **Export as SillyTavern JSON**, **Export chat** saves a chat as JSONL, and a one-card bundle saved as `.charx` carries the card with its art.

> **Tip:** **What stays behind.** API keys, on purpose, and everything in the SillyTavern folder other than the items above: its own settings, themes, backgrounds, Quick Reply sets and extensions.
