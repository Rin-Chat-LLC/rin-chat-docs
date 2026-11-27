# Interactive Replies: Choices, Command Tags, Generators

> Read this on [rin.chat](https://rin.chat/guide/interactive-replies/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

## Choices & interactive replies

Any card can give the player buttons, inputs and other interactive bits in a reply — not just Game Mode cards. These are a Rin Chat feature written into the card's text (part of the AICC format); in other apps the lines just show as plain text. Each element goes on a line of its own. In the card editor, type `[[` in a greeting, example dialogue, system prompt or lorebook entry for a list of every element with a short note on each; pick one and it drops in ready to fill.

**Clickable choices**

A line that holds only `[[choice: …]]` shows as a button right where you wrote it, and clicking it sends that text as the player's message. Put a few in your greeting and example dialogues and the model picks up the pattern; you can group them under headings to make a menu:

```
*The innkeeper wipes the bar and waits.*

**Explore**
[[choice: Search the cellar]]
[[choice: Climb to the attic]]
**Talk**
[[choice: Ask about the missing map]]
```

- Only the latest reply's buttons work. Earlier ones stay visible, grayed out, as a record of what was offered.
- With the message box empty and the cursor out of any text box, `1` to `9` pick the newest reply's choices in order, skipping locked and hidden ones.
- A choice has to be on a line of its own. `[[choice: …]]` in the middle of a sentence stays as text, so you can still write about the syntax.
- A choice can roll one of the card's generators when it's picked: `[[choice: Draw a card | gen Tarot]]`. Stat changes and dice need a Game Mode card (Writing a Game Mode card).

**More interactive elements** — same rule, each on a line of its own, on any card:

- **Timed choice**: `[[choice: Freeze | timer 15]]` puts a countdown under the reply; if you don't pick in time, *that* choice is sent for you. The clock only starts once the choices are on screen, adds time for how long the reply is to read, and pauses while you type or with its Pause button. Don't want it? Settings → Chat Display → Timed choices.
- **Reactions**: the smiley button under the newest reply puts one emoji on it, and the character is told once, on its next turn ("Sam reacted 😂 to your last reply."). Tap it again to take it back; a regenerated reply starts with none. They are off until you turn them on for every chat in Settings → Chat Display → Reactions, or a card turns them on for itself with `"reactions": true` in its features (the card editor's Game features section has a switch for it, under Play), or picks its own few with `"reactions": { "set": ["🔥", "🌙"] }`. A card's `"reactions": false` keeps them off in its chats whatever the setting says.
- **Ask**: `[[ask: What do you name the dragon? | as dragon]]` shows a text box; your answer is sent as your message, and with `as` it's also saved, so the card can use `{{getvar::dragon}}` from then on.
- **Choices that remember**: `[[choice: Take the brass key | set has_key = true]]` saves a story variable when it's picked (`+=` and `-=` count up and down: `set coins -= 5`; several at once: `set a = 1, b = 2`). The model is told what changed, and the card can read it with `{{getvar::has_key}}`.
- **Lists**: `+=` and `-=` with a word keep a list, `set inventory += rope`: one item each time, no duplicates, any case, read back as `rope, lantern`. On a number they still add and subtract, and a word there changes nothing; a number added to a list joins it as an item. `if inventory has rope` checks for that whole item (`inventory has key` is false for a keycard), while `contains` matches any part of the text.
- **Choices you finish yourself**: `[[choice: Ask about the map | insert I catch up and ask about ]]` puts its text in your message box instead of sending it, so you can complete the sentence before you send. The button shows a pencil. Everything after `insert` is dropped in exactly as written.
- **Locked choices**: `[[choice: Open the chest | if has_key | hint You need a key]]` shows with a lock, and the hint, until the condition holds — then it opens by itself. Conditions: `has_key`, `!has_key`, `coins >= 5`, `door == open`, `inventory has rope`, joined with `&&` or `||`. Add `| hide` to keep a choice secret until it's available. On a Game Mode card, a condition can read the character sheet too (`if gold >= 10`).
- **Choices that finish something**: `[[choice: Take the ember shard | set shard_ember = true | once]]` disappears once everything it sets with `=` already holds, so the story stops offering the kiln after the shard is taken. `+=` and `-=` don't count, so `set gold -= 2, bread += 1 | once` can be bought again. With `| if` too, it shows only while that condition holds and its sets aren't all done; when the condition fails it's hidden, not locked, so no hint shows. A set value with `&&`, `||` or a bracket turns `once` off. It never shows on the button.
- **Grouping a condition**: brackets decide what goes together, `[[choice: Bribe the guard | if has_coin && (charm >= 3 || knows_name)]]`, and `!` in front of a bracket flips the whole group: `if !(guard_awake || alarm)`. Without brackets, `&&` is read before `||`, as before. Brackets inside a value are just text (`title == Mr (Smith)`), and a bracket left open, a stray one or an empty `()` keeps the choice locked rather than guessing.
- **Choice tones**: `[[choice: Jump the gap | tone danger]]` gives the button a red edge and arrow. The tones are `danger` (red), `good` (green), `warn` (amber) and `quiet` (muted), in your theme's colors. It only changes the look, never the label, and works beside `if`, `hint`, `set` and the rest; an unknown tone word is ignored.
- **The story can change variables too**: a reply line `[[set: door = open]]` is applied by the app and never shown — for when the character hands something over or a door opens without a choice. Regenerating, swiping or deleting that reply puts the variables back. The line still counts when the model writes it in bold, as a bullet, with a period after it, or at the very end of a sentence; in the middle of a sentence it stays as text, and so does one in code formatting at the end of a sentence, which reads as an example. The same goes for a `[[lore:]]` line.
- **What the model sees**: older replies' choices, question boxes and forms stay on screen, but only the newest set is sent to the model, which saves tokens in a long chat. **Send only the newest buttons** in Settings → Memory & Context turns this off. `[[set:]]` lines and documents are always sent.
- **Tip for game cards**: build progress from what the *player* does, not from the model remembering a rule. A vault that opens on `if shard_ember && password == whiskers && shard_echo` — each part set by the player's own choice or answer — works however loosely the model plays along.
- **Pick several**: `[[pick 2: Sword | Shield | Bow | Lantern]]` lets you tick exactly two, then Confirm. Leave out the number to allow any amount. To ask a question above the boxes, put it first, in quotes: `[[pick: "What defines you?" | Strong | Agile | Sharp | as trait]]`.
- **Scales**: a slider for "how much" questions: `[[scale: How much do you trust her? | 1-10 | Not at all ... Completely | as trust]]`. The first part is the question; then a range, two numbers like `1-10` or `0-100 step 5` (1-10 when you leave it out); then, if you like, the words under each end, separated by three dots. The slider starts in the middle and Send sends the number as your message. With `as trust` the number is also saved, so `[[choice: Tell her the secret | if trust >= 7]]` can depend on it.
- **Forms**: several questions answered at once, on one line: `[[form: Your character | Name: text | Age: number 18-99 | Class: Fighter, Mage, Rogue | Backstory: long text optional | Has a pet: checkbox | as hero]]`. The first part is the form's title; each part after it is a field, its label, a colon, then what kind of field: `text`, `long text`, `number` (with an optional range like `18-99`), a list of options separated by commas, or `checkbox`. Every field must be filled unless you write `optional` after its kind; Send points at whatever is missing. The answers arrive as your message, one "Label: answer" line each. With `as hero`, each answer is also saved as a variable named after its label: `hero_name`, `hero_age`, `hero_has_a_pet` (a checkbox saves yes or no), for `{{.hero_name}}` later. Forms are drawn by the app, so there are no password or file fields, and nothing a form holds ever leaves the chat.
- **A form that rolls a generator**: a field written `Gender: from Contestant / Gender` is a list of that generator field's own options, and a `roll Contestant as=contestant as.entry="Contestant"` part rolls the generator when the form is sent, with anything a `[Create …]` tag takes (add `hidden` to show the result to the character only). The fields you picked are kept and the rest are rolled; an `optional` list left blank shows as Random and rolls too. A bare `roll` rolls every generator the form's fields come from. The roll is part of your message, so the reply can already use it: `[[form: Your rival | Gender: from Contestant / Gender optional | roll Contestant as=contestant as.entry="Contestant"]]`, then `{{getvar::contestant.Name}}`. A new swipe keeps the same roll; deleting your message takes it back.
- **A form as a character sheet**: on a Game Mode card, end the form with `as sheet` and the answers become your character's own stats, on the character sheet beside everything the builder and the game master put there: `[[form: Your hero | Class: Fighter, Mage, Rogue | Strength: number 3-18 | Agility: number 3-18 | as sheet]]`. You can edit them on the sheet like any other stat, and the game master reads them every turn. A character can also be built step by step: an ability the form leaves out, or every ability of a sheet left blank in the builder, is the game master's to choose with you in play. It records each one once, and from then on that score is yours to change, not the game master's. On an ordinary card, save the answers with any `as` name and have replies show them back as a sheet with a document block. The app fills the values in whenever the reply is displayed, so the sheet is always current. To have the model write it, put the block in the card's rules with each brace escaped, `\{\{.hero_name\}\}`: a plain `{{.hero_name}}` there is filled in before the model reads it, so it would copy today's value instead of the macro:

  ```
  [[doc: paper | Guild papers]]
  **Name:** {{.hero_name}}
  **Age:** {{.hero_age}}
  **Class:** {{.hero_class}}
  [[/doc]]
  ```
- **Reveal**: a folded note you click open. Nothing is sent, and the model isn't asked anything — good for letters, clues and item descriptions:

```
[[reveal: Read the letter]]
*The ink is smudged.* Meet me at the old mill at midnight. Come alone.
[[/reveal]]
```

Choices, asks and picks can sit inside a reveal, as long as the reveal ends with its own `[[/reveal]]`: they appear when it's opened. A document can't hold them, though: a choice inside a `[[doc:]]` ends the document there.

**Documents** show a block as a real object: `paper`, `newspaper`, `terminal`, `sms` or another built-in style (Document styles, below, lists them all), with an optional title after a `|`. In `sms`, `social-thread`, `stream-chat` and `phone-messages`, write one `Name: message` per line; your own lines are set apart.

```
[[doc: sms | Kisa]]
Kisa: you up?
{{user}}: now I am
Kisa: come outside. look up.
[[/doc]]
```

You can make your own document styles (how, with examples: Document styles) in Document styles, a Library tab: HTML with placeholders for where the text goes ({{title}}, {{content}}, and a line template for message threads), plus CSS for that style alone. Duplicate a built-in to start from its real template. A card can carry its own styles too, in the card editor's Document styles section, and they work in its chats. A style nobody has shows plain. Reveal and documents also work in your own messages; the other elements only in replies.

**Menus**

A menu is a set of choice lines written once in the card editor's **Menus** section: a shop, places to travel, people to talk to. The AI is told each menu's name and offers one by writing `[[menu: shop]]` on a line of its own, and the chat draws your lines in its place exactly as written, so prices and conditions never drift:

```
[[choice: Buy a potion (5 gold) | if gold >= 5 | set gold -= 5]]
[[choice: Buy a lantern (8 gold) | if gold >= 8 | set gold -= 8, lantern = true | once]]
[[choice: Leave the shop]]
```

- **Names** are lowercase letters, digits, dashes and underscores. The line still counts in bold, as a bullet, with a period after it, or at the end of a line after other text; in the middle of a sentence, inside a code block, or with a name the card doesn't have, it stays as written.
- **Everything a choice carries works**: conditions, sets, costs, tones, timers, `once`, and macros, which are filled in when the menu is drawn. A menu line that names another menu isn't opened.
- **Arguments**: write `{{arg}}` in a menu's lines and the AI fills it after a bar, so one "talk to" menu serves everyone: `[[menu: talk | Mira]]`. For more than one, use `{{arg1}}` to `{{arg9}}`. They're filled before macros, so `{{getvar::trust_{{arg}}}}` reads a separate variable for each character. A missing argument is empty and extra ones are ignored. Brackets, braces, bars, commas, `=`, `<`, `>`, `&` and line breaks are taken out of an argument, and it stops at 60 characters, so the AI can't add a cost or a condition through it. The AI is taught the bar only when one of the card's menus uses `{{arg}}`.
- **When the AI should offer it** is an optional note of up to 200 characters, sent beside the menu's name: "when the player is in a store".
- **Add a starter** adds a ready-made `shop`, `travel` or `talk` menu (costs, a one-time purchase, a locked route with its hint, a talk menu that fills in whoever you're speaking to), each with its note for the AI. Beside each menu the editor shows the exact line the AI writes to call it, `[[menu: talk | <name>]]` included.
- **Size**: up to 32 menus a card, 4,000 characters each. A longer menu is cut at its last line break before that; one with no line break there isn't used, and its `[[menu:]]` line shows as written.
- Menus travel with the card in every export, SillyTavern files included. On Discord a menu's choices are listed like any others, though macros inside a menu aren't filled in there.

**Action bar**

Buttons that stay above the message box for the whole chat, written once in the card editor's **Action bar** section, one `[[choice: …]]` line per button:

```
[[choice: Look around]]
[[choice: Check my bag | if has_bag | hint You need a bag]]
[[choice: Write in my journal | insert Dear diary,]]
```

- They follow the rules of choices in a reply: `if`, `hint`, `hide`, `once`, `set`, `ask`, `insert` and `tone` all work, and a locked button opens the moment the story sets what it needs. `[[menu: name]]` puts one of the card's menus in the bar. A `timer` does nothing there, and lines that aren't choices are ignored.
- The bar is grayed out while a reply is being written or a roll is waiting, isn't shown in DM mode, and is never sent to the AI. In a group chat it's the main card's.

**Status panel**

A panel above the message box, and above the action bar, that redraws whenever a variable changes. Write it in the card editor's **Status panel** section with document blocks, `[[bar:]]` gauges and `{{getvar::…}}`, up to 4,000 characters (`status-window` is one of the document style starters):

```
[[doc: status-window | Status]]
Hunger: [[bar: hunger / 10]]
Gold: {{getvar::gold}}
[[/doc]]
```

Choices in it show as plain text, so put buttons in the action bar. It shows in DM mode too, uses the main card's panel in a group chat, and is never sent to the AI.

**Status bars**

A small gauge, drawn wherever you write it. It's the only element that doesn't need a line of its own, so it can sit in a sentence or at the end of a status line. Either side can be a number or the name of a story variable, so both ends can move as the story goes:

```
Hunger: [[bar: hunger / 10]]
Rations: [[bar: rations / capacity]]      reads both from variables
Heat:    [[bar: heat / 10 | 20]]          20 cells wide instead of 10
Fever:   [[bar: fever / 10 | invert]]     full is the BAD end
```

- **What fills it** is the first number over the second. Past the top it's simply full, and below zero it's empty. A variable that isn't set yet draws an empty bar, so turn one looks right; a *second* number that can't be read is a mistake in the card, so the line stays on screen as written for you to spot.
- **What drives the color** is that same fraction, not the variable's name: over 60% draws in the theme's accent, 60% or under turns orange, 25% or under turns red. That assumes full is the good news: health, fuel, trust.
- **For a bar you want low** (infection, suspicion, heat), add `| invert` and the colors read from the other end, so 9 out of 10 shows nine lit cells in red. It only changes the color; the fill still follows the value. The width and `invert` can come in either order.
- **The bar itself is never sent to the model**: the app draws it from the text. The text around it *is* sent, though, so to keep a status line out of the model's view entirely, wrap it in `[co]…[/co]` or add it with a display-only regex script.

For a status line under every reply rather than one the card writes by hand, the card editor's Regex section has **Add text to every message**. It builds the pattern and ends it with a `[co]…[/co]` pair, with the caret between the tags: what you write there is in the chat and out of the model's view. It writes into the message as it arrives, so you can edit it with the ordinary Edit action, and writing the sides as macros, `[[bar: {{.hunger}} / 10]]`, freezes the numbers to that turn instead of following the variables afterwards (Regex scripts).

> **Tip:** For the logic behind these — variables, `{{if}}`, `{{calc}}`, `{{switch}}` and meters — see Variables & macros.

## Command tags

**Command tags** are square-bracket tags typed straight into a message. They change what the model sees or what you see. In the composer, type `[` for an autocomplete.

#### What the model sees

- `[once]…[/once]`: **sent for this turn only.** A one-off nudge: a dream, a noise outside, a private instruction. It survives **rerolls, swipes and Continue** of that turn — those are all attempts at the same beat — and is deleted the moment you send your next message, so it never lingers in the context.
- `[ooc]…[/ooc]`: out of character. Shown grayed in the chat and labeled to the model as an aside from you rather than as story.
- `[h]…[/h]` (or `[hide]`) — **hidden from you, kept for the model.** Stage directions you don't want cluttering the transcript.
- `[co]…[/co]` (or `[chatonly]`) — the reverse: **shown in the chat, never sent to the model.** Notes to yourself.
- `[code]…[/code]`: **inert everywhere.** No macros run, no generator tags fire, no markdown. Use it to show a literal `{{macro}}` or `[tag]`, or to keep a block of text exactly as typed.

> **Tip:** `[h]` and `[co]` don't need closing — an unclosed one applies to the end of the message. Handy, and worth knowing before you wonder where the rest of your message went.

#### Looks

- `[small]…[/small]`, `[center]…[/center]`, `[color=crimson]…[/color]`.
- `color=` takes a hex value (`#f80`, `#ff8800`), a CSS color name (`red`, `gold`), or `rgb(…)`. Anything else is ignored and the text renders plain.

> **Tip:** **These three change how text looks, not whether it's sent.** The model still reads the words inside — only the markup is stripped. If you want text the model never sees, use `[co]`.

```
Give the character a dream, once:
        [once]Last night {{char}} dreamt of the lighthouse again.[/once]
        So — sleep well?

        A private note to yourself, never sent:
        [co]remember: he still doesn't know about the letter[/co]

        Show a macro literally instead of running it:
        Type [code]{{roll::d20}}[/code] to roll a die.
```

## Generators (randomizers & wheels)

The Library's **Generators** tab builds reusable weighted randomizers — NPC generators with dependent fields, numeric ranges (heights, sizes), and outcome "wheels". Two examples, **HumanNPC** and **Outcome**, are there on first run. You can import and export them as JSON.

Sort them into **folders** with the sidebar, the same way as shared lorebooks: drag a generator onto a folder, or use the folder button on its tile's corner. A folder only sorts the page; `[Create …]` finds a generator wherever it is filed.

#### Fields

- **Options**: a list of values, each with a weight. Higher weight, more likely; a weight of 0 or less removes the option. Values can contain macros (`{{user}}` works).
- **Range**: a number rolled between Min and Max by Step, displayed as a plain number (with an optional unit), as *inches* (`7"`), or as *height* (`5'11"`).
- **Only if …**: makes a field depend on another field's value, so gender-specific or class-specific details only roll when they apply. A skipped field's placeholder blanks out, and a template line made only of skipped fields is dropped entirely.

#### Output

- **Template**: write the result yourself with single-brace `{Field}` placeholders. Leave it blank and a one-field generator prints its bare value, while a multi-field one prints `Field: value` lines.
- **Message prefix / suffix**: text before and after the rolled block; a wheel usually wants a suffix telling the model what to do with the result.
- **Send results silently**: the result is wrapped in `[h]`: sent to the model, hidden from the chat.
- **Chained generators**: run others afterwards and append their output, each with its own chance of running and, optionally, a condition on this generator's roll (*Female Name only if Gender is Female*).

#### Four ways to fire one

- **You type it:** `[Create HumanNPC]`, or with pinned fields `[Create HumanNPC Gender=Female, Hair Color=Auburn]`. It's rolled **once and frozen** into that message, so it never re-randomizes. Works in greetings, in messages you send, in quick replies, in lorebook entries, and in the model's replies. A tag inside `[code]` won't fire. Add `x3` to roll it several times in one go (`[Create HumanNPC x3]`, up to 10), or a range like `x1-3` to roll how many too, and `as=guard` to keep the result as chat variables: `{{getvar::guard}}` is the whole text, `{{getvar::guard.Hair Color}}` one field — and with a count, `{{getvar::guard.2.Hair Color}}` and `{{getvar::guard.count}}`. They last as long as the message does. Tick *No repeats in a batch* on a field (a name, say) so one `x3` never rolls the same value twice.
- **Auto-fire**: give the generator a % chance to fire on its own each reply. Events are injected as a note at the end of the recent history, capped at two per turn, and are stable across swipes of the same turn.
- **The model calls it**: tick *Let the model trigger this* and it becomes a tool the model can call to roll. Needs a provider with tool calling on. The tool takes no arguments on purpose: the model can ask for a roll, but it can't cherry-pick the outcome.
- **The model offers a button** (Game Mode) — the model ends a reply with `[[gen: HumanNPC | Draw a contestant]]` and the player gets a button that fires the generator on click (the label is optional). Unlike model tool-calling this needs no tool support, so it works on **every** model — and it's more reliable than asking the model to type a bare `[Create …]` itself, which weaker models tend to narrate as prose instead of emitting. Ideal for a spin, a draw, or a random encounter the player pulls.

> **Tip:** **Auto-fire and model tool-calling only work for generators attached to a card** (the card editor's Generators section), the character's or the persona you're chatting as. A generator sitting in the library will still expand when you type `[Create …]`, but it won't fire on its own — otherwise one character's random events would show up in every unrelated chat.

> **Tip:** **A tag in the card itself** — its description, personality, background, setting, system prompt or example dialogues — is rolled once when a chat starts and stays the same for that chat. Behavior rules are the exception: they're sent as written, so the model reads a tag there as the command to type, and it only rolls when a reply contains it.

```
[Create HumanNPC Gender=Male]
        → Male, 5'11" tall, Slim build, with Auburn hair.
          Personality: Bratty.
          Facial hair: Stubble.

        A wheel, sent silently:
        [Create Outcome]
        → [h] Outcome: {{user}} partially fails
              Narrate the consequence in-world. [/h]
```

## Quick replies

One-tap buttons at the end of the reply-tools row that send a preset message — or, if you mark it "insert only", drop it into the box for you to edit first. Messages support the same `{{macros}}` as chat and are expanded when they're sent. Create them in Settings → Quick Actions.

A preset can be **disabled** to hide its button without deleting it, and the buttons are switched off while a reply is generating.

```
Label: "Look around"   Message: *{{user}} scans the room carefully.*
```
