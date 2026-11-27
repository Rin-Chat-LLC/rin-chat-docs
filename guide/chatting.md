# Chatting in Rin Chat: Swipes, Personas and Direct Chat

> Read this on [rin.chat](https://rin.chat/guide/chatting/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

## Chatting: the essentials

Type and press `Enter` to send (`Shift`+`Enter` for a newline — flip this in Settings → Chat Display, which swaps them so `Ctrl`+`Enter` sends). Clicking **Send** with an *empty* box just asks for the next reply — pressing Enter on an empty box does nothing.

You can keep typing while a reply streams; only *sending* waits for it to finish.

The **📓 Notes** button in the chat topbar opens a scratchpad that belongs to this chat — your own notes, never sent to the model.

**Attachments** (📎 beside the message box): **images** go to the model as real vision input (downscaled automatically; needs a vision-capable model — with a text-only one the app resends the turn without images and tells you). **Documents** — pdf, docx, odt, epub — are converted to text on your machine, and plain text files (md, txt, json, csv, code…) attach directly. Attachments stay on their message, are re-sent with the history every turn, and file contents can't trigger macros or generators. The same paperclip lives in Direct Chat and the Card Forge.

[Screenshot] Hovering a message reveals its actions — **copy**, **edit**, **exclude from context**, **pin**, **bookmark**, **branch**, **delete**. The header carries the message number (`#7`) and the icon for what the model can see.

**On each message (hover for the buttons):**

- **Regenerate**: on the *last* reply only. Plain click re-rolls it; **Shift-click** opens a directions box with one-click quick prompts (Shorter, Spicier, Darker, Less repetitive…) — edit that list in Settings → Quick Actions. Directions you give the last reply are remembered and reused by the next plain click.
- **Swipes**: regenerating stacks alternates you can step through with `←` / `→` or the arrows under the reply. Stepping past the last one generates a fresh alternate. The trash icon deletes the alternate you're on.
- **Edit**: inline. If the reply has a thinking block it gets its own box; that box is never sent back to the model, and clearing it deletes the block.
- **Copy**: puts the message's text on your clipboard.
- **Speak aloud**: reads the message out, when a voice is set up in Settings → Voice Providers.
- **Delete**: removes that message. **Shift-click** removes it *and everything after it*.
- **Exclude from context** (eye): keeps a message on screen but stops sending it to the model.
- **Pin to memory** (pin): always kept in context — see Memory below.
- **Bookmark** (ribbon): a place-marker you can jump back to. It changes nothing about the prompt. Once anything is bookmarked, a bookmark button appears in the topbar to jump between them.
- **Add to lorebook** (book-plus): *make it canon*. Select the text that matters (or take the whole message) and a review window lets you edit the entry, set keywords, and save it into the **character's built-in lorebook** — active in every chat with them from then on. The Memory window's **Story so far** tab does the same at scene scale: pick how many entries (1, 2, 3 or 5 — 3 by default), press **Draft entries from this chat**, and the AI splits the recent chat into that many (one per distinct fact, each with its own keywords), shown together for review — accept or reject each, restructure any with AI, then save the keepers in one go.
- **Branch** (git-branch): forks a new chat from that point, named `Original (Branch #1)`, leaving this one intact.

**Find and replace in a chat:** `Ctrl`+`F` (or the search button in the chat's header) searches every message, including ones scrolled far out of view, with `Enter` / `Shift`+`Enter` to step and a Match case toggle. It reads every part of a message: thinking, hidden text, and the words on choice buttons, forms, rolls, reveals and documents. Matches elsewhere in the chat (Game Mode's DM stats, the player character, the party, the chat's own lorebook, memory, the Author's Note) are counted beside it. The **Replace** toggle adds a Replace with box: **Replace** changes the highlighted match and moves to the next, and **Replace all** changes every match in the whole chat at once, every swipe of every message included, and says where they were. A stat named after what you replace is renamed too ("Odette" to "Maisey" turns `odette_hp` into `maisey_hp`), along with every `[[stat:]]` and `{{getvar::}}` that names it; one whose new name is taken, or that the game's character sheet uses, is left alone and said so. Directive and tag names, option words, generator and picture names and macros are never changed, and neither is the card itself. **Undo** on the bar puts the whole replace back until you replace again or close the bar. Replace is off while a reply is being written.

**Graduate an NPC into a full card:** the Memory window's **Create character** tab. Say who to extract, pick the message range they appear in (the endpoints are previewed so you know you grabbed the right scene), and a new **Card Forge** session opens seeded with that excerpt — the guide builds them from how they actually behaved, and you refine from there.

> **Tip:** Everything except Copy and Speak is switched off while a reply is streaming — including the swipe arrows, which disappear entirely. Stop the reply, or let it finish, then act.

On an *older* reply the arrows only **preview** its alternates (the counter says "2 / 3 · preview"). The chat still uses the version it was written against, and nothing is saved.

Each message shows its number (`#12`) and an icon for what the model can see: an **eye** (sent in full), a **brain** (dropped from context, but your memory still covers it), a **crossed eye** (dropped, nothing catching it), or a **ban** sign (you excluded it).

**In the reply-tools row (above the box):**

[Screenshot] The reply-tools row, with the **context meter** above it — each segment is one thing filling the window, and **Summarize** appears on the right as it fills up.

- **Length**: Auto / Short (~80 words) / Medium (~240) / Long (~650). It *asks* the model for that length and caps the reply to match. If a thinking model spends the whole cap reasoning, see Sampling & generation settings.
- **Send with directions**: **Shift-click Send** (or `Ctrl`+`Shift`+`Enter`) to tell the model what its next reply should do ("keep it short", "have her admit the truth"). They go in right after your message for that one reply, aren't saved to the chat, and that reply's Regenerate reuses them. The wording that introduces them is the prompt profile's regenerate nudge.
- **Continue**: extends the last reply from where it stopped. Shift-click, or type `/continue your notes`, to steer how. Don't like where it went? **Undo** puts the reply back as it was before the Continue.
- **Impersonate**: the AI drafts *your* next message straight into the box (which goes read-only while it streams) so you can edit before sending. Shift-click (or `/impersonate …`) opens a bigger editor where you can page back through earlier drafts and highlight part of one to rewrite just that part.
- **Undo**: drops the last exchange and puts your message back in the box. If you've already started typing something else, your draft is kept instead.
- **Suggest**: three one-line ideas for what you could say, as chips above the box. Clicking one *fills the box*; it doesn't send.
- **Director**, **Image** and the **gallery** icon get their own sections below.

**Right-click:** highlight part of any message and right-click for **✨ Regenerate this section** — the model rewrites just that span in place, with the rest of the reply as context. Right-clicking in the message box gives you spelling suggestions, cut/copy/paste, and the formatting list, including **Add to dictionary** for a word the checker flags that you spell that way on purpose. `Shift`+right-click *in a text box* gets the system menu instead. Right-click any picture, in a chat, on a card or in the image generator, for **Save image** (to your Downloads folder) and **Copy image**.

The ⤢ button opens the message box full screen — same shortcuts, its own Send, `Esc` to close. If you've set up dictation, the microphone works even while a reply is streaming, and sending while it's recording stops it and appends what you said.

## Multiple chats at once

You can keep up to **five** chats open simultaneously — same or different characters. A reply keeps streaming in the background while you're in another chat, so you can bounce between them.

- Open another chat from the library or a card; it opens as its own session.
- The **stacked chip** (right end of the reply-tools row, or floating bottom-right when you're not in a chat) shows a count and opens a list of the other open chats — each with its character's avatar and a pulsing dot while it's generating. Click to jump; × to close.
- At five open, opening another closes the least-recently-used *idle* one. If all five are generating, nothing is closed — you're told to close one yourself.

> **Tip:** Send in chat A, switch to chat B and read/reply, then jump back — A's response will be waiting.

## Personas (who you are)

A **persona** is your side of the roleplay — and it is a **card**, like any character: give it art, tags, a folder, export it, share it. What makes a card a persona is one setting: the **Card type** dropdown in the editor header, set to **User Persona**. Pick one from the dropdown in the chat topbar, or choose **+ New temporary persona** to make one that lives only in this chat. The **Personas** row in the **Characters** sidebar is the library filtered to your persona cards.

**Your default persona** is the one every new chat starts with, unless the character has its own bound persona. Set it from a persona card's right-click menu or its editor's **Tools** menu (**Set as default persona**), the star beside a persona in the chat's persona dropdown, or Settings → Personas. The default wears a *default* badge in the library.

**A persona can carry lorebooks.** Its own lorebook, and any shared lorebooks attached in its editor's Lorebook section, work in every chat where you play as it, like a character's: an entry is sent when its keywords come up. In a group chat every character can read them, since you're in every scene.

A **game persona** is a persona with a character sheet for Game Mode — abilities, gear, level. Tick **Game character sheet** in the editor's Game features section (the type stays User Persona) and the **Character sheet** appears under it; the card then shows up in the Game Mode character builder's "Load a saved game persona" list, and characters you build there are saved as these cards.

Two names, two jobs: the **name** is what the model receives as `{{user}}` and what labels your lines in the transcript; the optional **display name** is only for the app's own dropdowns and message headers. Set just the name if you want them the same.

Your persona's **appearance** and **personality** go to the model every turn — the character needs to know who it's talking to. **Dialogue examples** don't: they're sent only when something is writing *as you* (`/impersonate`, the Story Director). Example lines are the strongest style cue a prompt carries, so including yours in every reply nudged characters toward your phrasing and encouraged them to write your side of the scene.

The **link** button beside the dropdown pins the current persona as the default for new chats with this character (click again to unbind). A *temporary* persona can't be pinned that way — use the pencil beside it to edit it, or **Save to my personas** to promote it to a real one first.

## Direct Chat (no character)

**Direct Chat** (a tab of **Tools** in the title bar) talks to the model with **no character, no persona, no system prompt, no lorebook and no macros** — only the messages you see are sent. Use it to test a new provider or model, compare models on the same question, or ask something that isn't roleplay at all.

- It keeps **several conversations**, saved between sessions. The dropdown at the top switches between them (labeled from their opening line) and **+ New chat** starts another.
- **Clear** empties the current conversation; **Delete** removes it (available once you have more than one).
- It has its **own provider and model picker**, separate from the one your character chats use — so you can point it at a model you're evaluating without disturbing anything.
- Messages have **Copy** and **Speak aloud**, and dictation works here too.
- **Attachments** (paperclip): **images** go to the model as real vision input (downscaled automatically; needs a vision-capable model — a text-only one refuses with a clear message). **Documents** — pdf, docx, odt, epub — are converted to text on your machine and attached under the file's name, and plain text files (md, txt, json, csv, code…) attach directly. The model re-reads attachments on every turn, so you can keep asking about them.

> **Tip:** If a character chat behaves oddly, ask the same question in Direct Chat: a clean answer here means the problem is in the card, lore, or settings — not the model.

## AI chat titles & suggestions

Small AI helpers that reduce busywork, all using your active provider:

- **Chat titles:** the ✨ button beside the chat name names it from its opening exchange; new chats can be auto-titled after N messages. Both only appear/run when AI chat titles are enabled — toggle it in Settings → Chat Display.
- **Rename by hand:** click the chat name. Enter saves, `Esc` cancels.
- **Pin the name:** the pin beside ✨ locks it so AI naming can't change it — useful for a chat you've named deliberately and keep auto-titling.
- **Former names:** the history icon lists the last 20 names this chat has had, with how long ago each was replaced. Click one to rename back to it.
- **Suggested replies:** the **Suggest** button offers a few things you could say next — clicking one fills the message box rather than sending it.
- **Scene & group director:** covered in Scene tracker and Group chats above.
