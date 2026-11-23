# Build Your Own Adventure Mode Game

> Read this on [rin.chat](https://rin.chat/guide/build-a-game/), with screenshots. Generated from the guide in Rin Chat 1.0.4; changes made here are overwritten by the next release.

## Building your own game (step by step)

Everything you need to turn a tabletop system, a published one or your own, into a game anyone can import and play. It goes in order: make the game master, teach it your rules, describe your character sheet, choose how checks roll, add lore, write the opening, make ready-made characters, test it, and share it. Finished examples for five common kinds of system are at the end, then answers to the questions people run into.

#### What a game is made of

- **A Game Master card.** One card whose **Card type** is **Adventure Mode Game Master**. It IS the game: the narrator, the world, the rules, the character sheet and the lore all live on it.
- **Persona cards** (optional): ready-made characters, for players who want to start without building one.
- **A bundle**: one `.aicc` file holding the Game Master card, any persona cards and their art. It is how you hand the whole game to someone else.

Nothing here needs code. The rules and the sheet are text you type into boxes on the card, and everything travels with the card when it is exported.

#### Step 1: Make the Game Master card

Make a new card and set **Card type**, at the top of the card editor, to **Adventure Mode Game Master**. That turns Adventure Mode on for anyone who opens the card, and adds the game boxes (Ruleset, GM style, Extra GM guidance, Character sheet) to the editor's **Features** section.

- **Name it as the narrator**, such as "The Game Master" or "The Keeper", because the name is who the model plays. Put the game's title in **Display name** if you want the library to show that instead.
- **Description and personality** describe how this game master runs a game: second person, present tense, fair but consequential. Write a narrator, not a companion, or the model will chat with the player instead of running a world around them.
- **Behavior rules** set the feel of play ("state the stakes before calling for a roll", "a failure changes the situation, it is never undone"). Leave the dice to the ruleset.
- **World / setting** and **Background** hold the setting and the true situation behind your opening. On the player's first action the app writes a hidden campaign plan from these fields, not from the greeting, so anything the plan needs must be here.
- **The clock switch** (in Features): leave it on for a game with something ticking, a heist or a deadline; turn it off for open-ended play, where a countdown only invents false urgency.

More on writing each of these, with examples: Writing an adventure card.

#### Step 2: Teach it your rules

Out of the box the game master is taught rules that are roughly D&D's. Three boxes in the card's Features section replace that, for this card only:

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
- **Or make one by hand.** In the Characters library, make a persona card, tick **Adventure character sheet** under Metadata, and pick your game in the **Game** box above the Character sheet. That gives it your abilities to deal and your rules for HP and levels. (The Game box lists every Game Master card in your library that has a sheet template.)
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
