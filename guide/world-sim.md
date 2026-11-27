# World Sim Cards: Clock, Places, Relationships and Endings

> Read this on [rin.chat](https://rin.chat/guide/world-sim/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

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
