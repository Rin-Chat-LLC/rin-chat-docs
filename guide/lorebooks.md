# Lorebooks: Import SillyTavern World Info and Write Entries

> Read this on [rin.chat](https://rin.chat/guide/lorebooks/), with screenshots. Generated from the guide in Rin Chat 1.0.6; changes made here are overwritten by the next release.

## Lorebooks

A **lorebook** injects extra context only when it's relevant. Each entry has keywords; when a keyword appears in recent messages, the entry's content is added to the prompt. Edit a card's own entries in its editor, or build reusable books on the Library's **Lorebooks** tab.

**Other names for the same thing.** Every app calls this something different, and they all mean these keyword-triggered entries: SillyTavern says **World Info** (and *Character Lore* for a card's own), NovelAI and Chub say **Lorebook** (Chub calls a card's own a *Characterbook*), Agnai says **Memory Book**, and the character-card format stores it as `character_book`. Rin Chat says **lorebook** throughout. Books exported from any of them import here as-is.

> **Tip:** **Keyword matching only reads the last 4 messages.** That's the answer to almost every "my lore stopped applying" — the keyword simply left the scan window. Change the window for every book in Settings → Lorebook Activation, or let one entry widen or narrow it for itself with its own **Scan depth**.

#### How an entry decides to fire

- **Keys**: comma-separated; *any one* of them fires the entry. By default a key matches as a *substring*, so `or` fires inside "order" and "north". Tick **Whole words only** to stop that (it's almost always what you want), or **Case-sensitive** for exact case.
- **Always on (no keys needed)**: the entry is in every prompt. Its keys are ignored entirely.
- **Keys are regex**: keys wrapped in slashes (like `/dragons?/i`) become regular expressions; a key without slashes still matches as plain text, so imported keys like `C++` can't break. Case-insensitive unless you tick Case-sensitive. Applies to secondary keys too, and the Audit warns when a key looks like a pattern but is missing its slashes.
- **Also require other keys**: adds a second key list plus a rule: `AND ANY` (at least one secondary present), `AND ALL` (all present), `NOT ANY` (none present), `NOT ALL` (not all present).
- **Scan depth**: how many recent messages *this* entry reads. Blank uses the normal window. **0 reads no chat at all**, which only makes sense with a matching source ticked.
- **Chance %**: the entry rolls each turn it matches. The roll is re-seeded per turn, so a 25% entry really fires about a quarter of the time.
- **Additional matching sources**: also scan the character description, personality, scenario, your persona description, the character's note, or the creator's notes.

#### Where it lands

- **Position**: *Before character* or *After character* put the text in the system block, around the character definition. *At depth* puts it in the chat history N messages from the end, as *System*, *User* or *Assistant*.
- **Order**: lower goes in first, among entries in the same place.

> **Tip:** *Before Author's Note* and *After Author's Note* currently behave exactly like *Before character* and *After character* — they're accepted for compatibility with imported books, but nothing is placed relative to the Author's Note. Use *At depth* if you need a specific spot in the history.

#### Timing

All counted in chat messages, `0` = off.

- **Delay**: the entry can't fire until the chat is that long. Good for lore that would spoil an opening.
- **Sticky**: once it fires, it stays active for that many messages even if the keyword is gone, and skips its chance roll while the window is open. *This is the fix for lore that keeps dropping out mid-scene.*
- **Cooldown**: after firing (or after a sticky window ends) the entry is blocked for that many messages. Ignored by an always-on entry.

#### Scope

- **Only for these characters / tags**: one box, matched against both the card's name and its tags. Tick **Exclude** to invert it.
- **Only on these actions**: limit the entry to `normal`, `continue`, `regenerate`, `impersonate`, `swipe` or `quiet` generations. Nothing ticked means every action.

#### Grouping — one of several

Give several entries the same **Group name** and only *one* of them is injected when more than one matches. That's how you make three weather entries or five rumours behave like a single slot. **Weight** sets the relative odds (200 is twice as likely as 100); **Prioritize** means only prioritized members can win; **Score by matches** picks whichever member's keys matched most times instead of drawing.

#### Recursion & routing

An entry that has fired can pull in another. Write `[recursion: KEY]` in its content, or emit a routing token `<!--TOKEN-->`; both resolve to the entry whose keyword *or title* matches exactly, and both are removed from the injected text. Chains go three levels deep by default (**Max recursion steps** below). Control it per entry with **Can't be pulled in by others**, **Can't pull in others**, and **Only via another entry** (skips the first scan entirely — the entry exists only as a recursion target).

**Recursive scan** — in Settings → Lorebook Activation, on by default and applying to every book. On, a fired entry's own text is also checked for other entries' keywords, and those fire too. Off, only the markers above chain, so a common word inside an entry can't quietly load half the book. **Max recursion steps** beside it caps how deep the chain goes (three by default, 0 for no limit).

**Routing tokens** work outside the lorebook too. `<!--TOKEN-->` (letters, digits and underscores only) can sit in a greeting, a message, a lorebook entry, or a chat variable's value; while it's there, the entry whose keyword or title is exactly `TOKEN` is loaded — no keyword in the chat needed. It's an HTML comment, so nobody sees it, and because it names an entry it's removed before the model reads the text. Any other HTML comment is left in for the model, so a card's own `<!-- notes -->` still reach it. Example: give an entry the title `TAVERN_MENU` and no keywords, then put `<!--TAVERN_MENU-->` at the end of the greeting that opens in the tavern.

#### Tools in the editor

- **Audit**: free and instant. Finds keys that fire on everything, entries that can never fire, 0% chances, duplicated content, oversized entries, keys shared across entries, and how many tokens your always-on entries cost every single turn. Click a finding to jump to the entry. **✨ Check for contradictions** is the opt-in AI pass on top (it sends the book).
- **Test**: paste sample chat text and see which entries would activate and why, using the same scanner the chat uses. An entry that another entry pulled in names that entry and the key it matched, so a chain of entries can be traced back to what started it.
- **✨ Directive**: describe what the book is for, so AI assist writes entries that fit. Stored on your machine, never exported with the book or card.
- **Sort** is display-only; **Apply as order** renumbers every entry in the book (in tens, so you can slot things in later) to match what you're looking at.
- **Show**, beside the filter box, narrows the list by how entries fire: enabled or disabled, always on, fired by keys, no keys, no title, secondary keys, in a group, a chance under 100%, delay, sticky or cooldown, recursion options, limited to characters or actions, or one position.
- **Several entries at once.** Ctrl-click (Cmd-click on a Mac) a row to select it, and Shift-click another to select every row between them. On a phone, or without a keyboard, tap **Select** and then tap rows. The bar that appears can **Enable**, **Disable** or **Delete** them (after asking), **Change a setting** on all of them (always on, position, depth, chance, group, scan depth, matching and recursion options; an Order you type is given to the first and counts up in tens down the list), **Move or copy** them into one of your shared lorebooks, and **Fill empty titles**, which names each untitled one after its first key or the opening words of its content. **Select all** takes every row the filter and Show are showing, so narrow the list first to change just those. Escape clears the selection.
- **Keyboard:** Tab to a row's arrow, then Up and Down move between rows and Enter opens or closes one.

#### Shared lorebooks

The Library's **Lorebooks** tab holds reusable books. Import and export SillyTavern World Info JSON, star a book to make it **global** (injected into every chat), or attach one to a single chat from that chat's lorebooks panel. A character's own embedded book is always active for that character. All active books are scanned in turn, and variables set by one carry into the next.

**Folders.** The folder sidebar beside your shared lorebooks works like the Characters tab's: **+ New folder** at its foot, then hover a folder to add a subfolder, rename it or delete it (what's inside goes back to the top level; nothing is deleted). Drag a book onto a folder, or use the folder button on its tile's corner, which is how you move one on a phone. A book made while a folder is open lands in it. Folders only sort the page: a book in a folder works exactly as it did. Character lorebooks live in their cards, so they show under **All lorebooks** and their own **Character lorebooks** row, never in a folder. A book's star (top corner of its tile) makes it **global**.

**Importing a book you already have.** An imported book is matched to yours by name, ignoring case and spaces at the ends. With the same entries it is skipped as already in your library. With different entries it is listed next to the book it matched: choose **Replace** (puts the imported entries into your book, so every character, chat and Global setting that uses it keeps using it; this can't be undone), **New** (adds it beside yours as "Name (2)") or **Skip** for each, then **Apply**. A `.aicc` bundle's lorebooks are matched the same way.

#### Settings that apply to every book

Settings → Lorebook Activation holds the activation settings: how many recent messages keywords are looked for in (**Scan depth**), whether to keep looking further back until something fires (**Min activations**), recursive scanning and its depth, whether keys are case-sensitive or must match whole words, how much of the context lore may fill (**Context share** and **Budget cap**, spent across all books at once), and whether the character's own book or the shared ones go in first. An entry that sets one of these for itself keeps its own answer — these are what the rest fall back to.

> **Tip:** Use an *always-on* entry for setting-wide rules, keyed entries for characters, places and items that only matter when mentioned, and **Sticky** for anything that must stay put once it comes up.

#### The chat's own lorebook

A third kind, besides a card's book and a shared one: **a lorebook that belongs to one chat and dies with it.** Open it from the book button beside the character sheet and the variables panel. It is not an Adventure Mode feature; any chat can keep one.

It exists for what turns up DURING play. A town of people nobody wrote down before the chat started, a rumor, a room. Those have nowhere good to live otherwise: a variable only reaches the character if something prints it every single turn, the card's own book is permanent and would carry one playthrough's cast into the next, and a shared book outlives the chat as well. A chat-owned book recalls by keyword like any other, so a cast of forty costs nothing in context until one of them is actually mentioned.

**The character can write into it as you play,** on a line of its own, which you never see in the reply:

- `[[lore: NAME | what to remember]]` writes the entry, and writing the same name again replaces it. That is how a correction is made.
- `[[lore+: NAME | one more thing]]` ADDS to an entry already written. Use this for a single new fact, which is most of what a scene teaches you: restating a whole entry to record one detail is how an older detail quietly goes missing.

The line still counts when the character writes it in bold, as a bullet, with a period after it, or at the very end of a sentence. In the middle of a sentence, or in code formatting at the end of one, it stays as text.

A card opts into that by using the line in its own text, the same way every other interactive element works, so no other card pays tokens for it. The character can create and revise entries but never empty or delete one, and there are limits on how long an entry is, how many can be written in a turn, and how many a chat can hold. A refused write is reported rather than dropped silently.

**A generator can write one too,** with `as.entry=` beside the usual `as=`: `[Create NPC as.entry="Guild Receptionist"]`. Quote a name with a space in it. A repeat numbers them apart, so `[Create Adventurer x5 as.entry=ADVENTURER]` is five people rather than one entry written over five times, and re-rolling replaces that roster.

**Where you put that tag decides what it does.** In a message, a greeting or a choice it rolls fresh and the entry belongs to that message, so deleting or rerolling the message takes the entry with it. In the CARD's own text it rolls once, when a chat begins, which is how a card starts every playthrough with its own cast. There, `as.entry=` leaves the entry open for the story to revise and `as.entry.static=` fixes it so the story can never touch it. Entries a card seeded are marked *from the card*, and deleting one makes it stay deleted.

The two lists in the panel are two lifetimes. **Yours** belongs to the chat, and a branch copies it. **Written by the story** belongs to the message that wrote each one, so deleting, rerolling or branching away from that message takes the entry too. Editing one of those stops it rewinding that way, though the story can still revise it later, and renaming it keeps the old name quietly attached so the character's next write still finds it rather than starting a second entry about the same person. **Save as a shared lorebook** copies the whole thing into your Lorebooks library when a chat turns up something worth keeping.

> **Tip:** The card's own lorebook is never touched by any of this, and nothing reaches your library unless you press that button.
