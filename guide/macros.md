# Macros and Regex Scripts

> Read this on [rin.chat](https://rin.chat/guide/macros/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

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
