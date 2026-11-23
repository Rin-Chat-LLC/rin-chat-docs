# Import from SillyTavern: Cards, Lorebooks, Presets, Personas

> Read this on [rin.chat](https://rin.chat/guide/import-from-sillytavern/), with screenshots. Generated from the guide in Rin Chat 1.0.4; changes made here are overwritten by the next release.

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

- **Character cards**: SillyTavern V1, V2 and V3 cards, as PNG or JSON, from the Library's **Import** button or dragged onto the Characters tab (see Characters & the library). The card's embedded lorebook, alternate greetings, depth prompt and regex scripts come with it, and the fields Rin Chat doesn't use are kept and written back when you export it to SillyTavern again. A `.charx` file imports too.
- **Lorebooks**: a SillyTavern World Info JSON imports on the Library's **Lorebooks** tab with its **Import** button, several files at once, whether its entries are SillyTavern's numbered map or a plain list (see Lorebooks).
- **Presets**: **Import from SillyTavern**, under the list in Settings → Generation Configs or Settings → System Prompts, reads a preset, context template, system prompt or reasoning template. Prompt Manager blocks are folded into the context template and post-history instructions, with a tick box for each. Before anything is added it shows what came across, what didn't, and any SillyTavern macros Rin Chat doesn't have (those print nothing). The result is new configs; nothing you use changes until you pick them.
- **Personas**: a persona here is a card. Import or make one like any character, then set its **Card type** in the editor header to **User Persona** (see Personas).
- **Chats**: a SillyTavern chat file (`.jsonl`) goes into a character from its page, **Tools → Import a chat**. The messages and their alternate replies come across (see Export, import & backup).
- **Sprite packs**: a SillyTavern expression pack, a zip or images named `joy.png`, `anger.png` and so on, goes in with **Import sprite pack** in the card editor's **Images** section. A sprite pack zip dropped on the library asks which card it's for (see the card format).

#### Duplicates

A card you already have is caught on import: an identical one is skipped (**Import anyway** still brings it in), and one that matches by name and creator but differs is shown beside yours to **Replace**, keep as **New** or **Skip**. Lorebooks on the Lorebooks tab work the same way. An identical book is skipped, and one with the same name offers Replace, New or Skip.

#### Going back to SillyTavern

A card's **Export ▾** in the editor, and its right-click menu in the library, have **SillyTavern Format** as PNG or JSON: a V3 card that SillyTavern and most other apps read. Rin Chat's split-out fields are merged back into SillyTavern's flat ones under labels (what that loses is in the card format). A lorebook's menu on the Lorebooks tab has **Export as SillyTavern JSON**, **Export chat** saves a chat as JSONL, and a one-card bundle saved as `.charx` carries the card with its art.

> **Tip:** **What stays behind.** API keys, on purpose, and everything in the SillyTavern folder other than the items above: its own settings, themes, backgrounds, Quick Reply sets and extensions.
