# Adventure Mode: AI Game Master, Dice and Character Sheets

> Read this on [rin.chat](https://rin.chat/guide/adventure-mode/), with screenshots. Generated from the guide in Rin Chat 1.0.4; changes made here are overwritten by the next release.

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

**The GM's hidden side.** Flip *Show DM stats (spoilers)* in the sheet's settings to see what the GM is tracking: enemy hp and states (`npc_`), world flags, hidden clocks and any condition it's put on you (`dm_`), plus the **campaign plan** — an overarching arc the app writes right after your first action, which every later reply steers by, and which also starts the GM off already tracking the people and clocks that matter. The plan is fully editable: rewrite it to change where the story is heading, or right-click a selection to rewrite that part with AI. Writing it takes a few seconds ("Crafting the scenario…") and **Stop** cancels it — the box stays there empty and writable either way, so you can simply type one. Deleting your way back to the start of the chat clears the plan so your next opening gets a fresh one.

**The GM tidies up too.** It can drop stats that are finished — a slain enemy's hp, a clock that ran out, an item used for good. It can only drop *its own* stats and consumables; your character's abilities, hp, level and XP are yours alone and the app refuses anything else. Gear that's confiscated rather than destroyed **moves to whoever took it** instead of vanishing, so it comes back intact when you get your pack off the warden (turn on spoilers if you want to watch it sitting there).

**Levelling** is the app's job, not the GM's: every 100 XP earns a level, and you get a dialog with the HP it adds and **two points to raise abilities** (up to 18) — spend them, or take *Skip the points* to bank the level and the HP without them. Earn two levels at once and you get both, and all four points, in one go. Your current HP rises by exactly what the maximum did, so levelling is never a free heal. It works off the sheet, so XP you type in by hand levels exactly like XP the GM awarded — but it needs a character: a sheet-less adventure never levels.

**House rules the app enforces:** healing can never push past your max HP; deleting, rerolling or swiping a message **reverses its stat changes** (with a "Sheet reverted" notice, so a refund is never silent); and at 0 hp you're never killed — the app puts you back at **5 hp**, weak but alive, and the GM narrates the setback. Turn on **Hard mode** in the sheet's settings and 0 hp is a real death: final, and the sheet won't let you edit that hp back up. Rests are house rules the GM is *taught* rather than rules the app enforces — a short rest heals a chunk, a long rest restores you to full — so a weaker model may play them loosely.

**Making your own?** See Writing an adventure card.

> **Tip:** Adventure chats work best with a model that follows instructions well — the app referees the mechanics, but the storytelling is all the model's.

#### Being the DM yourself

The same machinery runs backwards: **you** game-master, and the AI characters are the players. Open any chat that has a character in it, click the **dice button** in the sidebar, and turn on **I’m the DM**. No game-master card is needed — a one-on-one chat works (a party of one), and so does a group.

- **Everyone at the table gets a sheet.** The app stats each character up from their card — or reads the sheet the card already carries (card editor → *Metadata → Adventure character sheet*). Every number is yours to edit in the sidebar: abilities, HP, level, XP, inventory, and stats you invent yourself — HP and XP take `-6` or `+50` as well as a plain number. Award enough XP and a **level-up** button appears, which adds the level and its hit points and leaves the ability points to you.
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

**Meters the player can see.** A stat named `meter_<name>` with a number above 0 in `meter_<name>_max` shows as a labeled bar under the reply the whole time, fight or not: affection, sanity, heat, anything that fills up. It can't go past its max, and `dm_meter_` or `npc_meter_` ones stay hidden. The GM isn't taught meters, so the card says what to track in its own rules:

```
Track Kisa's affection in meter_kisa_affection, 0 to 10 (meter_kisa_affection_max = 10).
[[stat: meter_kisa_affection +1]]
```

**6. The campaign plan reads your card.** On the player's first action the app writes a hidden arc from your card's description, personality, behavior rules, setting, background and system prompt — but *not* from your greetings or dialogue examples (the opening scene is passed separately). If the true situation behind your opening lives only in a greeting, move it into the description or the setting so the planner can build on it.

**7. Ship it right.** Set the editor's **Card type** to *Adventure Mode Game Master* (this is what auto-enables the mode for whoever imports the card — it travels in JSON and PNG exports), tag it so it's findable, and use Creator notes to tell players what to expect (difficulty, themes, whether a character sheet matters).

> **Tip:** Test-play a few turns and watch the sheet: if stats drift or rolls come without stakes, add a dialogue example demonstrating the exact behavior you want — weaker models need more example coverage, not more rules.

#### Teaching it a different tabletop system

Everything above describes the rules the app teaches by default, which are roughly D&D's. If your game is Fate, Powered by the Apocalypse, Cortex, Forged in the Dark or something you wrote yourself, **a card can replace them.** An Adventure Mode Game Master card carries three boxes in the card editor's **Features** section: **Ruleset** (how a roll is read: what a target number means, modifiers, damage, rests), **GM style** (how the game is run: pacing, when to call for a roll at all) and **Extra GM guidance**, which is added on top and replaces nothing. A card's text beats the same boxes in Settings, so a game you share is self-contained: whoever opens it plays your system without changing a setting.

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
