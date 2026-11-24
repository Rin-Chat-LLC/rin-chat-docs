# Writing Rin Chat lorebooks

> For AI models and tools that write and tune Rin Chat lorebooks. Generated from the app's own Guide on every release (this copy is version 1.0.6), so it matches what the app does. "Settings → …" names a place in the app's Settings.

- A lorebook can be its own file or embedded in a card as `character_book`. Its entry fields: https://rin.chat/card-format.md
- "Memory & context" and "Prompting & steering" are included because they decide where lore lands in the prompt and what it competes with for space.

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

## Memory & context

A model only sees a limited window of text. Rin gives you five separate tools for keeping what matters, and they stack. Everything below is configured in Settings → Memory & Context, except the context budget itself.

**The context budget** is `Max context tokens` on your generation config in Settings → Generation Configs — **8192** on a new config. If a provider declares its own ceiling, the smaller of the two wins. When the budget is full the *oldest chat turns* are dropped first; the newest message is always kept, and some room is held back for the reply that's coming. See exactly what's filling the window in the Session-info inspector's budget view.

> **Tip:** **Some blocks are never trimmed.** The character block, your persona, pinned facts, the chat summary, the scene note and the recall block are each sent as their own system message and always survive. They don't get dropped — they shrink the room left for actual conversation. That's why a chat with a big lorebook and a long summary can feel like it forgets quickly.

- **Pinned facts**: short statements injected into *every* turn. Add them in the Memory modal, or click **📌** on any message to pin its text (📌 turns orange; click again to unpin). Best for rules and relationships, not events.
- **Chat memory (summary)**: a rolling prose summary of the story so far, kept in its own system message. Write or edit it from the **Summarize** button in the chat's context bar. It **does not update on its own** unless you tick *Rewrite automatically* (off by default, then every **10** messages). You can point it at a cheaper model and rewrite the prompt it uses.
- **Scene tracker**: an AI-maintained note of where you are right now. On by default; see the **Scene tracker** section of this guide.
- **Drop first messages**: excludes the oldest N messages from what's sent while leaving them visible. Summarize first, or you lose them.
- **Recall past messages**: searches everything that has scrolled away. The rest of this section is about that.

#### Recall past messages (experimental)

Off by default. It needs **two** switches: the global one in Settings → Memory & Context, and a per-chat one in that chat's Memory drawer. Turning it on globally turns it on for every chat; any chat can opt out.

**It defeats prompt caching.** The injected block changes every turn, so a chat that currently gets cache hits will get slower and cost more. It earns its keep on long chats and small context windows, not on short ones.

**Only messages that have already scrolled out of context are indexed** — all of them, right up to the edge. If your whole chat still fits in the window, the index is empty — that's correct, not broken. The model can already read those turns. A section is only *summarized* once its messages have been out of context for a few turns: regenerating a reply changes its length, which moves the edge by a message or two, and waiting keeps that from costing summary calls. Until then those excerpts reach the model word for word.

#### The memory tree (on by default)

Older messages are grouped into verbatim *excerpts* (~700 characters, always whole messages). Every **4** complete excerpts become a *section*, which one model call summarizes. Once **2** sections exist, they're summarized again into a single *overview*. With the defaults that means **4 complete excerpts must scroll out of context before the first summary appears**, and 8 before the overview does.

Every turn, the overview plus each section summary — numbered `Part 3 of 11` so the model knows the order — is injected *regardless of what you're talking about*. That's the part that answers "what did we play at the beginning", a question containing none of its own subject for a search to find. Sections whose messages are all back in context are skipped. Then a similarity search pulls up to **3** matching excerpts back *word for word*, each labeled with the section it came from.

Summaries are written by the model you picked for **Chat memory**, one call per section, and each is saved as it completes — so a rate limit costs one call, not the run. Press **Vectorize all** again to continue. **While the tree is running for a chat, the rolling summary is neither sent nor updated** — the two would summarize the same messages twice.

Open the tree from the chat's Memory drawer to read the overview and every part, expand a part to see the exact excerpts it was written from, and **edit any summary by hand**. An edit sticks until the messages underneath it actually change.

- **Turn the tree off** and you get plain similarity search instead: messages are cut into 400-character chunks and the closest ones are injected wherever you choose (in the chat at a depth, or in the system block). No summaries, no model calls, no answer to "when did that happen".

#### Embeddings

- **In-app model** (default) — runs locally, free, private, works with every provider, downloads about 23 MB the first time.
- **Provider API**: calls `/v1/embeddings` on an OpenAI-compatible endpoint. Billed per token *separately* from a flat-rate chat subscription, and many endpoints don't offer it at all.

> **Tip:** **Changing the embedding source or model invalidates every stored vector.** Vectors from two models can't be compared, so old rows are ignored rather than silently mis-ranked. Press **Clear index**, then **Vectorize all**.

**Nothing being recalled?** Use **Test recall** in the Memory drawer. It shows the exact search text, how many chunks are searchable, and the top matches *including the ones the threshold rejected* — so "nothing similar" and "the threshold is too high" stop looking identical. Search by *subject* ("the gardening game"), not by question ("what did we do at the start?").

Branching a chat copies its whole memory across, so a branch never re-summarizes from scratch.

```
Pinned fact examples:
        - Anya is the user's younger sister and doesn't know about the heist.
        - The story is set in 1920s Chicago. Keep tech period-accurate.
        - {{char}} is secretly afraid of the dark.
```

## Prompting & steering

Shape how the model behaves, globally and per chat. The system-prompt side lives in Settings → System Prompts as named **prompt configs** — bundles you can keep one of per card, model or API.

> **Tip:** The **Default** config is read-only: it's the baseline, so it can't drift. Hit **Duplicate** to get an editable copy — that's the answer to "why is everything grayed out?". The one exception is the Author's Note, which you can edit on Default too, because it's your content.

**Export**, at the top of a prompt config, saves it as a file you can share, and **Import config**, under the list, adds one as a new config beside yours. The impersonation model choice stays behind, since it points at a config on your own machine.

- **Main system prompt**: the baseline instruction the model receives. Supports `{{char}}` / `{{user}}`. *Prefer a character's own system prompt* (on by default) lets a card that ships one override this.
- **Jailbreak prompt**: appended *after* the chat history every turn. **Empty by default**; most modern and open-source models don't need one. Only reach for it if you're getting refusals. A card's own post-history instructions are sent first and the jailbreak after them. Turn on *Prefer a character's own post-history instructions* (off by default) and a card that ships them sends those instead of the jailbreak, as SillyTavern does; a card that writes `{{original}}` in them still gets the jailbreak, placed there.
- **Author's Note**: free text injected near the end of the prompt every turn, at a *depth* in turns from the end (default 4) and a *role* (default system). A strong steer for tone or an upcoming beat. What you set here is the **default for new chats** on this config; each chat's own note (the sticky-note button in the chat) overrides it.
- **Context template** *(advanced)* — the "story string" that lays out the system block: `{{system}}`, `{{wiBefore}}`, `{{generalDescription}}`, `{{appearance}}`, `{{corePersonality}}`, `{{behaviorRules}}`, `{{speechStyle}}`, `{{backgroundHistory}}`, `{{worldSetting}}`, `{{dialogueExamples}}`, `{{wiAfter}}`, with `{{if x}}…{{/if}}` conditionals (comparisons, `&&`, `||`, and `{{.messageCount}}` for "drop this once the chat gets going"). Leave it blank to use the built-in layout. Your **persona is always its own system message** and can't be placed in the template.
- **Start reply with**: seeds every reply with text the model continues from, e.g. an opening quote to force a format. **Off by default**, and only applied to full replies, not to Continue or Impersonate. Some backends dislike a non-user final message — clear it if replies stop working.
- **Include names**: tags each turn with the speaker's name in the request (the OpenAI `name` field) rather than altering the text. Some weaker models keep speakers straighter this way. Ignored in group chats, which already label every turn.
- **Group chat prompt**: the system instruction telling the responder who else is present and to stay in their own character. `{{names}}` = participants, `{{char}}` = the responder. Empty sends nothing.

**The utility prompts** — one per button, all editable on a duplicated config:

- **Impersonation prompt**: what **Impersonate** (and `/impersonate`, and the Story Director writing your turns) is told. It can also point at its **own provider and model**: drafting your voice is a small job, so a fast non-thinking model here keeps it snappy without touching the chat's model. That picker only appears on an editable config.
- **Continue nudge**: appended when the **Continue** button extends the last reply.
- **Regenerate nudge**: prepended to your directions when you Shift-click **Regenerate**, pick a quick dial, or give a group member directions. Your text follows it.

**Quick dials** are the one-click buttons in that regenerate-directions box. The built-in ten are Shorter, Longer, More action, Slower, Spicier, Lighter, Funnier, Darker, More dialogue and Less repetitive. Edit them, reorder them, add your own, or bring the ten back with **Reset to defaults**, all in the **Regenerate quick prompts** card of Settings → Quick Actions. A dial with empty directions is hidden.

**Adventure Mode GM prompt** (a card on the prompt config, editable on a duplicate) is what Adventure Mode teaches the game master, in three layers. **Core** is the part the app's own parsers read, so it's always sent and can't be edited. **Ruleset** is how a roll is read (targets, modifiers, damage dice, rests, hp at zero) and **GM style** is how the game is run (when to roll, pacing, judging spoken words, using the campaign plan). **Load default text** puts the built-in wording in the box to edit, and **Reset to default** goes back to it. An adventure card can override either layer for itself; see Writing an adventure card.

**Response language** (its own card in Settings → System Prompts) is added to every AI request, whichever prompt config is active: chat, summaries, scene notes, chat titles, lorebook entries and Card Forge. Leave *Language* blank and the AI writes in the interface's language; fill it in (Japanese, 简体中文, Brazilian Portuguese) to use another. It covers lorebook keywords too, since an English keyword never fires in a chat written in another language. Turn off *Character replies too* to keep the roleplay itself in the card's own language while everything else follows the setting.

**Response length** and the auto-continue / auto-swipe behaviors are covered under Sampling & generation settings.

**Reasoning controls:** for thinking models, set the effort and how it's requested *per generation config*, in the *Reasoning* section under the config's model in Settings → Generation Configs (see Providers for each field). Chain-of-thought is folded into a collapsible *Thinking* block and is **never sent back** to the model, whichever way that block is displayed. Editing a reply that has one gives you separate boxes for the thinking and the reply; clearing the thinking box deletes the block. Whether the block starts expanded is a single global setting in Settings → Chat Display.

> **Tip:** **Model still thinks with effort set to None?** GLM 4.5+ and Qwen 3 ignore `reasoning_effort` entirely; whether they think is decided by their chat template. At effort *None* the app handles them itself: your last message gets the model's in-prompt tag (`/nothink` for GLM, `/no_think` for Qwen), and GLM, Qwen, Granite and Gemma models are also sent `chat_template_kwargs` turning thinking off. The config's *"Don't think" tag* only overrides the automatic tag; it's sent at effort *None*, and only ever to a GLM or Qwen model id. *Also send `chat_template_kwargs`* sends that flag at every effort tier, for another model that reads it; it needs a tier chosen (it isn't sent on *Auto*). The tag works even through gateways that drop unknown request fields. **DeepSeek has no in-prompt switch**: on the official API effort *None* sends `thinking: { type: "disabled" }`; on NanoGPT it sends `reasoning_effort: none`. Note DeepSeek V4 thinks by default, so *Auto* there means thinking ON.
