# Group Chats and Story Director

> Read this on [rin.chat](https://rin.chat/guide/group-chats/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

## Group chats

Add more characters to a chat with the group (people) button in the topbar. A group bar appears above the message box with a chip per member.

- **Pick who responds:** click a member's chip to have them answer now. **Shift-click** the chip to give that one reply directions first. Otherwise members follow the turn order.
- **Per-member settings:** the small gear on each chip sets that character's own provider, model and prompt preset — the gear lights up when it differs from the chat's.
- **☰ Order**: **who answers your messages**: *Smart* (whoever you name — name two or more and each answers in turn — else the best fit), *Natural* (as in SillyTavern: named members plus each member's **talkativeness** roll, so one message can get several replies; set it per member there), *List* (the rotation), *Pooled* (everyone once before anyone twice) or *Manual* (only who you click). Also the rotation itself and Auto mode's pause between turns. **Set next** jumps a member to the front; the arrows reorder the rotation. **Mute** keeps a member in the group but skips them whenever the app picks who speaks — the rotation, AI picks, Auto and the Director. Their chip dims; click it to have them speak anyway. At least one member always stays unmuted.
- **🎬 AI picks**: a director chooses who should speak next (whoever was addressed or would naturally respond). It overrides the turn order for that one turn only.
- **▶ Auto** lets the members talk among themselves for the number of turns in the box beside it (1–50), with a pause between turns (5 seconds unless you change it in ☰ Order). **⏹ Stop** ends it early and shows progress as it runs; so does starting to type.
- **New chat with this group** (in the group window) starts over with the same members, mutes, order and per-member models. Characters with *group-only greetings* open it with one, and so does a member you add before your first message.

> **Tip:** Regenerating a group reply re-rolls it as the *same* character who spoke it, using that member's own model and preset — not whoever happens to be next in the order.

## Story Director (auto-play)

The **Director** button (in the reply-tools row) hands the chat to an AI director that auto-plays it toward a storyline you set — steering the character(s) and, optionally, voicing your own character — for a fixed number of messages.

It works in two layers: on start a **planner** turns your storyline into a plot outline, then a **scene director** reads that outline plus the recent chat and writes the concrete next beat. Both calls show up in Session info → Exchanges.

**Setup:**

- **Storyline / direction**: describe where you want the scene to go (the arc, beats, and how it should end). The director advances it gradually rather than rushing to the finish, and starts wrapping up in the last couple of messages.
- **Messages to play**: how many messages to generate before it stops (1–100).
- **Reply length**: applies your chosen length preset to the generated turns.
- **Re-direct every N**: how often the director rethinks the next beat. 1 = before every message (most reactive); higher = fewer director calls and less reaction to what just happened.
- **Mode**: **Auto** plays straight through; **Step-by-step** stops before every message so you can edit the beat and the generated text before it's committed.
- **Delay between messages** (auto mode) — a reading pause in seconds after each message. 0 plays continuously.
- **Impersonate**: in a single chat the director voices your character (required to keep the story moving on its own). In a group chat this is a toggle: leave it off to only drive the characters, or on to have the director speak for you too. In groups it also picks who talks next each turn.
- **Advanced — director prompts**: the planner's and scene director's system prompts and user templates, with the placeholders each accepts. Edits save as you type; **Reset to defaults** puts them back.

**Before it starts** you get the drafted **plot outline** to read. Edit it, hit **↻ Regenerate** for a different one, or write your own — then **Accept & start**.

**While it runs** the **Story Director panel** sits on the right side of the chat (collapse it to a rail with the chevron; the rail still shows progress). It holds:

- **⏸ Pause**: stops after the current message and keeps your progress; **▶ Resume** continues. **⏹ End** closes the session. In step-by-step mode the button is **▶ Next beat** instead.
- The **direction box** — a live note to the director ("raise the stakes", "introduce a new character") that steers the upcoming turns without ending the run. Send an empty note to clear it.
- The **plot outline**, plus the current **next beat**. While paused the outline is editable — rewriting it is the strongest way to change where the rest of the run goes.
- During an auto-mode reading pause, a **Next in Ns** countdown with **Skip →**.

**Step-by-step mode** replaces those controls with a review for each beat: the director's stage direction and the generated message, both editable, with **↻ Regenerate** and **Accept**. Nothing is committed to the chat until you accept it.

Paused, you can type your own messages, regenerate, or edit as usual, then resume. When it reaches the message target it pauses on its own — set a number and hit **Continue +N** to add that many more messages and keep going (repeat as often as you like). Close the chat mid-run and the session comes back *paused* when you reopen it.

> **Tip:** Set a short message count for a quick guided beat, or a long one to let a whole scene play out while you watch. If a run keeps drifting, pause and edit the plot outline rather than fighting it with direction notes.

## Proactive messages

A character can **reach out on their own** when you've been quiet for a while — a message out of the blue, in their voice, with a notification. It's off by default and set up per character: open the card editor, then **Tools ▾ → Proactive messaging**.

It only runs **while Rin Chat is open**, and each message is a real request to your active AI provider — the same card, persona and lorebooks a normal reply gets. Nothing is generated while the app is closed, and nothing is queued up to arrive later.

**The settings:**

- **Reach out if I haven't messaged in**: the idle period. The clock runs from the latest of: when you switched this on, your last message to this character, and their last reach-out. Turning it on never fires immediately.
- **Randomize the timing by ±**: so it never lands at a predictable exact time. Set 0 for none.
- **Max messages per day**: resets at local midnight. Toggling the enable switch also resets today's count, so a capped-out character can resume straight away.
- **Stop after this many unanswered**: pauses outreach until you reply. Worth keeping: a long unanswered monologue is exactly what makes the messages get shorter and stranger. 0 = never stop.
- **Which chat**: either pick one of your existing chats, or leave it on *Start & reuse a new chat (no greeting)*, which creates a chat called **"<Name> - Messaging"** the first time it reaches out and keeps using it. That chat's name is fixed (no rename, no AI title) so you can always spot it — it's marked with a bell in your chat lists. Picking "Start & reuse" again abandons the current one (its history stays) and starts a fresh one next time.
- **First reach-out prompt**: optional flavour for the opening message ("you're feeling playful and miss talking to them").
- **When ignored / unanswered**: optional instructions for follow-ups. Left empty, the built-in behavior applies: pick the thread back up, bring something new, end with an easy hook, and don't guilt-trip.

**Quiet hours** are the one global guardrail — a From/Until window, in Settings → Automation, during which no character messages you. A message that comes due inside it waits for the window to end.

**When one arrives** you get an in-app toast and a desktop notification; clicking either opens the chat it landed in. The character's tile in your library gets a dot until you open that chat.

**Things worth knowing:**

- Nothing is sent into the chat you're looking at, or one that's mid-reply — but a chat left open in another tab can still receive one.
- Messages to the auto-created Messaging chat carry the tail of your most recent *real* conversation with that character as context, so they can reference what you last talked about.
- Changing the timing settings drops any pending schedule and recomputes it from the new numbers — "reach out in 1 minute" takes effect right away rather than waiting out the old delay.
- Only one character is served per check, so several due at once trickle in rather than arriving together.

> **Tip:** Try a short idle time (a few minutes) with a daily cap of 1 while you're tuning the prompts, then put it back to hours or days once it sounds right — every message costs a provider call.

## Scene tracker

An AI-maintained note of the **current scene** — location, time, who's present, mood — injected as an always-kept note so the model stays grounded even after older messages are trimmed away. It's per chat. Open it from the **Scene** tab of the Session-info inspector.

- **✨ Update from chat** asks the AI to rewrite it from recent messages; **Clear** empties it. You can also just type in the box — the note is sent to the model either way.
- **Rewrite automatically** keeps it current on its own, every N replies. It only kicks in *once a note exists*, so write one or press Update once to start it off.
- **Send it every … turns** controls how often the note is actually sent — 1 means every turn.
- Give it its **own provider and model**: this is a small, structured job a cheap fast model does well, and it saves paying your main model to restate who's in the room every few replies.
- The **scene tracking prompt** is editable, with a button to restore the built-in one.

All of it lives in Settings → Memory & Context → Scene tracker. Turning the tracker off hides the Scene tab and stops the note being sent.

```
Location: Rooftop garden, midnight
        Time: Late — the party is winding down below
        Present: {{char}}, {{user}}
        Mood: Tense, unspoken tension between them
```
