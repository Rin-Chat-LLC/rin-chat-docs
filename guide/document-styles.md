# Document Styles and Chat Styles

> Read this on [rin.chat](https://rin.chat/guide/document-styles/), with screenshots. Generated from the guide in Rin Chat 1.0.4; changes made here are overwritten by the next release.

## Document styles

A document style decides how a `[[doc: name]]` block looks in a reply: a letter, a phone screen, a game's system message, a loot box. Thirty-one come built in: `paper`, `newspaper`, `terminal` and `sms`, plus twenty-seven that also tell the AI when they fit: `diary-page`, `email-message`, `social-thread`, `classified-file`, `comm-link`, `tarot-card`, `stream-chat`, `phone-messages`, `departure-board`, `radar-sweep`, `neon-sign`, `glitch-broadcast`, `spellbook`, `matrix-terminal`, `rpg-dialog`, `vinyl-record`, `data-cube`, `ransom-note`, `potion-vial`, `aurora-sky`, `stock-ticker`, `heartbeat-monitor`, `cursed-scroll`, `sealed-letter`, `ouija-board` and `boss-encounter`. You can make your own from a little HTML and CSS. The look is drawn by the app; the AI only writes the block and its text.

#### Making one

- **Yours, for every chat**: the Library's Document styles tab. Each style shows as a tile with its live sample where a card's picture would be; **Edit** opens it, and the folder sidebar sorts them the same way as shared lorebooks (drag a tile onto a folder, or use the folder button on its corner). Built-in styles are listed under **All styles**, grouped by kind; **View** opens one read-only, to see how it's made, and **Duplicate** makes it yours. A folder only sorts the page: a style works the same wherever it is filed.
- **A card's own**: the card editor's **Document styles** section. These travel with the card when it's exported or shared and work in its chats, with nothing for the player to add.
- Pick a built-in and press **Duplicate** to start from its real template, or **New**. Each style has a **Name** (up to 30 lowercase letters, numbers and dashes: it's what you write in the block), its **HTML**, an optional **Line HTML** for message threads, and its **CSS**. **Tell the AI when to use it** is an optional line the AI reads beside the name (see below). **How to use it** shows the block to copy.
- **Add a starter** adds one of eight ready-made styles: a notice board, a letter, a status window in your theme's colors, a quest log that changes color for `New:`, `Complete:` and `Failed:` titles, a chat log, an animated alert, an achievement banner (a gold, silver or bronze medal from its `Tier:` field, and a `Reward:` box that hides when empty) and an item card colored by its `Rarity:`, common to legendary, with a `Type:` label. Each is an ordinary copy you can edit, with its use line already written. To see a technique in a finished style, add a starter and read its CSS.
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

A character card can bring its own chat style: a look for its chat window, from the message bubbles, names and avatars to the choices, dice rolls, the Adventure Mode HUD, the message box and every color. It applies only while that card's chat is open, and only inside the chat window: the top bar, sidebars, Settings, menus and pop-ups always keep the app's look. A chat style can't load anything from the web or run any code, and the AI never sees it, so it costs no tokens.

Together with document styles and Adventure Mode, it lets a card feel like its own game or story: a candlelit letter, a starship console, a retro RPG.

#### Turning them off

- **Every card**: Settings → Chat Display → Card chat styles. On by default.
- **One card**: hover the **Session info** button at the top of its chat and choose **Turn off this card's chat style** (the same line turns it back on). On a phone, open **Chat options** (the **…** at the top of the chat) and flip **Card chat style**. It stays off for that card in every chat, and nothing in the card changes.
- Both switches sit outside the chat window, so a style can never hide them.
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
- **The story can change the look**: every chat variable the story sets is on the window, a number as `var(--rc-var-gold)` and a word as `:scope[data-rc-scene="night"]`. In Adventure Mode, `:scope[data-rc-combat]` is on during a fight and `:scope[data-rc-hp]` is `ok`, `hurt`, `low` or `down`. **Try a state** above the preview shows it.
- **One message, its own look**: write rules for `.rc-message[data-rc-look="whisper"]` and the character can mark a reply `[[look: whisper]]` to wear it. The chat tells the character which looks your style has, and the mark never shows. You can mark your own messages too.
- **Light and dark**: either leave the theme's colors alone, or redefine the whole set (grounds, reply bubble, text, borders and accents) so the chat reads the same in both. Check both in the preview.
- **Motion**: keep it short, only on the newest reply (`.rc-newest`), and inside `@media (prefers-reduced-motion: no-preference)`, so readers who turned motion off get none.
- **Document blocks** inside replies keep their own document style: leave `.rc-doc` to it, and use one palette for both.
