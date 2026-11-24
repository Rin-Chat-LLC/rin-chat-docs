# Writing a Rin Chat Adventure Mode game

> For AI models and tools that write Rin Chat Adventure Mode game cards. Generated from the app's own Guide on every release (this copy is version 1.0.6), so it matches what the app does. "Settings → …" names a place in the app's Settings.

How to write the card file:

- A game is an AICC character card. Its full shape and rules: https://rin.chat/card-format.md
- Make it a game master card: set `metadata.features.card_type` to `"adventure_gm"` and `metadata.features.adventure_mode` to `true`.
- The editor's game boxes are strings under `metadata.features`: **Ruleset** is `adventure_ruleset`, **GM style** is `adventure_gm_style`, **Extra GM guidance** is `adventure_gm_rules`. Leave a key out to keep the app's default for that box.
- **Character sheet** is `metadata.features.sheet_template`: the JSON described under "Building your own game", stored as a JSON string.
- The `[[roll: ...]]`, `[[choice: ...]]` and other markup below is what the game master writes in its replies. Teach it through the Ruleset and GM style boxes and show it in the greetings.

## Adventure mode

Adventure mode turns a chat into a game-mastered adventure: the character (written as a GM) offers **choice buttons**, demands **dice rolls**, and keeps a live **character sheet** the story updates as you play. The app referees — it rolls the dice and applies the stat changes; the model only ever asks. Works with any model — how well it plays GM depends on the model.

> **Tip:** Quickest way to try it: your library already contains the **Adventure Mode Narrator**, a card built for this. Start a chat with it — nothing to configure.

**Turning it on:** it's carried by the *card*, not a setting. Adventure-built cards enable it automatically for their own chats; flag any card yourself by setting its **Card type** (the dropdown in the card editor's header) to *Adventure Mode Game Master*. Ordinary cards carry none of it.

**Playing:**

- **Choices**: click to take the action; some carry costs ("Bribe him (5 gold)") that hit the sheet the moment you pick them, so the GM can't charge you twice. **Shift-click** a choice and the model performs it *in your voice* instead of sending the plain label. You can always ignore the buttons and just type.
- **Rolls**: the GM states what you're rolling for and what's at stake, but **the app rolls the dice**. Typing "I rolled a 20" does nothing — a 🎲 you type is stripped from your message, and a 🎲 line the *GM* writes is deleted before the reply is saved. Only the app's reports are real. Results render as dice cards — success/failure vs the difficulty, nat-20 shimmer, and damage on successful attacks (rolled by the app, applied exactly by the GM). Sometimes you'll get **two or three rolls side by side** for genuinely different tactics — take one.
- **Your abilities move the dice.** When a roll leans on one of your abilities the app looks the score up on your sheet, applies the classic modifier, and names it on the button ("Roll Stealth (dexterity +2)") — a forgetful GM can't ignore your scores and an eager one can't count them twice.
- **Social rolls**: a say-box appears: what you actually say travels with the dice, and a good pitch legitimately tilts the result (a stated −6 to +6, never more — enough to carry a bad roll). The mask button drafts the line in your character's voice; **Shift-click the mask** to give it directions first ("be humble, mention the ring") and preview the draft before it drops into the box. `Enter` rolls, `Shift+Enter` adds a newline, and leaving the box empty just rolls normally.

**Your character:** the greeting's create-your-character button (or the person icon at the top of the sheet, once the greeting scrolls past) opens the builder — name, class, abilities (roll 4d6-drop-lowest then drag each score onto the ability you want, 27-point buy, or **manual entry** — honor system, type the scores in to bring a real tabletop character over as-is; rename, add or remove abilities to fit any genre), hp, level & XP, starting inventory, and notes to the GM. Give them a **portrait** too — upload one or generate it with AI, same generator the card art uses; it shows at the top of the sheet. **Complete sheet for me** first asks what you want them to be (optional: leave it empty and the AI decides), then fills only the fields you left *empty* (never your numbers, never overpowered gear); **Cancel** stops it mid-flight. *Use for this chat* stays disabled until the character has a name and every rolled score has been placed.

**Saving and reusing characters:** **Save as persona card** keeps a reusable copy in your library; load it later from the *Load a saved adventure persona* dropdown at the top of the builder. A character you loaded shows **Update persona card** instead (it edits that card rather than making a duplicate); to get rid of one, delete the persona card from the library. **Use for this chat** starts playing with the sheet, and **Play without a sheet** skips character creation entirely. In an adventure chat your character replaces your regular persona, so {{user}} *is* them — your normal persona is untouched everywhere else. Reopening the builder mid-game is safe: it re-applies your abilities, class and level, but leaves hp, max hp, XP and everything you picked up in play exactly as play left them.

**The sheet** (🎲 in the right sidebar) keeps bars such as hp pinned at the top, above tabs that sort the rest by name prefix: *Character*, *Inventory* (your *Money* sits there too), *Other* for anything unprefixed, and *DM* while spoilers are on. A tab only shows when it has something in it, and with no Character tab the unprefixed stats are simply *Stats*. There's a history of every change, and each entry jumps to the message that did it.

- **Edit** any value by typing in its box: it saves on `Enter` or when you click away, and `Esc` puts the old value back. On a number, `+3` and `-3` add and subtract, and `=-1` sets it outright. Health, stats with a maximum and clocks stop at 0; any other stat, like a PbtA cool, can go negative. **Saving an empty box deletes that stat**.
- **Add** a stat with the row at the bottom: the section follows the open tab, or pick one, type a name and a value, hit **Add**. The section *is* the stat's name prefix, so "rope" under Inventory becomes `inv_rope`. Adding a DM stat switches spoilers on so you can actually see it.
- **Click an item's name** in the inventory to give it a description; the GM sees it too.
- **Right-click a stat** (on a touch screen, tap the **⋯** beside it) to **rename or move** it between sections (the dialog shows you exactly what the GM will see) or to **delete** it.
- Your edits are the truth the GM must respect, and they **outrank the story** — a stat you fix or delete by hand stays fixed even if you later delete or reroll an older message. (An old message that *runs again* can still re-emit its own changes, and a GM that no longer sees a stat may recreate it — that's the story happening, not the sheet losing your edit.)

**Stats on screen.** A strip at the top of the chat keeps your numbers in view while you play. Unless the card picks its own, it shows your hp, level and XP, any other stat with a maximum, your money, and the location, day, time or date when the game tracks them (up to eight), then the card's meters and the clocks you're allowed to see. Each has an icon. Hp, meters, clocks and anything with a maximum draw a bar, and XP's bar is your progress through the current level; the rest show their value. During a fight the strip adds the turn order, and each name in it carries that fighter's health: a heart, their HP, and a small bar when their maximum is known. **Show less** folds it to one sideways row (still showing whose turn it is, with their HP) and the arrow, **Show more**, opens it again. On a phone it starts folded.

**Choosing what it shows:** open **On-screen stats** in the sheet. **Show stats in the chat** turns the whole strip off for this chat, fights included, and a checkbox per stat adds or removes it, starting from the card's picks. Tap a stat's icon to pick another from the icon grid (**Automatic** goes back to the card's), and use the arrows to move it up or down the strip. The choices are saved with this chat only, and stats the card adds in a later update still appear. DM stats are never on the strip.

**What each turn changed.** Under a message that moved the sheet, a folded **Changes** box previews the first few changes ("HP 100 → 92", "Gold +50", a stat that's new or removed) and "+N more" for the rest; open it for the full list. Your own message gets one too when a choice's cost hit the sheet. DM-side changes only show with spoilers on. Messages written before this feature have no Changes box.

**Tap a stat** on the strip, or in an open Changes box, to see its detail card: the value and a large bar; for XP and level, the level bands around yours with **You're here**; and **Recent changes**, each with **Jump** to the message that made it.

**The GM's hidden side.** Flip *Show DM stats (spoilers)* in the sheet's settings to see what the GM is tracking: enemy hp and states (`npc_`), world flags, hidden clocks and any condition it's put on you (`dm_`), plus the **campaign plan** — an overarching arc the app writes right after your first action, which every later reply steers by, and which also starts the GM off already tracking the people and clocks that matter. The plan is fully editable: rewrite it to change where the story is heading, or right-click a selection to rewrite that part with AI. Writing it takes a few seconds ("Crafting the scenario…") and **Stop** cancels it — the box stays there empty and writable either way, so you can simply type one. Deleting your way back to the start of the chat clears the plan so your next opening gets a fresh one.

**The GM tidies up too.** It can drop stats that are finished — a slain enemy's hp, a clock that ran out, an item used for good. It can only drop *its own* stats and consumables; your character's abilities, hp, level and XP are yours alone and the app refuses anything else. Gear that's confiscated rather than destroyed **moves to whoever took it** instead of vanishing, so it comes back intact when you get your pack off the warden (turn on spoilers if you want to watch it sitting there).

**Levelling** is the app's job, not the GM's: every 100 XP earns a level, and you get a dialog with the HP it adds and **two points to raise abilities** (up to 18) — spend them, or take *Skip the points* to bank the level and the HP without them. Earn two levels at once and you get both, and all four points, in one go. Your current HP rises by exactly what the maximum did, so levelling is never a free heal. It works off the sheet, so XP you type in by hand levels exactly like XP the GM awarded — but it needs a character: a sheet-less adventure never levels.

**House rules the app enforces:** healing can never push past your max HP; deleting, rerolling or swiping a message **reverses its stat changes** (with a "Sheet reverted" notice, so a refund is never silent); and at 0 hp you're never killed — the app puts you back at **5 hp**, weak but alive, and the GM narrates the setback. Turn on **Hard mode** in the sheet's settings and 0 hp is a real death: final, and the sheet won't let you edit that hp back up. Rests are house rules the GM is *taught* rather than rules the app enforces — a short rest heals a chunk, a long rest restores you to full — so a weaker model may play them loosely.

**Making your own?** See Writing an adventure card.

> **Tip:** Adventure chats work best with a model that follows instructions well — the app referees the mechanics, but the storytelling is all the model's.

#### Being the DM yourself

The same machinery runs backwards: **you** game-master, and the AI characters are the players. Open any chat that has a character in it, click the **dice button** in the sidebar, and turn on **I’m the DM**. No game-master card is needed — a one-on-one chat works (a party of one), and so does a group.

- **Everyone at the table gets a sheet.** The app stats each character up from their card — or reads the sheet the card already carries (card editor → *Adventure Mode Rulesets → Adventure character sheet*). Every number is yours to edit in the sidebar: abilities, HP, level, XP, inventory, and stats you invent yourself — HP and XP take `-6` or `+50` as well as a plain number. Award enough XP and a **level-up** button appears, which adds the level and its hit points and leaves the ability points to you.
- **Players state attempts, never outcomes.** A character reaching for something they could fail at ends their message with an attempt, and a roll box appears under it. You pick the ability and difficulty (or, for a game with its own dice, whatever its sheet says a check is made of: see Building your own game), optionally write *what happens if they succeed* and *if they fail* — before rolling, which is the point — and press Roll. Only the branch that actually happened is sent. Your choices stay on that message, so deleting the result and rolling again finds them still there.
- **Call for a check nobody asked for** from the roster in the sidebar — "everyone roll Perception" — with the same box.
- **Go round the table** with the **▶ Round** button above the composer: each player takes a turn in order, so everyone reacts to the scene before it comes back to you.
- **The greeting is set aside** when you turn DM mode on: the opening scene is yours to write. Turn it off and the card’s own greeting comes back.
- **Your notes stay yours.** Two tags, and the difference matters: `[co]…[/co]` stays *on your screen* and is never sent to anyone — the trap they haven’t found, who is lying, what happens if they open the box. `[h]…[/h]` is the opposite: it disappears from the chat and is withheld from every player’s prompt, but your own AI-assist draft still sees it — a note to your co-writer rather than to yourself. (In ordinary adventure mode `[h]` goes to the model, because there the model is the referee.) The stat board’s *npc_* and *dm_* stats are private the same way, and the notes panel (📓 in the toolbar) keeps longer material per chat.
- **Stuck for words?** The impersonate button drafts *your* next turn as the game master — scene, NPCs and consequences — rather than a character’s reply.

> **Tip:** The dice are still the app’s: a 🎲 line a player writes is stripped as a forgery, so the only real results are the ones you rolled.

## Writing an adventure card

The most important thing to know: **the app already teaches the game mechanics.** Every turn, the model receives the directive syntax, the referee rules (app-rolled dice, stat namespaces, rests, the works) and the live character sheet. Your card's job is everything the app *can't* supply: **who the GM is, what the world is, and how the adventure opens.** Don't spend card tokens re-explaining syntax — demonstrate it instead (see dialogue examples below). Those mechanics are roughly D&D's by default, and a card can REPLACE them with another tabletop system: see *Teaching it a different tabletop system* at the end of this section.

Already handled for you, every turn, so your card never needs to say it: the app applies ability modifiers to rolls, deletes any dice line the GM writes itself, teaches the GM to drop finished stats with `[[unstat:]]` (and refuses when it aims at the player's own), teaches it to move confiscated gear to whoever took it instead of deleting it, reminds it every turn that its own `npc_`/`dm_` values are its to *move*, and seeds a starting set of them from the campaign plan.

**1. Write the character as a narrator, not a companion.** Name it like one ("The Dungeon Master", "Adventure Mode Narrator") and make the description about its *style of running a game*: second person, present tense; fair but consequential; never speaks or decides for the player. A card written as a companion character will chat *with* the player instead of running a world *around* them.

**2. The greeting is the front door.** Set the opening scene in a few paragraphs, then offer the starting buttons. `[[character]]` and `[[starter_choice: …]]` are *opening-only* — they run character creation, and the GM is told never to emit them once play has begun:

```
…the caravan crests the ridge and the valley opens below, chimney smoke and trouble.

        [[character: Create your character]]
        [[starter_choice: Ride down before dark]]
        [[starter_choice: Camp on the ridge and watch the road]]
        [[choice: Just start walking — no character sheet]]
```

`[[character]]` opens the character builder. `[[starter_choice: …]]` runs character creation first, *then* sends the choice — use these for your main openings. A plain `[[choice: …]]` (the ordinary in-play directive) gives players a sheet-less freeform way in. Note that these buttons live under the *last* message, so they're gone the moment play starts — afterwards the character sheet's person icon is the way in.

**3. Behavior rules = game feel, not syntax.** Use them for how the game should play: "State the stakes before demanding a roll", "Consequences are permanent — a failed roll changes the situation, never gets retconned", "Introduce named NPCs with wants of their own", "Prefer hard choices over combat". The app enforces the mechanics; your rules shape the drama.

**4. Dialogue examples are your strongest lever.** Models imitate examples far more reliably than they follow instructions — one worked exchange showing the protocol in action beats a page of rules. Show a roll demand with stakes, stat bookkeeping, and hidden DM tracking. Note the `mod dexterity` segment: the app looks the ability up on the character sheet and applies the modifier itself, so the GM never has to do (or fudge) that math:

```
{{user}}: I try to slip past the guard post.
        {{char}}: Torchlight sweeps the road. If they spot you, the gate closes and the
        alarm brings the whole watch.
        [[roll: 1d20 vs 13 | Stealth | Slip past unseen — spotted means the gate slams and the watch turns out | mod dexterity]]

        {{user}}: 🎲 Stealth (dexterity +2) check: 1d20+2 → 16 + 2 = 18 vs DC 13 — SUCCESS
        {{char}}: You ghost between the wagons; the sentry yawns at nothing. Inside the
        walls, the fence's shop is dark — but a light burns upstairs.
        [[stat: npc_watch_alert = calm]]
        [[choice: Knock anyway]]
        [[choice: Wait in the alley until the light goes out | inv_torch -1]]
```

A choice can also roll one of the card's generators when it's picked — add a `gen` segment with anything a `[Create …]` tag takes. The roll lands in the player's message, and the GM narrates it next turn: `[[choice: Look for recruits | gen Mercenary x3 Class=Archer, as=recruits]]`. Effects still go in their own segment: `[[choice: Hire a guide (10 gold) | gold -10 | gen Guide]]`.

Choices are drawn where they're written, so you can group them under headings (**Fight**, **Talk**, **Flee**) instead of one long list. When a reply also asks for a roll, its choices are grayed out: the roll decides first.

**5. Seed the world.** Put geography, factions and tone in World / setting; use lorebook entries keyed to names for deep lore the model only needs when it comes up. The GM's hidden stats are its working memory — `npc_<who>_<what>` for a creature or person it is tracking, `dm_<what>` for everything else it keeps to itself (world state, a ticking clock, a secret), and `dm_<player>_<what>` for a condition it puts on the player (poisoned, disguised) — never the player's own prefix, which is their character sheet. Both hide behind the spoiler toggle. Your examples should show it using them, and clearing finished ones with `[[unstat: npc_wolf_hp]]` so a long game's sheet doesn't fill with dead business.

**Meters the player can see.** A stat named `meter_<name>` with a number above 0 in `meter_<name>_max` shows as a labeled bar on the stat strip at the top of the chat the whole time, fight or not: affection, sanity, heat, anything that fills up. It can't go past its max, and `dm_meter_` or `npc_meter_` ones stay hidden. The GM isn't taught meters, so the card says what to track in its own rules:

```
Track Kisa's affection in meter_kisa_affection, 0 to 10 (meter_kisa_affection_max = 10).
[[stat: meter_kisa_affection +1]]
```

**Choose the on-screen stats.** An Adventure Mode Game Master card's **Adventure Mode Rulesets** section has an **On-screen stats** list, below the Character sheet box: the stats the chat's strip shows, in your order, each with an optional label and an icon from the icon grid (*Automatic* picks one from the name). The arrows set the order, and a player can still pick their own icons and order in their sheet. Write a stat without the player's prefix, `hp` rather than `ava_hp`, and it finds each player's own; up to 12. **Leave it empty and the chat picks them from the sheet** (hp, level, XP, money and anything with a max). Meters and visible clocks are always added after your list, and players can still hide or add stats from their sheet. Each row's **More** adds named levels (like 1 to 20 Critical, 21 to 50 Wounded) and short **Raises it** / **Lowers it** lines; the player sees them on the stat's card, with the current level beside the value on the strip, and the game master is told them as well, so the story keeps to your levels and causes. Card Forge can write this list for you.

**6. The campaign plan reads your card.** On the player's first action the app writes a hidden arc from your card's description, personality, behavior rules, setting, background and system prompt — but *not* from your greetings or dialogue examples (the opening scene is passed separately). If the true situation behind your opening lives only in a greeting, move it into the description or the setting so the planner can build on it.

**7. Ship it right.** Set the editor's **Card type** to *Adventure Mode Game Master* (this is what auto-enables the mode for whoever imports the card — it travels in JSON and PNG exports), tag it so it's findable, and use Creator notes to tell players what to expect (difficulty, themes, whether a character sheet matters).

> **Tip:** Test-play a few turns and watch the sheet: if stats drift or rolls come without stakes, add a dialogue example demonstrating the exact behavior you want — weaker models need more example coverage, not more rules.

#### Teaching it a different tabletop system

Everything above describes the rules the app teaches by default, which are roughly D&D's. If your game is Fate, Powered by the Apocalypse, Cortex, Forged in the Dark or something you wrote yourself, **a card can replace them.** An Adventure Mode Game Master card carries three boxes in the card editor's **Adventure Mode Rulesets** section: **Ruleset** (how a roll is read: what a target number means, modifiers, damage, rests), **GM style** (how the game is run: pacing, when to call for a roll at all) and **Extra GM guidance**, which is added on top and replaces nothing. A card's text beats the same boxes in Settings, so a game you share is self-contained: whoever opens it plays your system without changing a setting.

What you cannot replace is the part the app's own parsers read: the directives, the dice grammar and the sheet. That is sent every turn whatever your ruleset says, which is exactly why rewriting the rest cannot break the choice buttons, the character sheet or undo.

**Decide first what the numbers on your sheet are**, because it decides what a roll should say, and getting it backwards is silent. If a rating is a *modifier*, as in Fate, PbtA, Forged in the Dark and Cortex, the roll adds it with a `bonus <stat>` segment and the app does the arithmetic. If a rating is the *target*, as a percentile skill is, nothing goes on the roll at all: the dice stay bare and the GM compares the number to the sheet itself. Use `bonus` on a percentile skill and a `1d100` becomes `1d100+55`, which is not a close call. `mod <stat>` is a third thing again and is always the D&D ability formula, so a system whose sheet already holds the final number should forbid it outright: a rating of 3 reaches the dice as +3 through `bonus` and as -4 through `mod`. If the stat a `mod` or `bonus` names isn't on the sheet, the roll goes ahead without it and the report the GM reads ends with `· cool not on the sheet`, so a misspelled stat shows up instead of quietly changing the odds.

**Dice the app can roll:** `1d20+2` and anything shaped like it, `4dF` for Fate and Fudge dice, `3d6kh1` and `2d6kl1` to keep the best or worst of a pool, `d8+d6+d10` to add different dice together, and `(d8+d6+d10)kh2` to keep the best two *across* dice of different sizes, which is how Cortex scores a trait pool. Rolls that drop dice show you the ones they dropped. A `!` after a die makes it **explode**: on its top face it rolls again and adds, up to 10 times per die, so `1d6!`, `2d10!` and `1d8!+2` cover Savage Worlds acing and World of Darkness 10-again, and the roll shows each step (`6!+6!+2 = 14`). The `!` goes before a keep, `3d6!kh1`, so a Savage Worlds trait die with its wild die is `(d8!+d6!)kh1`. It works in a `dmg` segment, a character sheet's dice and `{{roll::1d6!}}` too; Fate dice can't explode.

**What it will not do:** a target number means meet or beat, unless the card's character sheet sets its roll method to roll-under (Call of Cthulhu and that family; see Building your own game), in which case at or under succeeds. A system that counts successes in a pool (World of Darkness, Shadowrun) cannot use a target: leave it off, and the app reports the number, claims no verdict, and prints every face it rolled, so your ruleset can tell the GM to read it however your system says.

**If the GM reads the result itself, put the numbers on the sheet before you play.** This is the one that bites a percentile game. With no Spot Hidden on the sheet the GM will invent a threshold to compare against, and the same action then succeeds one turn and fails the next, with nothing on screen to show why. Fixed bands are safe, because they live in your ruleset text: a 7-9 in PbtA is a 7-9 for everyone.

**Giving the character builder your sheet, and choosing how checks roll.** A Game Master card can also describe its character sheet (abilities, groups, dealt scores, HP, levels) and how its checks are rolled and judged. That is all in Building your own game, with finished examples for Fate, Powered by the Apocalypse, Cortex Prime and percentile games.

**Writing it so a model actually follows it.** Every one of these came from watching a real game go wrong:

- **Name the wrong dice as well as the right ones.** "Rolls are 4dF" is not enough on its own, because every worked example a model has ever seen is a d20. "NEVER use d20; this game rolls 4dF" is what stops it.
- **Forbid the segments your system does not use,** and say why. Told only what Fate does, a model still added advantage out of habit; "never use adv or dis, an edge in Fate is an aspect you invoke" stopped it.
- **Put one fully worked roll line in the ruleset.** It is the only concrete example the model gets, and it is the single highest-value line in the box.
- **Say what to DO on each outcome, not just what the bands are called.** A table of bands was not enough: the model read "6 or less is a hard move" and then offered another roll. Phrase them as instructions to the game master.
- **Repeat the dice clause.** Name it in the rule, show it in the example, and say what it is called, or the same ruleset will produce `2d6kh1` in one run and a bare `2d6` in the next, which are very different odds.

> **Tip:** Check it landed before you play: open the prompt inspector on a reply and confirm your own words are in there and the default ruleset is not. The context meter is not that check, because its Adventure ruleset row looks identical whether the text is yours or the built-in one.

## Building your own game (step by step)

Everything you need to turn a tabletop system, a published one or your own, into a game anyone can import and play. It goes in order: make the game master, teach it your rules, describe your character sheet, choose how checks roll, add lore, write the opening, make ready-made characters, test it, and share it. Finished examples for five common kinds of system are at the end, then answers to the questions people run into.

#### What a game is made of

- **A Game Master card.** One card whose **Card type** is **Adventure Mode Game Master**. It IS the game: the narrator, the world, the rules, the character sheet and the lore all live on it.
- **Persona cards** (optional): ready-made characters, for players who want to start without building one.
- **A bundle**: one `.aicc` file holding the Game Master card, any persona cards and their art. It is how you hand the whole game to someone else.

Nothing here needs code. The rules and the sheet are text you type into boxes on the card, and everything travels with the card when it is exported.

#### Step 1: Make the Game Master card

Make a new card and set **Card type**, at the top of the card editor, to **Adventure Mode Game Master**. That turns Adventure Mode on for anyone who opens the card, and fills the editor's **Adventure Mode Rulesets** section with the game's settings (Ruleset, GM style, Extra GM guidance, Character sheet, On-screen stats, Story clock).

- **Name it as the narrator**, such as "The Game Master" or "The Keeper", because the name is who the model plays. Put the game's title in **Display name** if you want the library to show that instead.
- **Description and personality** describe how this game master runs a game: second person, present tense, fair but consequential. Write a narrator, not a companion, or the model will chat with the player instead of running a world around them.
- **Behavior rules** set the feel of play ("state the stakes before calling for a roll", "a failure changes the situation, it is never undone"). Leave the dice to the ruleset.
- **World / setting** and **Background** hold the setting and the true situation behind your opening. On the player's first action the app writes a hidden campaign plan from these fields, not from the greeting, so anything the plan needs must be here.
- **The clock switch** (in Adventure Mode Rulesets): leave it on for a game with something ticking, a heist or a deadline; turn it off for open-ended play, where a countdown only invents false urgency.

More on writing each of these, with examples: Writing an adventure card.

#### Step 2: Teach it your rules

Out of the box the game master is taught rules that are roughly D&D's. Three boxes in the card's Adventure Mode Rulesets section replace that, for this card only:

- **Ruleset**: how a check is made and read. Your dice, what a target means, what the sheet's numbers are, what happens on each result, damage, recovery.
- **GM style**: how the game is run. Pacing, when to call for a roll at all, how spoken words are judged.
- **Extra GM guidance**: added after everything else and replaces nothing. House rules and the things this particular game needs its game master to know.

A card's text beats the same boxes in Settings, so the game is self-contained: whoever opens it plays your rules without changing a setting. What your text cannot replace is the part the app itself reads, the directives, the dice grammar and the sheet, which is sent every turn. That is why rewriting the rules can never break the buttons, the dice or the character sheet.

**A ruleset that works covers these, in this order:**

1. **The dice, and the wrong dice.** "Every check is 4dF. NEVER a d20." Every example a model has seen is a d20, so naming the dice you do NOT use is what stops it.
2. **One fully worked roll line**, exactly as the game master should write it. It is the most valuable line in the box.
3. **What the sheet's numbers are.** A modifier is added with a `bonus <stat>` segment. A die size goes into a pool as that die. A percentile skill is the number to roll under, so nothing is added. Say which, and forbid `mod` unless your game derives modifiers the way D&D does: `mod` is always D&D's (score - 10) / 2, so it turns a +3 into -4 and a d12 into +1.
4. **Targets**: your difficulty ladder, or "no target" for a game that reads bands.
5. **What to DO on each result**, written as instructions to the game master ("on a 7-9, they get it but you introduce a cost"), not just the names of the bands.
6. **Your game's resources** (stress, fate points, momentum, sanity) and which stat holds each.

**What your ruleset can tell the game master to write.** These are the directives the app reads; your ruleset decides how they are used.

- `[[roll: <dice> vs <target> | <label> | <what is at stake> | <segments>]]`. The segments are optional: `bonus <stat>` adds a sheet number as it stands, `mod <stat>` adds D&D's modifier for it, `adv` / `dis` roll the d20 twice, `dmg <dice>` rolls damage on a success, and `speak` lets the player argue their case in words before the verdict. Leave `vs <target>` off for a game that reads results in bands.
- `[[stat: <name> = <value>]]`, `[[stat: <name> +2]]`, `[[unstat: <name>]]`. Names say whose a stat is: the player's own sheet uses their name as a prefix (the game master must not put conditions there), `inv_` is inventory, `money_` is money, `npc_<who>_<what>` tracks a character the game master runs, `dm_<what>` is anything else on the game master's side of the screen, and `dm_<player>_<what>` is a condition put on the player (stress, a wound, a curse). `clock_<name> = 2/6` draws a progress dial. The npc_ and dm_ stats are hidden from the player.
- `[[choice: <action> | <effects>]]` offers buttons; see Choices & interactive replies.

**Dice the app can roll:** `1d20+2` and anything shaped like it, `4dF` (Fate and Fudge dice), `3d6kh1` and `2d6kl1` to keep the best or worst, `d8+d6+d10` to add different dice, and `(d8+d6+d10)kh2` to keep the best two across different dice. **Not supported:** dice that explode on their maximum (have the game master call for a second roll instead), and counting successes across a pool (leave the target off; the app prints every face, and your ruleset tells the game master how to count them).

#### Step 3: Describe your character sheet

The **Character sheet** box, under the ruleset, tells the app what a character in your game looks like. Without it the character builder makes a D&D character: six D&D abilities, then 4d6 or a 27-point buy. It is written as JSON. Every field:

```
{
  "abilities": {
    "label": "Skills",
    "names": ["Athletics", "Fight", "Notice", "Rapport", "Stealth", "Will"],
    "scale": "number",
    "deal": [3, 2, 2, 1, 1, 0]
  },
  "groups": [
    { "label": "Approaches", "names": ["Careful", "Clever", "Flashy"], "deal": [2, 1, 0] }
  ],
  "notes": ["Concept", "Trouble", "Stunts"],
  "hp": false,
  "levels": false,
  "roll": { "method": "add", "dice": "4dF" }
}
```

- `abilities.names` (required): the abilities a new character starts with, up to 24. Players can still rename, add or remove them in the builder.
- `abilities.label`: the heading over them, in place of "Abilities".
- `abilities.scale`: `"number"` (the default) shows scores as written; `"die"` shows a 12 as d12, for systems whose ratings are dice. Either way the sheet stores the number, so your ruleset should say what it means.
- `abilities.deal`: a fixed set of scores the player places, one to an ability, by clicking or dragging, in place of D&D's dice and point buy. It may be shorter than the list of names (abilities it does not reach stay at 0), never longer. Scores may be negative. Leave it out to keep D&D's roll and point buy with your own ability names.
- `abilities.points`: your game's own point buy, in place of D&D's 27 points across 8 to 15. `{ "total": 10, "min": 0, "max": 4 }` gives the player 10 points to spread across the abilities, one point per step, no score below 0 or above 4. `min` defaults to 0 and `max` to no cap. With a `deal` as well, the player chooses either one. Numbers only, not a `"die"` scale.
- `groups`: more sets of abilities beside the first, each with its own `label` (required, and different from every other), `names`, `scale`, `deal` and `points`. Each is its own block in the builder, dealt or bought on its own. Up to four.
- `notes`: headings that a new character's "Who they are" starts with, for the parts of your sheet that are words rather than numbers (aspects, moves, backgrounds, powers).
- `hp`: `false` for a game without hit points (no HP field, no HP on the sheet, none on a level-up), or a number every character starts with in place of D&D's Constitution formula. Leave it out for D&D's formula.
- `levels`: `false` for a game without levels or XP. Neither is asked for or put on the sheet, nothing levels up, and the game master is not told to award XP.
- `roll`: how checks are rolled and judged; see Step 4.
- `start`: stats the chat starts with, set when the player clicks Use in the builder exactly as if the game master had written `[[stat: name = value]]` for each, such as `{ "health_bar": 10, "health_bar_max": 10, "floor": 1, "clock_collapse": "0/6" }`. A value is a number of 0 or more or a short text, up to 40 stats. Re-running the builder only sets the ones the sheet does not have yet. Use it for fixed starting resources the game master should not have to remember; anything worked out from the scores, like a modifier, stays the game master's job.

If the box cannot use what you typed, a line above it says why (invalid JSON, a deal longer than its names, a group with no label). A template it cannot read is ignored, so the card falls back to the D&D builder rather than breaking. A `start` it cannot read is ignored on its own, and the rest of the template still works.

**What the template changes, everywhere at once:**

- **The character builder** (the button in your greeting, or the person icon on the character sheet) starts from your abilities, one block per group, and offers *Deal the scores* and *Enter scores* instead of D&D's methods. "Who they are" starts with your headings, and HP, Level and XP are hidden where you turned them off.
- **Complete sheet for me** describes the character to the AI in your terms and writes under each of your headings. It also sends the start of your Ruleset box and any lorebook entries keyed to one of your headings, "character creation" or "intake", so its answers use your game's backgrounds, classes and gear. What you type in the box it opens with goes too, ahead of the AI's own ideas but behind your ruleset and the scores, which it never changes.
- **The game master's turn-by-turn view of the character** lists each group under its heading in your notation, with no D&D scale, and tells it to read the numbers the way your ruleset says rather than through `mod`. That applies to a sheet that deals its scores or counts in dice; a template that only renames abilities keeps the D&D wording.
- **Persona cards**: a character built in the builder keeps a copy of your template, so its card shows the sheet your way in the card editor and carries it when exported (Step 7).
- **DM mode**: when a player runs the game themselves, the AI party members are seated and statted on your sheet, and the roll box builds checks your way (Step 4).

#### Step 4: Choose how checks roll

`roll` in the template says how a check is rolled and judged. There are four methods:

- `{ "method": "d20" }`: D&D. 1d20 plus the ability's modifier against a DC, with advantage and disadvantage.
- `{ "method": "add", "dice": "4dF" }`: the dice plus the ability's number as it stands, against a target or none. Fate is `4dF`, Powered by the Apocalypse `2d6`.
- `{ "method": "pool", "keep": 2 }`: a pool where each trait is a die of its size, rolled by Cortex rules (below). `keep` is how many dice make the total; 2 if left out.
- `{ "method": "under", "dice": "1d100" }`: roll under. A check succeeds at or under the ability's number, as percentile games do.

Without `roll`, the app picks: a sheet counted in dice rolls a pool, a D&D-shaped sheet rolls the d20, and any other sheet adds its numbers to a die.

**When the AI runs the game**, the game master writes its own `[[roll:]]` lines from your ruleset, and the method decides how the app judges and reports them: a pool is rolled by Cortex rules, a `vs` number on a roll-under game succeeds at or under it, and the dice card says *Target* (or *Under*) rather than D&D's *DC*.

**When a player is the DM** (see Adventure mode), the roll box under a party member's attempt builds the check from that member's sheet: an ability and a DC for the d20; an ability and an optional target for "add"; one trait from each block, any extra dice and an optional target for a pool; an ability for roll-under. Every roll box also has a dice field: type any dice there to roll them instead. A roll with no target reports the number with no verdict, and then neither of the DM's outcome notes is sent.

**Cortex rules, as the app applies them to a pool:** every die is rolled; a die showing 1 is a *hitch* and counts for nothing; the best `keep` of the rest are added up; the largest die left over is the *effect die* (a d4 if none is left); and all 1s is a *botch*. When two dice show the same face, the smaller one goes into the total, which leaves the bigger free to be the effect die. The dice card shows the effect die and any hitches, and the game master reads them in the report line.

#### Step 5: Add your lore

The card's own lorebook carries everything the game master only needs when it comes up: factions, places, powers, equipment, notable characters. Each entry fires when its keywords appear. See Lorebooks.

- **Keep the rules out of the lorebook.** A distilled rulebook often keeps its whole system in one always-on entry. Move that text into the Ruleset box: the lore budget can trim a lorebook entry mid-session, and the ruleset is never trimmed.
- Give each entry the words a player would actually use. A list of twelve factions is twelve entries, each keyed to its own name, not one entry that is always on.
- Use always-on entries sparingly: they cost tokens on every turn.

#### Step 6: Write the opening and character creation

The greeting sets the scene, then offers the way in. These buttons only work in the opening:

```
...the job board is empty except for one sealed envelope with your name on it.
[[character: Make your character]]
[[starter_choice: Open the envelope]]
[[starter_choice: Ask the barkeep who left it]]
```

- `[[character: <label>]]` opens the character builder.
- `[[starter_choice: <action>]]` runs character creation first, then sends the choice. Use these for your main openings.
- A plain `[[choice: <action>]]` starts without a sheet.

**Add one dialogue example in your system.** Models copy examples far more reliably than they follow instructions. Show the game master stating the stakes, writing one roll line, the result arriving, and the narration that follows, including any stat changes. The easiest way to get the result line exactly right is to play one turn and copy the dice line the app wrote. A D&D one looks like this:

```
🎲 Stealth check: 1d20+2 → 16 + 2 = 18 vs DC 13 — SUCCESS
```

**Give the game its own look.** System messages, announcements, loot and boss intros can each be a document style on the Game Master card, drawn as a styled panel in the reply. Add a behavior rule saying when to use each one, and show one of each in your examples; see Document styles.

#### Step 7: Make ready-made characters

- **Build one in the builder.** In a chat with your Game Master card, open the builder, make the character, and press **Save as persona card**. The card keeps a copy of your sheet template.
- **Or make one by hand.** In the Characters library, make a persona card, tick **Adventure character sheet** under Adventure Mode Rulesets, and pick your game in the **Game** box above the Character sheet. That gives it your abilities to deal and your rules for HP and levels. (The Game box lists every Game Master card in your library that has a sheet template.)
- Players load them from **Load a saved adventure persona** at the top of the builder.

#### Step 8: Test it

1. Open a new chat with your Game Master card and start the builder. Check your blocks, your deal, your headings, and that HP, Level and XP are there or not as you set.
2. Press **Complete sheet for me** and check it writes under your headings.
3. Play three or four turns. The game master's rolls should use your dice and never a `mod` you forbade, and the dice card should read the way your game does.
4. Open the prompt inspector on a reply: your ruleset text should be in it, and the default "Checks are a d20 plus modifiers" should not. (The context meter is not that check: its ruleset row looks the same either way.)
5. Look at the character sheet panel: stats should land under the names your ruleset gives them.
6. If you want DM mode to work, add a character to a chat, turn on **I'm the DM**, and make one roll from the party panel.
7. Try a smaller model too. Where it drifts, add a dialogue example of the exact behavior; weaker models need more examples, not more rules.

#### Step 9: Share it

- **One card:** right-click it, Export, PNG or JSON. The ruleset, the sheet template and the card's own lorebook all go with it.
- **The whole game:** click the select button in the library toolbar, click your **Game Master card first**, then any persona cards, and press **Export bundle**. The first card you pick is the main card. Give the bundle a **Name**: it names the file and the folder the cards are imported into. Tick any shared lorebooks or document styles the game uses.
- **Write a read-me.** Put how to play in the Game Master card's **Public Creator Notes** (Markdown works). When someone imports a bundle of several cards, those notes open in an "About" window, and they stay on the card to read again later.
- Players need Rin Chat 0.7.6 or newer for sheet templates, dealt character building, Cortex pools and the About window.

#### Finished examples

Each is a Character sheet box and the core of a matching ruleset. Adjust names and numbers to your game.

**Fate Core style**: skills as ladder ratings, a skill pyramid, 4dF plus the skill.

```
{ "abilities": { "label": "Skills",
    "names": ["Athletics", "Burglary", "Contacts", "Crafts", "Deceive", "Drive", "Empathy", "Fight", "Investigate",
              "Lore", "Notice", "Physique", "Provoke", "Rapport", "Resources", "Shoot", "Stealth", "Will"],
    "deal": [4, 3, 3, 2, 2, 2, 1, 1, 1, 1] },
  "notes": ["High Concept", "Trouble", "Aspects", "Stunts"],
  "hp": false, "levels": false,
  "roll": { "method": "add", "dice": "4dF" } }
```

```
Every check is 4dF plus the skill. NEVER a d20, NEVER mod, NEVER adv or dis.
[[roll: 4dF vs 2 | Athletics | Clear the gap before the bridge goes: fail and you are hanging from it | bonus athletics]]
Targets: 0 Mediocre, 1 Average, 2 Fair, 3 Good, 4 Great. Beat it by 3 or more: success with style.
Track stress as [[stat: dm_<player>_stress = 1]] and consequences as text under dm_<player>_.
```

**Powered by the Apocalypse style**: five stats from -1 to +2, 2d6 plus the stat, read in bands with no target.

```
{ "abilities": { "label": "Stats", "names": ["Cool", "Hard", "Hot", "Sharp", "Weird"],
    "deal": [2, 1, 1, 0, -1] },
  "notes": ["Playbook", "Look", "Moves"],
  "hp": false, "levels": false,
  "roll": { "method": "add", "dice": "2d6" } }
```

```
A move is 2d6 plus the stat, with NO target. NEVER a d20, NEVER mod, NEVER "vs".
[[roll: 2d6 | Act Under Pressure | Get through the door before it seals | bonus cool]]
10+: they do it. 7-9: they do it, and you give them a hard choice or a cost. 6 or less: make a hard move.
```

**Cortex Prime style**: attributes and values rated as dice, pools of one of each, best two added up.

```
{ "abilities": { "label": "Attributes",
    "names": ["Agility", "Alertness", "Intelligence", "Strength", "Vitality", "Willpower"],
    "scale": "die", "deal": [10, 8, 8, 6, 6, 6] },
  "groups": [{ "label": "Values", "names": ["Duty", "Glory", "Justice", "Love", "Power", "Truth"],
    "scale": "die", "deal": [10, 8, 6, 6, 4, 4] }],
  "notes": ["Distinctions", "Specialties", "Signature Assets", "SFX"],
  "hp": false, "levels": false,
  "roll": { "method": "pool", "keep": 2 } }
```

```
Build a pool: exactly one Attribute and one Value, each a die of its size, plus a Distinction at d8
and a Specialty when one fits. Keep the best two. NEVER a d20, NEVER mod or bonus.
[[roll: (d10+d8+d8)kh2 vs 11 | Alertness + Justice + Ex-Cop | Spot the forger in the crowd: fail and he spots you]]
Targets: 3 very easy, 7 easy, 11 challenging, 15 hard, 19 very hard.
The report gives the effect die and any hitches. On a won conflict the loser takes stress of the effect die's
size. For each hitch, add a d6 complication ([[stat: dm_complication_<what> = d6]]).
```

**Percentile style** (as in Call of Cthulhu): skills from 1 to 99, roll 1d100 at or under the skill.

```
{ "abilities": { "label": "Skills",
    "names": ["Spot Hidden", "Listen", "Library Use", "Psychology", "Stealth", "Dodge", "Fighting", "Firearms"],
    "deal": [70, 60, 50, 50, 40, 40, 30, 30] },
  "groups": [{ "label": "Characteristics", "names": ["STR", "CON", "DEX", "INT", "POW", "APP"],
    "deal": [70, 60, 60, 50, 50, 40] }],
  "notes": ["Occupation", "Backstory", "Possessions"],
  "hp": 11, "levels": false,
  "roll": { "method": "under", "dice": "1d100" } }
```

```
Every check is 1d100 against the skill's value, and the target IS that value. NEVER bonus or mod.
[[roll: 1d100 vs 60 | Spot Hidden | Catch what moved behind the glass]]
The app judges at or under. Read the bands yourself: under half is a hard success, under a fifth is extreme.
```

**A D&D variant with your own ability names**: give only names, and the builder keeps 4d6 and point buy, and checks stay d20 plus modifier. No ruleset box is needed unless you change the rules.

```
{ "abilities": { "names": ["Might", "Grace", "Grit", "Wits", "Insight", "Presence"] },
  "notes": ["Background", "Bonds"] }
```

#### When it does not work

- **The builder still makes a D&D character.** The chat's own card has to be your Game Master card (in a group, the card the chat was opened with). Check the Character sheet box for an error line, and that the Card type is Adventure Mode Game Master.
- **The game master rolls d20s anyway.** Strengthen the dice clause in the ruleset (name the wrong dice), add the worked roll line, and add a dialogue example. Then check the prompt inspector to confirm your ruleset is the one being sent.
- **Rolls come out with strange modifiers**, such as a d12 trait adding +1. The game master used `mod`. Forbid it in the ruleset by name.
- **The DM's roll box offers a d20 and a DC.** The sheet is D&D-shaped (no deal, numbers) and has no `roll`. Add one.
- **A rating is a number of dice** (Forged in the Dark rolls one d6 per point and keeps the highest). There is no method for that; teach the game master to write `3d6kh1` for a rating of 3, and as DM type it in the roll box's dice field.
- **A persona from another game shows the wrong abilities.** Pick this game in its **Game** box in the card editor: abilities that do not fit are replaced, ready to deal again.
- **Old dice lines still say DC.** A roll keeps the wording it was made with. New rolls use your game's.
- **Exploding dice, or counting successes.** Not supported as such: see the dice list in Step 2.

## Choices & interactive replies

Any card can give the player buttons, inputs and other interactive bits in a reply — not just Adventure Mode cards. These are a Rin Chat feature written into the card's text (part of the AICC format); in other apps the lines just show as plain text. Each element goes on a line of its own. In the card editor, type `[[` in a greeting, example dialogue, system prompt or lorebook entry for a list of every element with a short note on each; pick one and it drops in ready to fill.

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
- A choice can roll one of the card's generators when it's picked: `[[choice: Draw a card | gen Tarot]]`. Stat changes and dice need an Adventure Mode card (Writing an adventure card).

**More interactive elements** — same rule, each on a line of its own, on any card:

- **Timed choice**: `[[choice: Freeze | timer 15]]` puts a countdown under the reply; if you don't pick in time, *that* choice is sent for you. The clock only starts once the choices are on screen, adds time for how long the reply is to read, and pauses while you type or with its Pause button. Don't want it? Settings → Chat Display → Timed choices.
- **Ask**: `[[ask: What do you name the dragon? | as dragon]]` shows a text box; your answer is sent as your message, and with `as` it's also saved, so the card can use `{{getvar::dragon}}` from then on.
- **Choices that remember**: `[[choice: Take the brass key | set has_key = true]]` saves a story variable when it's picked (`+=` and `-=` count up and down: `set coins -= 5`; several at once: `set a = 1, b = 2`). The model is told what changed, and the card can read it with `{{getvar::has_key}}`.
- **Lists**: `+=` and `-=` with a word keep a list, `set inventory += rope`: one item each time, no duplicates, any case, read back as `rope, lantern`. On a number they still add and subtract, and a word there changes nothing; a number added to a list joins it as an item. `if inventory has rope` checks for that whole item (`inventory has key` is false for a keycard), while `contains` matches any part of the text.
- **Choices you finish yourself**: `[[choice: Ask about the map | insert I catch up and ask about ]]` puts its text in your message box instead of sending it, so you can complete the sentence before you send. The button shows a pencil. Everything after `insert` is dropped in exactly as written.
- **Locked choices**: `[[choice: Open the chest | if has_key | hint You need a key]]` shows with a lock, and the hint, until the condition holds — then it opens by itself. Conditions: `has_key`, `!has_key`, `coins >= 5`, `door == open`, `inventory has rope`, joined with `&&` or `||`. Add `| hide` to keep a choice secret until it's available. On an Adventure card, a condition can read the character sheet too (`if gold >= 10`).
- **Choices that finish something**: `[[choice: Take the ember shard | set shard_ember = true | once]]` disappears once everything it sets with `=` already holds, so the story stops offering the kiln after the shard is taken. `+=` and `-=` don't count, so `set gold -= 2, bread += 1 | once` can be bought again. With `| if` too, it shows only while that condition holds and its sets aren't all done; when the condition fails it's hidden, not locked, so no hint shows. A set value with `&&`, `||` or a bracket turns `once` off. It never shows on the button.
- **Grouping a condition**: brackets decide what goes together, `[[choice: Bribe the guard | if has_coin && (charm >= 3 || knows_name)]]`, and `!` in front of a bracket flips the whole group: `if !(guard_awake || alarm)`. Without brackets, `&&` is read before `||`, as before. Brackets inside a value are just text (`title == Mr (Smith)`), and a bracket left open, a stray one or an empty `()` keeps the choice locked rather than guessing.
- **Choice tones**: `[[choice: Jump the gap | tone danger]]` gives the button a red edge and arrow. The tones are `danger` (red), `good` (green), `warn` (amber) and `quiet` (muted), in your theme's colors. It only changes the look, never the label, and works beside `if`, `hint`, `set` and the rest; an unknown tone word is ignored.
- **The story can change variables too**: a reply line `[[set: door = open]]` is applied by the app and never shown — for when the character hands something over or a door opens without a choice. Regenerating, swiping or deleting that reply puts the variables back. The line still counts when the model writes it in bold, as a bullet, with a period after it, or at the very end of a sentence; in the middle of a sentence it stays as text, and so does one in code formatting at the end of a sentence, which reads as an example. The same goes for a `[[lore:]]` line.
- **What the model sees**: older replies' choices, question boxes and forms stay on screen, but only the newest set is sent to the model, which saves tokens in a long chat. **Send only the newest buttons** in Settings → Memory & Context turns this off. `[[set:]]` lines and documents are always sent.
- **Tip for game cards**: build progress from what the *player* does, not from the model remembering a rule. A vault that opens on `if shard_ember && password == whiskers && shard_echo` — each part set by the player's own choice or answer — works however loosely the model plays along.
- **Pick several**: `[[pick 2: Sword | Shield | Bow | Lantern]]` lets you tick exactly two, then Confirm. Leave out the number to allow any amount. To ask a question above the boxes, put it first, in quotes: `[[pick: "What defines you?" | Strong | Agile | Sharp | as trait]]`.
- **Forms**: several questions answered at once, on one line: `[[form: Your character | Name: text | Age: number 18-99 | Class: Fighter, Mage, Rogue | Backstory: long text optional | Has a pet: checkbox | as hero]]`. The first part is the form's title; each part after it is a field, its label, a colon, then what kind of field: `text`, `long text`, `number` (with an optional range like `18-99`), a list of options separated by commas, or `checkbox`. Every field must be filled unless you write `optional` after its kind; Send points at whatever is missing. The answers arrive as your message, one "Label: answer" line each. With `as hero`, each answer is also saved as a variable named after its label: `hero_name`, `hero_age`, `hero_has_a_pet` (a checkbox saves yes or no), for `{{.hero_name}}` later. Forms are drawn by the app, so there are no password or file fields, and nothing a form holds ever leaves the chat.
- **A form as a character sheet**: on an Adventure Mode card, end the form with `as sheet` and the answers become your character's own stats, on the character sheet beside everything the builder and the game master put there: `[[form: Your hero | Class: Fighter, Mage, Rogue | Strength: number 3-18 | Agility: number 3-18 | as sheet]]`. You can edit them on the sheet like any other stat, and the game master reads them every turn. A character can also be built step by step: an ability the form leaves out, or every ability of a sheet left blank in the builder, is the game master's to choose with you in play. It records each one once, and from then on that score is yours to change, not the game master's. On an ordinary card, save the answers with any `as` name and have replies show them back as a sheet with a document block. The app fills the values in whenever the reply is displayed, so the sheet is always current. To have the model write it, put the block in the card's rules with each brace escaped, `\{\{.hero_name\}\}`: a plain `{{.hero_name}}` there is filled in before the model reads it, so it would copy today's value instead of the macro:

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
- **The model offers a button** (Adventure Mode) — the model ends a reply with `[[gen: HumanNPC | Draw a contestant]]` and the player gets a button that fires the generator on click (the label is optional). Unlike model tool-calling this needs no tool support, so it works on **every** model — and it's more reliable than asking the model to type a bare `[Create …]` itself, which weaker models tend to narrate as prose instead of emitting. Ideal for a spin, a draw, or a random encounter the player pulls.

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
