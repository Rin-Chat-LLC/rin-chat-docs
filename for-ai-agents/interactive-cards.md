# Writing interactive Rin Chat cards

> For AI models and tools that write interactive Rin Chat cards: choice buttons, forms, living worlds, document styles, macros, generators and regex scripts. Generated from the app's own Guide on every release (this copy is version 1.0.7), so it matches what the app does. "Settings → …" names a place in the app's Settings.

- A card is an AICC character card. Its full shape and rules: https://rin.chat/card-format.md
- The markup below is written in the card's text (greetings, description, system prompt) or taught to the model there, so the model writes it in its replies and the app renders it.
- For a dice game with a character sheet, use https://rin.chat/updates/adventure-game.md instead.

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

## World sim: clock, places and relationships

A card can give its chats a living world: an in-story clock with a calendar, places to go, characters with routines, relationship meters, gifts, dated events, an end-of-day diary, texting between scenes and endings to collect. Each is a card feature that works in any chat, with or without Game Mode or the visual novel stage. The app keeps the world's state, tells the AI where things stand every turn and undoes it with the reply: swipe, delete, regenerate or branch, and the clock, place and meters go back too. A card that uses none of it behaves exactly as before. In a group chat the main card's world runs the chat.

#### What you see in a world chat

- **Status chips** in a strip above the chat (the Game Mode one, when that's on): the time (*Tue 14:20*, a sun or moon by the time of day), the place you're in and each relationship meter. The strip folds away, and starts folded on a phone.
- **Story** and **See** at the start of the tool row change how your next message is sent. Story sends it as narration for the AI to continue from, and See asks what your character notices without any time passing (empty See looks around). Click one to pick it; it applies to that one message, then goes back to sending what you type as it is. Click it again to unpick it. They show in Game Mode and on the visual novel stage, and in any chat whose card turns them on.
- **Calendar** lists the events still to come, with what each needs (a day, a time, a place, a relationship level), and marks the ones available now.
- **Map** shows the card's map with a pin per place. Hover, focus or tap a pin for its travel time, whether it's open and who is there; click it (Go there on a phone) to head there. A pin can open another map, with Back.
- **Text** lets you message a character who isn't in the scene. Each message passes a little time; Done keeps the exchange in the chat as one message, and closing with messages asks before throwing them away.
- **Give** offers the gifts you hold to whoever is in the scene.
- **Trophies** (*2/5*) counts the card's endings and achievements you have found across all its chats. One not found yet has no fill, a dashed edge and a lock, with *Not found yet* (a secret one shows as ???); a new chat with the card is how you try for another ending.
- **Gallery** (*7/20*) shows the card's pictures you have found, large with their captions, and the rest as silhouettes.
- **Notices** for what the world did on its own: a scheduled beat, a relationship level changing (*Mara: Friend → Close*), an event, an ending reached. Several in one turn queue, so each is readable.

Each control shows only when the card uses that feature. On desktop the world tools (Calendar, Give, Map, Text, the card's sound mute, Trophies and Gallery) are icons in the rail at the chat's right edge, with the counts as small badges; hover one for its name. On a phone, or with the card panel open, they sit in a **World** row of their own just above the message box. The first world chat an account opens says where they are, once. Buttons in the reply or the action bar do the rest: travel, give a gift, take an event, wait until morning.

#### Turning it on

The card editor's **Game features** section has one switch per feature, in groups: **World** (clock, places, character schedules, events, map), **People** (relationships, gifts, texting), **Story** (diary, endings, achievements, gallery, outfits) and **Play** (Story and See buttons, Emoji reactions, on-screen stats). Each row says in one sentence what the player gets, and a quiet line says when it needs another feature to work, such as a diary without the in-story clock or a map without places.

**Presets only add.** **Dating sim**, **Life sim** and **Game Mode** each turn on their own features and leave everything else as it is, so they stack (on a world card, Game Mode adds a game master and keeps the world). A preset shows pressed while all of its features are on, and a short note under the row names what it changed. **Clear all**, at the end of the row, turns every feature off, Game Mode included; Story and See buttons and Emoji reactions go to Default, which can still show them, and Always off hides them. A feature turned off keeps its settings in the card, so turning it back on brings them back. The game master, dice and on-screen stats are in Game Mode.

**A first world in five steps:**

1. Press **Dating sim**.
2. In **Places and travel**, rename the example places to your own.
3. In **Relationships**, give the meter a name, such as Trust.
4. In **Dates and events**, write one event: a *Title* and *What happens*.
5. Save the card and start a chat with it.

The ids that conditions and directives use are under **More** in each entry; a new entry takes its id from its name the first time you leave it, and keeps it when you rename it later.

**Forms, with the JSON one click away.** Turning a feature on fills in an example fitted to your card (its character, places, pictures and meter) and opens it as a form. Fields that name something on the card are pick lists: its places, times of day, weekdays, characters, meters, pictures and outfit folders. **Form** and **JSON** at the top of each row's settings switch between the two at any time; the form edits only what you change and keeps any setting it doesn't show. A JSON slip names its line and column, the card keeps the last valid version until it's fixed, and the form waits until the JSON is valid again. **Reset to example** puts working settings back, after asking. A few forms draw what you built:

- The **In-story clock** shows its day midnight to midnight, a colored band per time of day and a tick per scheduled beat.
- Under each character in **Character schedules**, a **Week** grid shows where they are, day by day and hour by hour, as the game will place them.
- Changing a place's id asks whether to update what names it: the starting place, travel, schedules, events, map pins, outfits by place, and the conditions and `world_go` choices in the features and the card's text. **Rename only** leaves those as written. Deleting a place names what still uses it.
- A condition box says in one line why a condition won't read (an unclosed bracket, "and" where `&&` belongs), and suggests what the card defines while you type: meters and levels, gifts, places, the clock, schedules, events, achievements, endings and your own variables.
- **Build** beside a condition box opens it as rows in plain words: a variable, a test (is, is at least, has the item, is set and so on) and a value, picked from a list when the card defines them, joined by **All of these** or **Any of these**. The text box stays the condition, and nothing changes until you edit a row. **Build** beside a beat's or an event's *Then set* does the same for its changes: a variable, set to, add or take away, and a value.
- The **Map** form is its pin editor: pick one of the card's pictures or backgrounds, click to drop a pin, drag it or move it with the arrow keys, and choose its place.

Everything is saved under `metadata.features` and travels with the card in every export. **How these work** at the top of the section opens this page.

Or ask Card Forge for it in plain words ("a life sim with a town map and three endings"). **Ask Card Forge**, beside the presets, opens it on the card you are editing with "Add game features to this card:" already typed; nothing is sent until you finish the request and send it. It writes the features and teaches the AI the directives in the card's greetings and rules. A value it can't use leaves the card's world as it was.

#### The features

Each field's label in the form is in italics, with its JSON key beside it.

- **In-story clock** (`world_clock`): `true` for the defaults (day 1, 08:00, Monday, 10 minutes a reply; morning, afternoon, evening and night), or an object with *Start* (`start`), *Each reply moves the clock* (`per_turn`), *Longest time skip* (`max_jump`, the most one reply may skip), *Time format* (`format`, `"12h"`), your own *Times of day* (`periods`) and *Weekdays* (`weekdays`), and *Scheduled beats* (`beats`): things that happen at a time, once or daily, with an optional condition, variable changes and a notice. The background follows the time of day: with a background named `cafe-night` on the card, `cafe` draws as that at night; without one, a light tint for the period goes over the picture (unchecking *Tint the background by time of day*, `"tint": false`, turns it off, and a time of day's own *Tint color* under More, `tint`, takes a color and a strength, written as a color with alpha such as `#10204080`).

  ```
  "world_clock": { "start": { "time": "07:30", "weekday": "Monday" }, "per_turn": 15,
    "beats": [{ "at": "09:00", "every": "day", "text": "The shops open.", "toast": "The shops are open." }] }
  ```
- **Places and travel** (`world_places`): *Start at* (`start`), and each place has an `id` (under More), a *Name*, a *Description*, optional *Open hours* (`hours`; `"16:00-02:00"` runs past midnight) and *Open on* (`days`), a *Background* (`background`) that shows when you arrive, *When closed* (`closed_text`), and *Travel time to* other places (`travel`, else the row's *Travel time*, `default_travel`). A closed place can still be entered; the AI is told it's closed.

  ```
  "world_places": { "start": "home", "places": [
    { "id": "home", "name": "Home", "travel": { "cafe": 10 } },
    { "id": "cafe", "name": "The café", "hours": "09:00-18:00", "closed_text": "The chairs are up." }] }
  ```
- **Character schedules** (`world_npcs`): where each character is by day and hour, as *Routine* rows of *Days*, *Hours* and *At* (`days`, `hours`, `place`), the first that fits winning, then *Otherwise at* (`default`), else away. Only the characters at your place are in the scene. A group member's own card can carry its routine as `world_routine`.

  ```
  "world_npcs": [{ "name": "Mara", "default": "home", "routine": [
    { "days": ["Saturday", "Sunday"], "hours": "10:00-16:00", "place": "park" },
    { "hours": "09:00-18:00", "place": "cafe" }] }]
  ```
- **Relationships** (`relationships`): up to six named *Meters* (`meters`), each with a range (*Lowest* and *Highest*, `min` and `max`), *Starts at* (`start`), *Most per reply* (`per_turn`, the most the AI may move it in one reply), an *Icon* and *Levels* (`levels`) with names. *Characters* (`characters`) lists who has them; left out, it's the card's own character. *Notice* (`toast`) picks notices on a level change (the default), on every change, or `"off"`.

  ```
  "relationships": { "characters": ["Mara"], "meters": [{ "id": "affection", "max": 100,
    "per_turn": 3, "levels": [{ "at": 20, "label": "Friend" }, { "at": 50, "label": "Close" }] }] }
  ```
- **Gifts** (`gifts`): items from the inventory list (`items`), each with *How each one feels* (`reactions`): who `loves`, `likes`, `dislikes` or `hates` it, else the *Reaction when not set* (`default`); and *Meter changes* (`effects`), the change each reaction makes. Giving takes the item out of the inventory and tells the AI to play the reaction. Something has to fill the inventory first, such as a choice with `| set inventory += rose`; on a Game Mode card, `"inventory": "game"` gives from the sheet's inventory instead. *Getting it* (`get`) fills it for you: pick *How the player gets it* (`verb`: `buy`, `pick_up` or `accept`) and the action bar offers the gift while the player holds none, at the places in *Where to get it* (`where`, only while the place is open) and while someone in *Who has it* (`who`) is in the scene. Leave both empty to offer it anywhere. Its *Choice text* (`label`) is sent as the player's turn; blank, it is the verb and the name, such as Buy rose.

  ```
  "gifts": { "items": [{ "id": "rose", "reactions": { "Mara": "loves" },
    "get": { "verb": "buy", "where": ["florist"], "label": "Buy a red rose" } }], "default": "likes" }
  ```
- **Dates and events** (`world_events`): scenes with a *What happens* (`script`) for the AI, *Offered* (`offer`) as a choice (`"choice"`, the default, with its *Choice text*, `label`) or happening on their own (`"auto"`) when every *When* (`when`) rule holds: place, weekdays, periods, a time window, a day range, a condition. Once by default; unchecking *Can happen only once* (`"once": false`) allows one a day. *Then set* (`set`, under More) changes variables when it happens.

  ```
  "world_events": [{ "id": "sunset", "title": "Sunset walk", "label": "Ask to watch the sunset",
    "when": { "periods": ["evening"], "if": "rel_mara_affection >= 40" },
    "script": "The two of them watch the sun go down.", "minutes": 30 }]
  ```
- **Diary** (`diary`): `true`, or settings for *Written by* (`writer`), *Page title* (`title`) and *Length* (`words`). When a reply crosses midnight, a diary page for the day that ended is added to that reply and, unless `"lore": false`, filed in the chat's lorebook. Needs the clock.
- **Texting between scenes** (`texting`): `true` for the chat's characters, or a *Contacts* list (`contacts`), with *Minutes per message* (`minutes_per_message`) and *Messages per conversation* (`max_messages`). Someone in the scene can't be texted.
- **Map** (`world_map`): pictures from the card's own images, with pins at percent positions that point to a place or to another map. Easiest to build in its form, the pin editor.
- **Endings** (`endings`): a *Deadline* (`deadline`) day and time, a list of *Endings* (`list`) with an *Only if* condition each (`if`; the first that holds wins), *If none holds* (`fallback`), and *Ends the story as soon as its condition holds* (`"early": true`). The AI is asked to write the epilogue.

  ```
  "endings": { "deadline": { "day": 7, "time": "21:00" }, "fallback": "goodbye", "list": [
    { "id": "together", "title": "Together", "if": "rel_mara_affection >= 60", "text": "Mara asks {{user}} to stay." },
    { "id": "goodbye", "title": "Goodbye at the station", "text": "{{user}} leaves on the last train." }] }
  ```
- **Achievements** (`achievements`): up to 50, each with an `id`, a `title` and an optional `description`. The AI unlocks one with `[[achievement: first_kiss]]`, a choice with `| set world_achieve = first_kiss`, and an *Unlocks when* condition (`if`) unlocks it by itself once it holds. *Hidden until unlocked* (`"hidden": true`) lists it as ??? until found, and the AI is told only its *Hint* (`hint`). They count on the trophy beside the endings; one found stays found in every chat with the card, and the chat that found it has `world_ach_first_kiss` for conditions.

  ```
  "achievements": [{ "id": "first_kiss", "title": "First Kiss", "description": "Share a kiss with Mara." },
    { "id": "night_owl", "title": "Night Owl", "description": "Still awake at 3 AM.", "hidden": true }]
  ```
- **Unlockable gallery** (`gallery`): the card's **Pictures** become a collection. Each unlocks the first time a reply shows it, and the gallery button (*2/5*) lists them, locked ones as ???. A picture found stays found in every chat with the card. `true`, or *Pictures unlock* at *All from the start* (`{ "unlock": "all" }`) to open them all from the start; *Pictures* (`names`) picks which pictures it holds, in order. An ending's or achievement's `image` unlocks with it.
- **Outfits** (`outfits`): which set of the card's expression sprites the character wears, for sprites named in sets such as `swimsuit/joy` (a feeling missing from the set falls back to the plain sprite). The AI changes it with `[[outfit: swimsuit]]` (`[[outfit: default]]` goes back), a choice with `| set world_outfit = swimsuit`, and *At a place* (`by_place`) or *At a time of day* (`by_period`) put one on when you arrive somewhere or the time of day turns; *Usual outfit* (`default`) is worn otherwise.

  ```
  "outfits": { "default": "casual", "by_place": { "beach": "swimsuit" }, "by_period": { "night": "pajamas" } }
  ```
- **Emoji reactions** (`reactions`): one emoji you put on the newest reply, which the character is told about on its next turn (see Reactions). Its row under Play has Default (follow the Chat Display setting), On and Always off.
- **Story and See buttons** (`composer_modes`): `true` shows both in any chat, `false` hides them even in Game Mode, or `{ "modes": ["see"], "see_text": "*Glances around.*" }` for one of them and the line an empty See sends (*See with nothing typed*). Its row in Game features, under Play, has Default (Game Mode and visual novel chats only), On and Always off.

#### What the AI writes

The AI moves the world with directives, each on a line of its own, which the app applies and never shows. The app teaches it the ones the card's features use:

```
[[time: +2h]]              also +90m, +1d, next morning, next day, 07:00
[[rel: Mara affection +2 | she liked the joke]]
[[go: cafe]]
```

- Without `[[time:]]`, the first reply to each of your messages moves the clock by `per_turn`; a Continue or the next group member's reply adds nothing. One reply can't skip more than `max_jump`. `[[time:]]` and `[[go:]]` also count at the end of a line of story.
- `[[rel:]]` needs a sign on its number and is capped at the meter's `per_turn`. The name can be left out on a single-character card.
- A `[[background:]]` that names a place moves you there too, and travel adds its minutes to the clock.
- The AI can't set the world's variables itself: a reply's `[[set:]]` of a `world_` or `rel_` variable is ignored. Your clicks can.

#### Buttons that ask the world for something

A choice makes a request by setting one of these, and the next reply carries it out:

```
[[choice: Sleep until morning | set world_wait = next morning]]
[[choice: Wait here | set world_wait = +1h]]
[[choice: Walk to the café | set world_go = cafe]]
```

`world_wait` takes the `[[time:]]` forms (`0` means no time passes) and wins over the reply's own; `world_go` a place; `world_event` an event id; `world_gift` an item and a person, `rose:Mara`. The app also builds four menus to put in a reply or the card's action bar: `[[menu: world-travel]]` (every other place with its travel time), `[[menu: world-gifts]]` (each held item for each person), `[[menu: world-get]]` (each gift the player can get here and now) and `[[menu: world-events]]` (the events available now). Travel, events and getting gifts join the action bar by themselves; write the menu line in the bar only to choose where they sit. A `world_wait` set while another is still pending adds to it (texting, then Wait here, moves the clock by both); a set time such as `next morning` replaces it.

#### Gating content on the world

The world is plain chat variables, so the conditions you already use read it: `world_day`, `world_time`, `world_period`, `world_weekday`, `world_place` (a place id), `world_here` (who is at your place), `world_ending`, and `rel_mara_affection` with its level in `rel_mara_affection_level` (names are lowercased, spaces become `_`).

```
[[choice: Tell Mara about the letter | if rel_mara_affection_level == Close | hide]]
[[choice: Ask her to dance | if rel_mara_affection >= 40 && world_place == pier && world_period == evening]]
[[choice: Wave Mara over | if world_here has Mara]]
{{if world_period == night}}The street is empty.{{else}}The street is busy.{{/if}}
```

- **Lorebook entries** take the same `{{if}}` condition, without the braces, in **Only while this condition holds** under the entry's Scope: `rel_mara_affection >= 40 && world_here == cafe`. Constant plus a condition is lore that appears only in that state; keywords plus a condition need both.
- **Numbers for thresholds, levels for names.** `>=` on a level label compares alphabetically, not by rank. Below the first level there is no label, so `== Friend` is false.
- **Two dialects.** A choice's `== night` ignores case and has `has` for a list item; in `{{if}}` and a lorebook condition `==` matches case exactly, and there is no `has`: use `contains`, which matches any part of the text. In `{{if}}` a plain name reads the variable unless a macro has that name (`char`, `user`, …); write `.world_place` to be sure.
- **Greetings rewrite themselves.** A greeting's macros are read against the current variables, so one gated on the time or a meter changes as they do. Use alternate greetings instead.

> **Tip:** Build the story from what the player does: let choices set `world_go` and `world_wait`, and gate the scenes that matter on meters and places, rather than asking the AI to remember where everyone is.

## Document styles

A document style decides how a `[[doc: name]]` block looks in a reply: a letter, a phone screen, a game's system message, a loot box. Thirty-one come built in: `paper`, `newspaper`, `terminal` and `sms`, plus twenty-seven that also tell the AI when they fit: `diary-page`, `email-message`, `social-thread`, `classified-file`, `comm-link`, `tarot-card`, `stream-chat`, `phone-messages`, `departure-board`, `radar-sweep`, `neon-sign`, `glitch-broadcast`, `spellbook`, `matrix-terminal`, `rpg-dialog`, `vinyl-record`, `data-cube`, `ransom-note`, `potion-vial`, `aurora-sky`, `stock-ticker`, `heartbeat-monitor`, `cursed-scroll`, `sealed-letter`, `ouija-board` and `boss-encounter`. You can make your own from a little HTML and CSS. The look is drawn by the app; the AI only writes the block and its text.

#### Making one

- **Yours, for every chat**: the Library's Document styles tab. Each style shows as a tile with its live sample where a card's picture would be; **Edit** opens it, and the folder sidebar sorts them the same way as shared lorebooks (drag a tile onto a folder, or use the folder button on its corner). Built-in styles are listed under **All styles**, grouped by kind; **View** opens one read-only, to see how it's made, and **Duplicate** makes it yours. A folder only sorts the page: a style works the same wherever it is filed.
- **A card's own**: the card editor's **Document styles** section. These travel with the card when it's exported or shared and work in its chats, with nothing for the player to add.
- Pick a built-in and press **Duplicate** to start from its real template, or **New**. Each style has a **Name** (up to 30 lowercase letters, numbers and dashes: it's what you write in the block), its **HTML**, an optional **Line HTML** for message threads, and its **CSS**. **Tell the AI when to use it** is an optional line the AI reads beside the name (see below). **How to use it** shows the block to copy.
- **Add a starter** adds a copy of any style to start from: yours, by folder, the built-ins, and eight ready-made starters. Type to search by name or by its use line. The starters are a notice board, a letter, a status window in your theme's colors, a quest log that changes color for `New:`, `Complete:` and `Failed:` titles, a chat log, an animated alert, an achievement banner (a gold, silver or bronze medal from its `Tier:` field, and a `Reward:` box that hides when empty) and an item card colored by its `Rarity:`, common to legendary, with a `Type:` label. Each is an ordinary copy you can edit, with its use line already written. To see a technique in a finished style, add a starter and read its CSS.
- **Moving a style**: in a card's Document styles, **Add one of yours** copies one of your own styles into the card, so everyone you share it with sees it styled. **Export** saves the style you're editing as a `.json` file, and **Import** adds one, on the Document styles tab or in a card. A name that's already taken gets `-copy`.
- **Linking a style**: in a card's Document styles, **Link from library** adds one of your styles, or a built-in, that stays in step with it: edit yours on the Document styles tab and the card follows, in its chats and in every export. Linking is also how a card's AI learns a built-in (see Getting the AI to use them, below). A linked style shows **Linked** and is edited in your library; **Unlink** makes it the card's own copy again. If the style is deleted, or the card is opened on another computer, the card keeps its last copy and offers **Relink**. Anyone you share the card with gets an ordinary copy. **Save to library** does the reverse: it saves a copy of one of the card's styles to yours, under a `-copy` name if that one is taken.
- **Preview** draws the style with a sample title and text you can type over (only for the preview, never saved), in **Light** or **Dark** and at **Phone width**. Under the CSS, a short list names anything the chat will skip, such as nested rules, `@layer` or a selector that reaches outside the block. Remove asks first, and two styles with the same name get a warning, since only the first one is ever used.
- **Card Forge** can build one with you: choose **Document style** when it asks what you're making. Or type a description into **Design one with AI** at the top of the Document styles tab: it opens a Card Forge session on that, the guide writes the style with your model, and you keep asking for changes in the same thread. The style lands in your list.
- **Same name, which wins**: yours, then the card's, then the built-in. So a style of yours called `system` replaces a card's `system` everywhere. If you make styles for a card you'll share, give them names nobody else is likely to use.

#### Writing the block

```
[[doc: notice | Wanted]]
**Black Tom Harrow**, for horse theft.

Reward: 50 silver.
[[/doc]]
```

- The opening and closing lines each go on a line of their own. The title, after the `|`, is optional.
- Markdown works inside. One line break stays a line break in the same paragraph; a blank line starts a new paragraph.
- A name no style has shows as a plain box, and a name with spaces or other characters shows as `paper`.

#### HTML and CSS

The HTML says where things go: {{title}} for the title, {{content}} for the text as markdown, {{raw}} for the text exactly as written, and any chat macro such as {{char}}. The CSS only ever reaches this style's blocks: `:scope` is the block itself, and any other selector styles something inside it. A selector that steps from the block to the text beside it, such as `:scope ~ p`, is dropped. The style behind the example above:

```
HTML:
<div class="notice-head">{{title}}</div>
<div class="notice-body">{{content}}</div>

CSS:
:scope {
  background: #f4ead2;
  color: #2b2116;
  border: 2px solid #6b4f2a;
  font-family: Georgia, serif;
}
.notice-head {
  text-align: center;
  text-transform: uppercase;
  font-weight: 700;
  border-bottom: 1px solid #6b4f2a;
}
.notice-head:empty { display: none; }
```

- **Light and dark.** A style can't tell which theme you're using. Either paint your own background *and* text color, as above, so it looks the same in both, or use the theme's colors (`var(--surface)`, `var(--foreground)`, `var(--border)`) so it follows the theme. Don't mix the two.
- **Not allowed**, and skipped: images and anything else with `url()`, `@import` and web fonts. `position: fixed` works as `absolute`. Scripts, forms, buttons and SVG are removed from the HTML. Use gradients, emoji or symbols instead of pictures.
- **One block, different looks**: the title and fields (below) are what a block can pass to its style. Copy one into an attribute in the HTML, `<div data-tier="{{title}}">`, and match it in the CSS, `[data-tier^="gold" i]`, so `[[doc: loot | Gold Box]]` draws in gold.
- **Animations** work. They hold still while a reply is still arriving and play once it is finished, and run only in the newest message: once a newer one arrives, older blocks stop and show their resting look. So a loop (`infinite`) is fine, as long as the block also reads well standing still. End the CSS with `@media (prefers-reduced-motion: reduce) { *, *::before, *::after { animation: none !important; } }` for players who turn motion off.

#### Fields

A `{{field:reward}}` in the HTML gives the style a field of its own. The block fills it from a `Reward: …` line at its top, and that line is taken out of the text, so a wanted poster can print its bounty in its own box:

```
HTML:
<div class="notice-head">{{title}}</div>
<div class="notice-body">{{content}}</div>
<div class="notice-reward">Reward: {{field:reward}}</div>

Block:
[[doc: notice | Wanted]]
Reward: 50 silver
**Black Tom Harrow**, for horse theft.
[[/doc]]
```

- Field lines go first in the block, one per field, in any case, bold or not (`**Reward:** 50 silver`). The first line that isn't a field ends them, so a `Reward:` further down stays in the text.
- Names match the way you'd write them: `{{field:due_date}}` is filled by `Due date: Friday`, since spaces, dashes and underscores count as the same.
- A field the block leaves out, or leaves empty, shows its default if it has one, written after a bar: `{{field:reward|Unknown}}`. Otherwise it's empty. A field is plain text and safe in an attribute: `data-tier="{{field:tier}}"`.
- The AI is told each style's field names, so a use line such as "for wanted posters; Reward: the bounty" is usually all it needs.

#### Getting the AI to use them

When a card's own text has a `[[doc:]]` block in it, or the card carries its own styles, the app tells the model the syntax and the styles **in or linked to the card**, with each style's **Tell the AI when to use it** line and its field names beside it. A style the card's own text writes in a `[[doc: name]]` block counts too, so a greeting that uses `newspaper` gets `newspaper`. The other built-ins and the rest of your library are not offered, so to give a card's AI a built-in, open the card's Document styles, press **Link from library** and pick it under **Built-in**: it shows as **Built-in**, linked, follows the app's version and exports as an ordinary copy. Any built-in still draws when a reply uses it. The note is sent every turn, around a hundred tokens plus the use lines (it shows as Interactive elements in the context meter). The model never sees what a style looks like, so for a card's own styles, back the use lines with:

- **A behavior rule**, such as: *"System text is always a document block: [[doc: system]] for announcements and refusals; [[doc: achievement | <name>]] with an italic taunt, a blank line, then Reward: <box>. [[stat:]], [[roll:]] and [[choice:]] lines go after the [[/doc]]."*
- **One example per style** in a greeting or an example dialogue, written exactly as the AI should write it. Models copy examples far better than they follow rules.

```
[[doc: achievement | Tire Iron Chef]]
*You beat a raccoon to death with car parts. He had a family. Mostly raccoons.*

Reward: Bronze Adventurer Box!
[[/doc]]
```

Want one of your own styles with somebody else's card? The card needs a `[[doc:]]` block somewhere in its own text first, or the AI is never told about it.

#### Things to watch for

- **Choices go after the block.** A `[[choice:]]`, ask, pick or form line inside a block ends it right there: the buttons work, but the rest of the block shows as ordinary text and its closing line shows as written.
- **A block the AI forgot to close** is closed for it, before its choices or at the end of the reply.
- **Sharing**: a card's own styles go with it in every export, SillyTavern files included (SillyTavern itself shows the blocks as plain text). Your own styles travel in a bundle, under **Document styles these cards use**, or as a file through **Export**; to make one part of a card, use **Add one of yours**.
- **Old messages** redraw with a style's current look, so changing a style changes every block that uses it.

## Chat styles

A character card can bring its own chat style: a look for its chat window, from the message bubbles, names and avatars to the choices, dice rolls, the Game Mode HUD, the message box and every color. It applies only while that card's chat is open, and only inside the chat window, plus the colors of the chat's top bar (never its size, fonts or buttons): the sidebars, Settings, menus and pop-ups always keep the app's look. A chat style can't load anything from the web or run any code, and the AI never sees it, so it costs no tokens.

Together with document styles and Game Mode, it lets a card feel like its own game or story: a candlelit letter, a starship console, a retro RPG.

#### Turning them off

- **Every card**: Settings → Chat Display → Card chat styles. On by default.
- **One card**: hover the **Session info** button at the top of its chat and choose **Turn off this card's chat style** (the same line turns it back on). On a phone, open **Chat options** (the **…** at the top of the chat) and flip **Card chat style**. It stays off for that card in every chat, and nothing in the card changes.
- Both switches sit outside the chat window, in a menu that keeps the app's look, so a style can never hide them.
- Importing a card or bundle tells you which characters bring a chat style.

#### Group chats

In a group, the window (background, message list and message box) takes the style of the first member in the group that has one. Every other member with a chat style styles its own messages, so each character keeps its look. Your own messages follow the window. Each member's style can be turned off on its own from the Session info menu.

To choose, open **Group chat** (the people button at the top of the chat). **Window style from** picks whose style dresses the window: **Auto** (the first member with one), a member, or **None**. Turn off **Members style their own messages** to keep only the window's style. Both show only while a member has a chat style.

#### Making one

- **In the card editor**: expand **Chat style** and write CSS. **Parts you can style** lists every piece of the chat you can target (click one to copy it), the list under the box names anything the chat will skip, and the preview shows your style on a sample chat in **Light**, **Dark** and **Phone** width. **Import** and **Export** move it as a `.css` file.
- **With the Card Forge**: open the card, open **Chat style** in the sidebar and press **Write a chat style**, or just ask your guide ("give her chat a candlelit letter look"). It shows the proposal on a sample chat before you accept; if the chat would skip part of it, **Fix the chat style** asks for a rewrite. Undo it from **Card history**.
- **From a starter**: **Start from a starter** in the editor's Chat style section (or the Card Forge sidebar) offers eight finished looks, from a fantasy tavern to a starship console and a visual novel, each drawn with your own character. Pick one with **Use** and change it as you like; you're asked first if the card already has a style.

The style travels with the card: exports, PNGs, SillyTavern files (which ignore it), bundles and uploads all keep it.

#### Writing the CSS

`:scope` is the chat window itself; any other selector styles something inside it. Style the parts, the classes that start with `rc-`, such as `.rc-bubble`, `.rc-name`, `.rc-choice` or `.rc-composer`: they stay the same across updates, unlike the chat's other class names. The quickest strong look is to redefine the app's color tokens on `:scope`:

```
:scope {
  --bg: #1b1410; --bg-alt: #140f0b;
  --surface: #2a201a; --reply-bg: #2a201a;
  --border: #4a3a2c; --foreground: #f1e6d4; --muted: #b9a68c;
  --purple: #b5832f; --purple-dark: #5a3d16; --on-purple: #1b1410;
  font-family: Georgia, "Times New Roman", serif;
}
.rc-name { font-variant: small-caps; color: #e0b261; }
.rc-message.rc-char .rc-bubble { border-radius: 4px 16px 16px 16px; }
.rc-choice { border-radius: 999px; border-color: #b5832f; }
```

- **Skipped**: anything with `url()`, `@import`, web fonts, CSS nesting and `@layer`. On `:scope` itself only colors, fonts and text, backgrounds, padding, borders and custom properties work, so nothing can move the window or paint outside it.
- **The card's own pictures and fonts**: each background and picture stored in the card is a variable named after it, `background-image: var(--rc-img-forest)`. Add font files under **Fonts** in the Chat style section (up to 8, 3 MB in all) and write `font-family: var(--rc-font-name, Georgia), Georgia, serif`. Card fonts load on Windows and Android only, so keep the fallback inside the `var()`. Both are listed under **Parts you can style**; click one to copy it. They travel in a bundle, not in a PNG or JSON card file.
- **The story can change the look**: every chat variable the story sets is on the window, a number as `var(--rc-var-gold)` and a word as `:scope[data-rc-scene="night"]`. In Game Mode, `:scope[data-rc-combat]` is on during a fight and `:scope[data-rc-hp]` is `ok`, `hurt`, `low` or `down`. **Try a state** above the preview shows it.
- **One message, its own look**: write rules for `.rc-message[data-rc-look="whisper"]` and the character can mark a reply `[[look: whisper]]` to wear it. The chat tells the character which looks your style has, and the mark never shows. You can mark your own messages too.
- **Light and dark**: either leave the theme's colors alone, or redefine the whole set (grounds, reply bubble, text, borders and accents) so the chat reads the same in both. Check both in the preview.
- **Motion**: keep it short, only on the newest reply (`.rc-newest`), and inside `@media (prefers-reduced-motion: no-preference)`, so readers who turned motion off get none.
- **Document blocks** inside replies keep their own document style: leave `.rc-doc` to it, and use one palette for both.

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

## Macros

Macros are `{{...}}` tokens expanded when text is sent. Type `{{` — in the message box, a card field or a lorebook entry — for a list of macros to pick from, or press `Ctrl`+`Space` to open it anywhere. Macro names are case-insensitive, and macros resolve innermost-first — `{{getvar::{{.which}}}}` works. Every macro is listed, with an example, in the Macro reference.

> **Tip:** **Where a macro works.** Almost everywhere: card fields, greetings, messages you type, quick replies, lorebook entries, the Author's Note. That includes the **character and chat** macros (`{{description}}`, `{{lastMessage}}`…). Only `{{system}}`, `{{wiBefore}}` and `{{wiAfter}}` are limited to the context template.

#### Names

- `{{char}}`: the character's name. `{{user}}` — your persona's name. `{{persona}}` — your persona's description.
- Old-style `<USER>`, `<BOT>` and `<CHAR>` tags from imported cards are converted automatically.
- `{{group}}`: everyone in a group chat (the character alone otherwise). `{{notChar}}` — everyone but the character. `{{charIfNotGroup}}` — the character's name, or nothing in a group.

#### The chat and its settings

- `{{summary}}` (Chat Memory's summary), `{{authorsNote}}`, `{{defaultAuthorsNote}}`, `{{charAuthorsNote}}` (the card's Character's Note, same as `{{charDepthPrompt}}`), `{{systemPrompt}}` (the one in use), `{{defaultSystemPrompt}}`. `{{original}}` puts in the prompt config's text the card field replaces: in a card's system prompt, the config's main system prompt; in its post-history instructions, the config's text sent after the chat (which is then sent there only, not a second time). Anywhere else it is empty.
- `{{model}}`, `{{maxContextTokens}}`, `{{maxResponseTokens}}`, `{{maxPrompt}}` (context minus response), `{{lastGenerationType}}` (normal, regenerate, continue, impersonate).
- `{{currentSwipeId}}` / `{{lastSwipeId}}` (which swipe of the last message, and how many), `{{firstIncludedMessageId}}` (the oldest message that fit in the last prompt), `{{idleDuration}}` (how long since your last message), `{{input}}` (what's typed in the box).
- `{{charPrefix}}` / `{{charNegativePrefix}}` — the card's image prompt prefixes.

#### Variables

Two stores: **chat variables** (`.`, saved with this chat) and **global variables** (`$`). Global variables are kept per chat too by default, so a new chat starts without any. To share them across every chat the way SillyTavern does, set **Scope of global variables** in Settings → Macros & Regex. A shared value is written once, when its message first appears, and a swipe, regenerate or rewind never undoes it.

- **Read:** `{{.hp}}`, `{{$coins}}`, or `{{getvar::hp}}` / `{{getglobalvar::coins}}`. Names may contain dots and hyphens: `{{.guard.hair}}`.
- **Set:** `{{.hp = 10}}` or `{{setvar::hp::10}}`. `{{.hp ??= 10}}` sets it only if it doesn't exist yet, and `{{.name ||= Stranger}}` also fills in an empty value — both print the value the variable ends up with. To set up a game silently, use `=` or `{{setvar}}`.
- **Maths:** `{{.hp += 5}}`, `{{.hp -= 5}}`, `{{.hp++}}`, `{{.hp--}}`, or `{{addvar::hp::5}}` / `{{incvar::hp}}` / `{{decvar::hp}}`. `+=` joins text when either side isn't a number; the others leave a text value alone.
- **Calculate:** for anything beyond a single step, `{{calc: hp + armor * 2}}` evaluates a whole expression. It takes `+ - * / %`, `**` (power), parentheses, variable names (`hp`, `.hp` for this chat only, `$coins` for a global; a missing one counts as 0), and `floor`, `ceil`, `round`, `abs`, `sqrt`, `min`, `max`, `clamp(x, low, high)`, and `randint(low, high)` / `rand(low, high)` for random numbers — e.g. `{{calc: min(hp, hp_max)}}` or `{{calc: 10 + randint(1, 6)}}`. Write `{{calc: hp = hp + armor}}` (or `:=`; `$coins = …` for a global) to store the result in a variable and print nothing — the tidy way to update a stat. A malformed expression prints nothing.
- **Show text for a value:** `{{switch: mood | 0-3=Tense | 4-6=Calm | *=Content}}` picks the first case that matches (an exact value, a numeric `low-high` range, or `*` as the fallback) — one line instead of a stack of `{{if}}`s.
- **Meters & pips:** `{{repeat: ♥ | hp}}` prints that many copies, and `{{bar: hp / hp_max | 10}}` draws a 10-wide filled/empty bar (`██████░░░░`); add `| ♥ ♡` to pick the characters.
- **Lists** (an inventory, clues found, party members) are a variable holding comma-separated items — `rope, torch`. `{{.inv push rope}}` adds one (or several: `push rope, torch`), `{{.inv remove rope}}` takes one out, and neither prints anything. `{{count: inv}}` is how many there are, `{{.inv contains rope}}` checks for one, and `{{each: inv | - {{item}}}}` writes a line per item (`{{index}}` is its number) — you can put an `{{if {{item}} == rope}}` inside. Items can't contain commas.
- **Fallbacks:** `{{.title ?? none}}` (only when unset), `{{.title || none}}` (also when empty).
- **Either/or:** `{{.hp > 0 ? alive : dead}}` — any condition, then `?` and `:` with spaces around them (`: dead` is optional). Only the chosen side runs, so a roll or `++` on the other side never happens.
- **Compare:** `{{.hp > 3}}`, `{{.flag == true}}`, `{{.inventory contains rope}}`, `{{.hp == .hp_max}}` (a `.name` or `$name` on the right reads that variable) — these print `true` or `false`, so they're mostly used inside `{{if}}`. Against a number, an unset variable counts as 0.
- **Also:** `{{hasvar::hp}}`, `{{deletevar::hp}}`, and a `…globalvar` twin of every function.

**Seeing them**

Variables are invisible until something prints them, which makes a card that tracks state by hand hard to follow. The **Variables** button on the right edge of a chat opens a panel listing everything this chat is keeping, with its value. You can change a value, rename one, add one and delete one from there.

- An edit here **wins**: it outranks what the transcript would put back, so deleting or regenerating an older message can't quietly undo it.
- A locked choice re-checks its condition straight away, so setting `has_key` here unlocks the button without waiting for the next reply.
- Global variables are listed separately. When they are shared, changing one changes it in every chat.
- Game Mode stats aren't here. They live on the character sheet, behind the dice button.

> **Tip:** What prints matches SillyTavern, so imported cards read the same. **Nothing:** `=`, `+=`, `-=`, `{{setvar}}`, `{{addvar}}`, `{{deletevar}}`. **The value:** `++`, `--`, `??=`, `||=`, `{{incvar}}`, `{{decvar}}`. To set a value and show it, write both: `{{.hp = 10}}{{.hp}}`. A variable's value always prints as written — macros stored inside it don't run.

#### Your own macros

Define **custom macros** in Settings → Macros & Regex: a name and the text it expands to, usable anywhere macros work — card fields, prompts, greetings, messages. Names are case-insensitive, and a built-in name can't be overridden ({{char}} stays the character no matter what you call a macro).

- **Values can contain other macros**, which expand too — including your other custom macros. So `{{sig}}` → `the Crimson Vow of {{char}}` prints the character's name inside your text, and macros can build on each other: `{{mood}}` → `{{random::wistful::furious::giddy}}`, then `{{scene}}` → `{{char}} is feeling {{mood}} today.`
- Use them for anything you keep retyping: a formatting reminder you drop into Author's Notes, a boilerplate scene-setter, a random-flavor table, a nickname that should follow `{{user}}` everywhere.

> **Tip:** The **macro playground** sits right under the editor in Settings → Macros & Regex and resolves your macros live as you type, against sample `{{char}}`, `{{user}}` and `{{persona}}` values — the fastest way to check a `{{roll}}` or a conditional before putting it in a card.

**Macro mistakes are pointed out for you** — in the playground as you type, above a card field or lorebook entry when you click away from it, and in the card's health check. It catches an `{{if}}` with no `{{/if}}` (or the reverse), a stray `{{else}}`, `=` where `==` was meant, a macro name that doesn't exist, `{{trim}}` with text inside it, and a `{{` that never closes.

#### Conditionals

`{{if EXPR}}…{{elseif EXPR}}…{{else}}…{{/if}}`. The condition is written *without* braces: `{{if .flag}}`, `{{if .hp < 3}}`, `{{if hasvar::met}}`. Chain as many `{{elseif}}` branches as you like (the first true one wins), combine with `&&`, `||`, `!` (not) and parentheses — `{{if .ready && (.hunger > 0 || .thirst > 0)}}` — and nest freely. Either side of a comparison may be a macro in braces: `{{if .name == {{user}}}}`. A plain name that isn't a macro reads the variable of that name when it's set, so `{{if rel_mara_affection >= 40}}` works like `{{if .rel_mara_affection >= 40}}`; a macro of the same name (`char`, `user`, `description`, …) always wins. A condition is true when it expands to something that isn't empty, `0`, or `false`, `off` or `no` (any case). `{{if::.flag}}` works too. `{{endif}}` works as `{{/if}}`. As in SillyTavern, what's inside a block is trimmed and un-indented, so you can lay it out on its own lines; write `{{#if …}}` to keep its spacing exactly. Any macro that takes an argument can be a block too — `{{setvar backstory}}`…`{{/setvar}}` stores everything between as the value. For mapping one value to text, `{{switch}}` (see Variables) is usually shorter than a chain.

#### Randomness

- `{{random::a::b::c}}`: re-rolls each time it's evaluated.
- `{{weighted::common:5::rare:1}}`: like random, but `text:N` is N times as likely (no number = 1, `:0` = never). Good for loot and encounter tables.
- `{{pick::a::b::c}}`: *stable*: the same list of options gives the same answer for the whole chat, and a different answer in a different chat. In a lorebook entry each entry gets its own draw.
- `{{roll::2d6+1}}`, `{{roll::d20}}`, `{{roll::1d6+1d4}}` — dice, added or subtracted. A bare number rolls one die of that size (`{{roll::20}}` is a d20). It reads the same dice the Game Mode roll button does: `{{roll::4dF}}` for Fate dice, each face -1, 0 or +1, and `{{roll::3d6kh1}}` to keep the highest of three (`kl` keeps the lowest). A `!` after a die explodes it, `{{roll::1d6!}}`: a top face rolls again and adds. Anything it cannot read returns `0`.

#### Date & time

- `{{date}}` / `{{time}}` — your system's local format.
- `{{isodate}}` → `2026-08-18`, `{{isotime}}` → `21:04` (24-hour, no seconds), `{{weekday}}` → `Tuesday`.
- `{{datetimeformat::YYYY-MM-DD HH:mm:ss}}`: only those six tokens are substituted; everything else is literal.
- `{{time::UTC+2}}`: the time at that UTC offset. `{{timeDiff::a::b}}` — how far apart two dates are, in words (`2 hours`).

#### Text utilities

- `{{newline}}`, `{{newline::3}}`, `{{space}}`, `{{space::4}}`.
- `{{trim}}`: prints nothing and swallows the line breaks either side of it. Use it to close up the gap an unused conditional leaves behind. As a block, `{{trim}}`…`{{/trim}}` keeps what's inside and trims its edges. Don't put text inside a single `{{trim …}}`: as in SillyTavern, that text is dropped.
- `{{silent}}`…`{{/silent}}` — everything inside still runs (variables are set, dice are rolled) but none of it is shown. Handy for debugging with the output visible, then wrapping the section once it works.
- `{{reverse::text}}`, `{{noop}}` (nothing at all), `{{// a note to yourself}}` (a comment, removed before sending — for several lines, `{{ // }}` … `{{ /// }}`).
- `\{\{char\}\}` prints `{{char}}` as written, without running it.
- SillyTavern's space form works for single-argument macros: `{{getvar hp}}`, `{{roll 1d20}}`, `{{reverse text}}`.
- `{{banned::word}}` is accepted for card compatibility but does nothing here.

#### Character & chat (prompt-building places only)

- **Card fields:** `{{generalDescription}}`, `{{appearance}}`, `{{corePersonality}}`, `{{behaviorRules}}`, `{{speechStyle}}`, `{{backgroundHistory}}`, `{{worldSetting}}`, `{{dialogueExamples}}`, `{{charVersion}}`, `{{charFirstMessage::0}}` (the N-th greeting). The list fields take a number for one item — `{{behaviorRules::0}}`, `{{speechPatterns::1}}`, `{{dialogueExamples::2}}` — counted from 0, the same as `{{charFirstMessage}}` and SillyTavern. There is no macro for a card's creator notes — that field is the publishable blurb for whoever downloads the card, so it never goes to the model.
- **Chat:** `{{lastMessage}}`, `{{lastUserMessage}}`, `{{lastCharMessage}}`, `{{lastMessageId}}`.

> **Tip:** Rin keeps card text in structured fields, but the old flat SillyTavern names still work: `{{description}}`, `{{personality}}`, `{{scenario}}` and `{{mesExamples}}` each read the merged text an export to SillyTavern would carry, so an imported card's own prompts keep working. Reach for the individual field macros above when you want one part of it rather than the lot.

#### Context template only

Inside a custom context template in Settings → System Prompts you also get `{{system}}` (the active system prompt), `{{wiBefore}}` and `{{wiAfter}}` (lore placed before/after the character), and `{{.messageCount}}` (how many messages so far — handy for dropping example dialogue once a chat gets going). `{{persona}}` is deliberately blank there: your persona is always sent as its own separate block.

#### Routing

A `<!--TOKEN-->` anywhere in evaluated text is removed and used to force-activate the world-info entry whose keyword or title is `TOKEN`. It works from a variable's value too, so a variable can steer which lore appears.

> **Tip:** To show a macro *literally* — in a card's creator notes, or when you want the model to see the token itself — wrap it in `[code]…[/code]`. Nothing inside is evaluated.

```
A greeting that varies and sets up state:
        "{{pick::Morning::Evening}}, {{user}}." {{char}} {{random::smiles::frowns}}.
        {{.trust ??= 0}}{{.met ??= false}}

        Later, in a lorebook entry:
        {{if .trust > 3}}{{char}} speaks freely around {{user}} now.{{else}}{{char}} is still guarded.{{/if}}

        A die roll with a comment:
        {{// combat check}}{{char}} rolls {{roll::2d6+2}} against the lock.
```

## Macro reference

Every macro, by what it's for. Names work in any capitals. Type `{{` (or press `Ctrl`+`Space`) in the message box, a card field or a lorebook entry to pick one from a list.

#### Names

| Macro | What it does | Example |
| --- | --- | --- |
| `{{char}}` | The character's name. | `Hi, {{char}}! → Hi, Rin!` |
| `{{user}}` | Your persona's name. | `{{user}} waves. → Alex waves.` |
| `{{persona}}` | Your persona's description. | `{{persona}} → a traveling merchant` |
| `{{group}}` | Everyone in a group chat (the character alone in a one-on-one). | `{{group}} → Rin, Ada` |
| `{{groupNotMuted}}` | Everyone in a group chat except muted members (mute from the speaker icon on their chip). | `{{groupNotMuted}} → Rin` |
| `{{notChar}}` | Everyone in the chat except the character. | `{{notChar}} → Alex, Ada` |
| `{{charIfNotGroup}}` | Same as {{group}} — as in SillyTavern, where it's another name for it. | `{{charIfNotGroup}} → Rin` |
| `<USER> <BOT> <CHAR>` | Old-style tags from imported cards, converted to {{user}} / {{char}}. | `<BOT> smiles. → Rin smiles.` |

#### The card

The list fields take a number for one item, counted from 0 (as in SillyTavern).

| Macro | What it does | Example |
| --- | --- | --- |
| `{{description}} / {{charDescription}}` | The description as SillyTavern exports it (General description + Appearance). | `{{description}} → A stoic knight…` |
| `{{personality}} / {{charPersonality}}` | Personality as SillyTavern exports it (core + behavior rules + speech style). | `{{personality}} → Loyal and terse…` |
| `{{scenario}} / {{charScenario}}` | Scenario as SillyTavern exports it (World / setting + Background). | `{{scenario}} → The keep at dawn…` |
| `{{mesExamples}} / {{mesExamplesRaw}}` | The example dialogue. | `{{mesExamples}} → {{user}}: Hi…` |
| `{{generalDescription}}` | The General description field. | `{{generalDescription}} → A stoic knight.` |
| `{{appearance}}` | The Appearance field. | `{{appearance}} → Tall, scarred…` |
| `{{corePersonality}}` | The Core personality field. | `{{corePersonality}} → Loyal.` |
| `{{behaviorRules}} / {{behaviorRules::N}}` | All behavior rules as a list — or just rule N. | `{{behaviorRules::0}} → Never lies.` |
| `{{speechStyle}}` | Tone, verbosity, format and patterns in one line. | `{{speechStyle}} → Tone: dry…` |
| `{{speechPatterns}} / {{speechPatterns::N}}` | All speech patterns as a list — or just pattern N. | `{{speechPatterns::0}} → Says 'aye'.` |
| `{{backgroundHistory}}` | The Background / history field. | `{{backgroundHistory}} → Raised in…` |
| `{{worldSetting}}` | The World / setting field. | `{{worldSetting}} → A kingdom at war.` |
| `{{dialogueExamples}} / {{dialogueExamples::N}}` | All example dialogue — or just example N. | `{{dialogueExamples::1}} → the second example` |
| `{{charFirstMessage::N}} / {{greeting::N}}` | Greeting N (0 = the first message). | `{{charFirstMessage::1}} → the first alternate greeting` |
| `{{charPrompt}}` | The card's own system prompt. | `{{charPrompt}} → You are {{char}}…` |
| `{{charInstruction}}` | The card's post-history instructions. | `{{charInstruction}} → Stay in character.` |
| `{{charDepthPrompt}} / {{charAuthorsNote}}` | The card's Character's Note (depth prompt). | `{{charDepthPrompt}} → Keep replies short.` |
| `{{original}}` | In a card's system prompt: the prompt config's main system prompt. In its post-history instructions: the config's text sent after the chat, which then goes only there. Empty anywhere else. | `{{original}} Also: never break character.` |
| `{{charVersion}} / {{version}}` | The card's version. | `{{charVersion}} → 1.2` |
| `{{charPrefix}} / {{charNegativePrefix}}` | The card's image prompt prefix / negative prefix. | `{{charPrefix}} → 1girl, silver hair` |

#### The chat and its settings

| Macro | What it does | Example |
| --- | --- | --- |
| `{{lastMessage}}` | The last message in the chat. | `{{lastMessage}} → The gate creaks open.` |
| `{{lastUserMessage}} / {{lastCharMessage}}` | Your last message / the character's last message. | `{{lastUserMessage}} → I draw my sword.` |
| `{{lastMessageId}}` | The number of the last message (counted from 0). | `{{lastMessageId}} → 12` |
| `{{currentSwipeId}} / {{lastSwipeId}}` | Which swipe of the last message is shown / how many there are. | `{{currentSwipeId}}/{{lastSwipeId}} → 2/3` |
| `{{firstIncludedMessageId}}` | The oldest message that fit in the last prompt. | `{{firstIncludedMessageId}} → 4` |
| `{{idleDuration}}` | How long since your last message. | `{{idleDuration}} → 2 hours` |
| `{{input}}` | What's typed in the message box right now. | `{{input}} → I open the` |
| `{{summary}}` | The Chat Memory summary. | `{{summary}} → Earlier, the party…` |
| `{{authorsNote}} / {{defaultAuthorsNote}}` | The Author's Note in use / the prompt profile's default one. | `{{authorsNote}} → Keep it tense.` |
| `{{systemPrompt}} / {{defaultSystemPrompt}}` | The system prompt in use / the app's own. | `{{systemPrompt}} → You are {{char}}…` |
| `{{model}}` | The model answering. | `{{model}} → glm-4.6` |
| `{{maxContextTokens}} / {{maxResponseTokens}} / {{maxPrompt}}` | Context size, reply length limit, and context minus reply (in tokens). | `{{maxContextTokens}} → 32000` |
| `{{lastGenerationType}}` | What the last reply was: normal, regenerate, continue, impersonate or quiet. | `{{lastGenerationType}} → regenerate` |
| `{{system}} / {{wiBefore}} / {{wiAfter}}` | Context template only: the system prompt, and the lorebook text before / after the character. | `{{if wiBefore}}{{wiBefore}}{{/if}}` |
| `{{.messageCount}}` | Context template only: how many messages the chat has. | `{{if .messageCount < 8}}…{{/if}}` |

#### Variables

{{.x}} is a chat variable (saved with this chat); {{$x}} is global (shared by every chat). Setting prints nothing; ++, --, ??= and ||= print the value.

| Macro | What it does | Example |
| --- | --- | --- |
| `{{.x}} / {{$x}}` | Read a variable. | `{{.hp}} → 10` |
| `{{.x = value}}` | Set it (prints nothing). | `{{.hp = 10}} → (nothing)` |
| `{{.x += n}} / {{.x -= n}}` | Add / subtract (+= joins text when either side isn't a number). | `{{.hp -= 3}}{{.hp}} → 7` |
| `{{.x++}} / {{.x--}}` | Add / subtract 1 and print the new value. | `{{.turn++}} → 4` |
| `{{.x ??= value}}` | Set it only if it doesn't exist yet; prints the value it ends up with. | `{{.hp ??= 10}} → 10` |
| `{{.x \|\|= value}}` | Set it if it's missing or empty; prints the value. | `{{.name \|\|= Stranger}} → Stranger` |
| `{{.x ?? fallback}}` | The value, or the fallback if it doesn't exist. | `{{.title ?? none}} → none` |
| `{{.x \|\| fallback}}` | The value, or the fallback if it's missing or empty. | `{{.title \|\| none}} → none` |
| `{{.x == v}} (!= > >= < <=)` | Compare: prints true or false. A number compares as a number; unset counts as 0. | `{{.hp > 3}} → true` |
| `{{.x contains text}}` | Whether the value contains the text (any case). | `{{.inv contains rope}} → true` |
| `{{.a == .b}}` | Compare two variables. | `{{.hp == .hp_max}} → false` |
| `{{.a > 1 && .b}} (\|\| ! and parentheses)` | Combine conditions; prints true or false. | `{{.a==1 \|\| .b==1}} → true` |
| `{{cond ? yes : no}}` | Pick text by a condition (spaces around ? and :). Only the chosen side runs. | `{{.hp > 0 ? alive : dead}} → alive` |
| `{{getvar::x}} / {{getglobalvar::x}}` | Read a variable (any name, even with spaces). | `{{getvar::guard name}} → Bram` |
| `{{setvar::x::value}} / {{setglobalvar::x::value}}` | Set a variable (prints nothing). | `{{setvar::mood::calm}}` |
| `{{addvar::x::n}} / {{addglobalvar::x::n}}` | Add a number, or join text (prints nothing). | `{{addvar::gold::5}}` |
| `{{incvar::x}} / {{decvar::x}}` | Add / subtract 1 and print the new value. | `{{incvar::turn}} → 4` |
| `{{incglobalvar::x}} / {{decglobalvar::x}}` | The same for a global variable. | `{{incglobalvar::runs}} → 12` |
| `{{hasvar::x}} / {{hasglobalvar::x}}` | Whether the variable exists: true or false. | `{{hasvar::met}} → false` |
| `{{deletevar::x}} / {{deleteglobalvar::x}}` | Delete it. | `{{deletevar::met}}` |
| `{{getvar hp}} (space form)` | SillyTavern's shorthand for one-argument macros. | `{{getvar hp}} → 10` |

#### Conditions

Content inside a block is trimmed and un-indented, as in SillyTavern; start it with # to keep the spacing exactly.

| Macro | What it does | Example |
| --- | --- | --- |
| `{{if cond}}…{{/if}}` | Show the text only when the condition holds. | `{{if .met}}Welcome back!{{/if}}` |
| `{{elseif cond}} / {{else}}` | Other branches; the first that holds wins. | `{{if .hp > 5}}Fine{{elseif .hp > 0}}Hurt{{else}}Down{{/if}}` |
| `{{if !cond}}` | Not. | `{{if !.met}}Who are you?{{/if}}` |
| `{{if (.a \|\| .b) && .c}}` | Combine with && (and), \|\| (or) and parentheses. | `{{if .ready && (.hp > 0 \|\| .potion)}}Go!{{/if}}` |
| `{{if {{getvar::x}} > 3}}` | Either side of a comparison may be a macro. | `{{if .name == {{user}}}}It's you.{{/if}}` |
| `{{#if cond}}…{{/if}}` | Keep the block's spacing exactly as written. | `Line{{#if .a}}⏎Extra{{/if}}` |
| `{{endif}} / {{if::cond}}` | Other spellings SillyTavern accepts. | `{{if::.a}}Y{{endif}}` |
| `False values` | Empty, 0, false, off, no (any case) count as false; anything else is true. | `{{if .flag}} with flag = no → skipped` |

#### Lists

A list is a variable holding comma-separated items (rope, torch). Items can't contain commas.

| Macro | What it does | Example |
| --- | --- | --- |
| `{{.x push item}}` | Add an item (or several: push a, b). Prints nothing. | `{{.inv push rope}}` |
| `{{.x remove item}}` | Take one matching item out (any case). | `{{.inv remove rope}}` |
| `{{count: list}}` | How many items. | `{{count: inv}} → 2` |
| `{{each: list \| template}}` | The template once per item, one per line. {{item}} is the item, {{index}} its number. | `{{each: inv \| - {{item}}}} → - rope ⏎ - torch` |
| `{{each: list}}…{{/each}}` | The same, as a block. | `{{each: inv}}{{index}}. {{item}}{{/each}}` |

#### Maths and display

| Macro | What it does | Example |
| --- | --- | --- |
| `{{calc: expression}}` | Work out + - * / % ** and ( ), variables by name (hp, .hp, $gold), and floor ceil round abs sqrt min max clamp randint rand. | `{{calc: hp + armor * 2}} → 16` |
| `{{calc: x = expression}}` | Work it out and store it in a variable (prints nothing; $x for a global). | `{{calc: hp = min(hp + 5, hp_max)}}` |
| `{{switch: value \| case=text \| *=default}}` | Text for a value: exact values, numeric ranges (0-3) or * as the fallback. | `{{switch: mood \| 0-3=Tense \| *=Calm}} → Calm` |
| `{{repeat: text \| n}}` | Text repeated n times. | `{{repeat: ♥ \| hp}} → ♥♥♥` |
| `{{bar: cur / max \| width \| fill empty}}` | A filled/empty meter. | `{{bar: hp / hp_max \| 10}} → ██████░░░░` |

#### Randomness

| Macro | What it does | Example |
| --- | --- | --- |
| `{{random::a::b::c}}` | A random option, chosen again each time. | `{{random::rain::sun}} → sun` |
| `{{pick::a::b::c}}` | A random option that stays the same for this chat. | `{{pick::red::blue}} → blue` |
| `{{weighted::a:5::b:1}}` | A random option, each :N times as likely (none = 1, :0 = never). | `{{weighted::common:9::rare:1}} → common` |
| `{{roll::2d6+1}}` | Dice — terms can be added (1d6+1d4); a bare number is one die of that size. | `{{roll::d20}} → 14` |

#### Date and time

| Macro | What it does | Example |
| --- | --- | --- |
| `{{time}} / {{date}}` | The time / date in your system's format. | `{{time}} → 9:15 PM` |
| `{{time::UTC+2}}` | The time at a UTC offset (SillyTavern's {{time_UTC+2}} works too). | `{{time::UTC-5}} → 2:15 PM` |
| `{{isotime}} / {{isodate}}` | HH:mm / YYYY-MM-DD. | `{{isodate}} → 2026-09-21` |
| `{{weekday}}` | The day of the week. | `{{weekday}} → Monday` |
| `{{datetimeformat::format}}` | The date in your own format (YYYY MM DD HH mm ss). | `{{datetimeformat::DD/MM}} → 21/09` |
| `{{timeDiff::a::b}}` | How far apart two dates are, in words. | `{{timeDiff::2026-01-01::2026-01-03}} → 2 days` |

#### Text and structure

| Macro | What it does | Example |
| --- | --- | --- |
| `{{newline}} / {{newline::n}}` | One or n line breaks. | `a{{newline}}b → a⏎b` |
| `{{space}} / {{space::n}}` | One or n spaces. | `[{{space::2}}] → [ ]` |
| `{{trim}}` | Removes the line breaks on both sides of it. | `a⏎{{trim}}⏎b → ab` |
| `{{trim}}…{{/trim}}` | Keeps what's inside and trims its edges. (Text inside a single {{trim …}} is dropped.) | `{{trim}} x {{/trim}} → x` |
| `{{silent}}…{{/silent}}` | Everything inside runs (sets, rolls, lists) but nothing is shown. | `{{silent}}{{.hp = 10}} debug{{/silent}} → (nothing)` |
| `{{noop}}` | Nothing at all. | `a{{noop}}b → ab` |
| `{{reverse::text}}` | The text backwards. | `{{reverse::abc}} → cba` |
| `{{// note}}` | A comment — removed before anything runs. | `{{// TODO: add a twist}}` |
| `{{ // }} … {{ /// }}` | A comment over several lines. | `{{ // }}notes{{ /// }}` |
| `\{\{text\}\}` | Braces shown as written, not run. | `\{\{char\}\} → {{char}}` |
| `{{name args}}…{{/name}}` | Any macro that takes an argument can wrap text; the text is its last argument. | `{{setvar backstory}}Born in a village.{{/setvar}}` |
| `{{banned::word}}` | Accepted for SillyTavern cards; does nothing here. | `{{banned::delve}} → (nothing)` |
| `{{yourMacro}}` | Your own macros from Settings → Macros & Regex. | `{{sig}} → the Crimson Vow of Rin` |

#### Card images

Use the card's own images (the editor's Images section) by name. Each becomes a line of its own.

| Macro | What it does | Example |
| --- | --- | --- |
| `{{img::name}} / {{img::name::caption}}` | Shows one of the card's pictures in the message. | `{{img::map::The old map}} → the picture, captioned` |
| `{{background::name}}` | Switches the chat background to one of the card's backgrounds. | `{{background::tavern}}` |
| `{{expression::name}}` | Sets the character's sprite for this message, whatever the automatic picker says. | `{{expression::joy}}` |

#### SillyTavern macros with nothing to do here

Accepted so SillyTavern cards don't break. The macro checker points them out as tips, not mistakes.

| Macro | What it does | Example |
| --- | --- | --- |
| `{{isMobile}}` | Always false: prompts are built on the desktop, even when you chat from the phone. | `{{if isMobile}}…{{/if}} → skipped` |
| `{{hasExtension::name}}` | Always false: Rin Chat has no extensions. | `{{hasExtension::tts}} → false` |
| `{{instructUserPrefix}} and the other instruct… macros, {{chatStart}}, {{chatSeparator}}` | Instruct-format pieces. Rin Chat sends chat messages, not instruct-formatted text, so these print nothing. | `{{instructStop}} → (nothing)` |
| `{{reasoningPrefix}} / {{reasoningSuffix}} / {{reasoningSeparator}}` | Reasoning format pieces for text completion; nothing here. | `{{reasoningPrefix}} → (nothing)` |
| `{{firstDisplayedMessageId}} / {{allChatRange}}` | For SillyTavern's scripting commands; nothing here. | `{{allChatRange}} → (nothing)` |
| `{{outlet::key}}` | SillyTavern's lorebook outlets; Rin Chat's lorebooks have no outlet position. | `{{outlet::scene}} → (nothing)` |
| `{{charCreatorNotes}} / {{creatorNotes}}` | Left out on purpose: public creator notes often hold changelogs the model would take as instructions. | `{{charCreatorNotes}} → (nothing)` |

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

## Regex scripts

Ordered find/replace passes over message text, managed in Settings → Macros & Regex. They run *after* macros expand, and they can rewrite what the model receives, what you see, or both.

- **Find**: a bare pattern, or a `/pattern/flags` literal. Either way it replaces every match, not just the first.
- **Replace**: leave it empty to delete matches. Supports `$1`, `$<name>`, `$&` and `{{match}}` (the whole match).
- **On**: which messages it runs against: **User**, **AI**, **System**. A new script starts on AI only.
- **Display** and **Prompt** — two independent switches. *Display* changes what you see; *Prompt* changes what the model receives. A new script has **Display on and Prompt off**, so if you're trying to strip something from the prompt, tick Prompt. Both work on a copy, every time the message is used, and leave the message itself alone.
- **Message**: the third destination, and the one that *does* change the message. It runs once, as the message is saved, and what it writes is the message from then on: it shows in chat, it goes to the model, you can edit it by hand, and any macros in it are frozen to the values they had that turn. Good for a reading you want kept: a status line, a timestamp, a dice result. Add `[co]` before the part the model shouldn't see.
  Two things to know: it's exclusive with Display and Prompt (a script that rewrote the message and then ran again on the way out would apply itself twice), and it can't be taken back. Turning the script off leaves what it already wrote, so you'd edit those messages yourself.
- **Trim strings**: comma-separated bits removed from each match before the replacement runs. Handy for stripping wrapper tags while keeping their contents.
- **Min / Max depth**: only run on messages within that distance from the end. `0` is the newest message, and both ends are inclusive. Blank means no limit.

A card can carry its own scripts, in the card editor's Regex section, and they run after yours on that character's messages, once you've allowed them. You're asked per script, in the chat: each one shows what it finds, what it puts in its place and where it applies, and you can allow one, allow them all, or keep them off. A script that arrives new or changed (a card updated by an import that replaced its scripts, or one Card Forge wrote) stays off until you allow it again; renaming one doesn't count as a change. A script you write or edit yourself in the card editor counts as allowed. If a script's replacement can put a web address into a message, the question says so: an image there would be fetched from that server, along with whatever the script copied into the address. The editor's **Run these in chat** switch allows or stops every script the card holds.

A script imported from a SillyTavern card that sets neither "display only" nor "prompt only" lands on **Message**, because that is what it does in SillyTavern: it edits the stored message.

Scripts run **in the order they're listed**, which is the order you added them — one script's output is the next one's input.

The **Regex live tester** at the bottom of the panel runs your enabled *display-phase, AI-target* scripts over sample text and updates as you edit.

> **Tip:** A pattern with a quantifier applied to an already-quantified group — `(\w+\s?)+` is the classic — can take hours to match and is **refused**: the script does nothing rather than freezing the app, and the editor marks the pattern in red so you know. An invalid pattern is flagged the same way. Very long messages (over 100,000 characters) are skipped.

```
Strip a phrase the model keeps repeating:
          Find:    /\s*I can't help but notice[^.]*\./g
          Replace: (empty)
          On: AI · Display + Prompt

        Hide OOC blocks from view but keep them in context:
          Find:    /\[ooc\][\s\S]*?\[\/ooc\]/g
          Replace: (empty)
          On: AI · Display only
```
