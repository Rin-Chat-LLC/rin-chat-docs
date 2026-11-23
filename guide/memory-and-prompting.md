# Memory, Context and Prompting

> Read this on [rin.chat](https://rin.chat/guide/memory-and-prompting/), with screenshots. Generated from the guide in Rin Chat 1.0.4; changes made here are overwritten by the next release.

## Memory & context

A model only sees a limited window of text. Rin gives you five separate tools for keeping what matters, and they stack. Everything below is configured in Settings → Memory & Context, except the context budget itself.

**The context budget** is `Max context tokens` on your generation config in Settings → Generation Configs — **8192** on a new config. If a provider declares its own ceiling, the smaller of the two wins. When the budget is full the *oldest chat turns* are dropped first; the newest message is always kept, and some room is held back for the reply that's coming. See exactly what's filling the window in the Session-info inspector's budget view.

> **Tip:** **Some blocks are never trimmed.** The character block, your persona, pinned facts, the chat summary, the scene note and the recall block are each sent as their own system message and always survive. They don't get dropped — they shrink the room left for actual conversation. That's why a chat with a big lorebook and a long summary can feel like it forgets quickly.

- **Pinned facts**: short statements injected into *every* turn. Add them in the Memory modal, or click **📌** on any message to pin its text (📌 turns orange; click again to unpin). Best for rules and relationships, not events.
- **Chat memory (summary)**: a rolling prose summary of the story so far, kept in its own system message. Write or edit it from the **Summarize** button in the chat's context bar. It **does not update on its own** unless you tick *Rewrite automatically* (off by default, then every **10** messages). You can point it at a cheaper model and rewrite the prompt it uses.
- **Scene tracker**: an AI-maintained note of where you are right now. On by default; see the **Scene tracker** section of this guide.
- **Drop first messages**: excludes the oldest N messages from what's sent while leaving them visible. Summarize first, or you lose them.
- **Recall past messages**: searches everything that has scrolled away. The rest of this section is about that.

#### Recall past messages (experimental)

Off by default. It needs **two** switches: the global one in Settings → Memory & Context, and a per-chat one in that chat's Memory drawer. Turning it on globally turns it on for every chat; any chat can opt out.

**It defeats prompt caching.** The injected block changes every turn, so a chat that currently gets cache hits will get slower and cost more. It earns its keep on long chats and small context windows, not on short ones.

**Only messages that have already scrolled out of context are indexed** — all of them, right up to the edge. If your whole chat still fits in the window, the index is empty — that's correct, not broken. The model can already read those turns. A section is only *summarized* once its messages have been out of context for a few turns: regenerating a reply changes its length, which moves the edge by a message or two, and waiting keeps that from costing summary calls. Until then those excerpts reach the model word for word.

#### The memory tree (on by default)

Older messages are grouped into verbatim *excerpts* (~700 characters, always whole messages). Every **4** complete excerpts become a *section*, which one model call summarizes. Once **2** sections exist, they're summarized again into a single *overview*. With the defaults that means **4 complete excerpts must scroll out of context before the first summary appears**, and 8 before the overview does.

Every turn, the overview plus each section summary — numbered `Part 3 of 11` so the model knows the order — is injected *regardless of what you're talking about*. That's the part that answers "what did we play at the beginning", a question containing none of its own subject for a search to find. Sections whose messages are all back in context are skipped. Then a similarity search pulls up to **3** matching excerpts back *word for word*, each labeled with the section it came from.

Summaries are written by the model you picked for **Chat memory**, one call per section, and each is saved as it completes — so a rate limit costs one call, not the run. Press **Vectorize all** again to continue. **While the tree is running for a chat, the rolling summary is neither sent nor updated** — the two would summarize the same messages twice.

Open the tree from the chat's Memory drawer to read the overview and every part, expand a part to see the exact excerpts it was written from, and **edit any summary by hand**. An edit sticks until the messages underneath it actually change.

- **Turn the tree off** and you get plain similarity search instead: messages are cut into 400-character chunks and the closest ones are injected wherever you choose (in the chat at a depth, or in the system block). No summaries, no model calls, no answer to "when did that happen".

#### Embeddings

- **In-app model** (default) — runs locally, free, private, works with every provider, downloads about 23 MB the first time.
- **Provider API**: calls `/v1/embeddings` on an OpenAI-compatible endpoint. Billed per token *separately* from a flat-rate chat subscription, and many endpoints don't offer it at all.

> **Tip:** **Changing the embedding source or model invalidates every stored vector.** Vectors from two models can't be compared, so old rows are ignored rather than silently mis-ranked. Press **Clear index**, then **Vectorize all**.

**Nothing being recalled?** Use **Test recall** in the Memory drawer. It shows the exact search text, how many chunks are searchable, and the top matches *including the ones the threshold rejected* — so "nothing similar" and "the threshold is too high" stop looking identical. Search by *subject* ("the gardening game"), not by question ("what did we do at the start?").

Branching a chat copies its whole memory across, so a branch never re-summarizes from scratch.

```
Pinned fact examples:
        - Anya is the user's younger sister and doesn't know about the heist.
        - The story is set in 1920s Chicago. Keep tech period-accurate.
        - {{char}} is secretly afraid of the dark.
```

## Prompting & steering

Shape how the model behaves, globally and per chat. The system-prompt side lives in Settings → System Prompts as named **prompt configs** — bundles you can keep one of per card, model or API.

> **Tip:** The **Default** config is read-only: it's the baseline, so it can't drift. Hit **Duplicate** to get an editable copy — that's the answer to "why is everything grayed out?". The one exception is the Author's Note, which you can edit on Default too, because it's your content.

- **Main system prompt**: the baseline instruction the model receives. Supports `{{char}}` / `{{user}}`. *Prefer a character's own system prompt* (on by default) lets a card that ships one override this.
- **Jailbreak prompt**: appended *after* the chat history every turn. **Empty by default**; most modern and open-source models don't need one. Only reach for it if you're getting refusals.
- **Author's Note**: free text injected near the end of the prompt every turn, at a *depth* in turns from the end (default 4) and a *role* (default system). A strong steer for tone or an upcoming beat. What you set here is the **default for new chats** on this config; each chat's own note (the sticky-note button in the chat) overrides it.
- **Context template** *(advanced)* — the "story string" that lays out the system block: `{{system}}`, `{{wiBefore}}`, `{{generalDescription}}`, `{{appearance}}`, `{{corePersonality}}`, `{{behaviorRules}}`, `{{speechStyle}}`, `{{backgroundHistory}}`, `{{worldSetting}}`, `{{dialogueExamples}}`, `{{wiAfter}}`, with `{{if x}}…{{/if}}` conditionals (comparisons, `&&`, `||`, and `{{.messageCount}}` for "drop this once the chat gets going"). Leave it blank to use the built-in layout. Your **persona is always its own system message** and can't be placed in the template.
- **Start reply with**: seeds every reply with text the model continues from, e.g. an opening quote to force a format. **Off by default**, and only applied to full replies, not to Continue or Impersonate. Some backends dislike a non-user final message — clear it if replies stop working.
- **Include names**: tags each turn with the speaker's name in the request (the OpenAI `name` field) rather than altering the text. Some weaker models keep speakers straighter this way. Ignored in group chats, which already label every turn.
- **Group chat prompt**: the system instruction telling the responder who else is present and to stay in their own character. `{{names}}` = participants, `{{char}}` = the responder. Empty sends nothing.

**The utility prompts** — one per button, all editable on a duplicated config:

- **Impersonation prompt**: what **Impersonate** (and `/impersonate`, and the Story Director writing your turns) is told. It can also point at its **own provider and model**: drafting your voice is a small job, so a fast non-thinking model here keeps it snappy without touching the chat's model. That picker only appears on an editable config.
- **Continue nudge**: appended when the **Continue** button extends the last reply.
- **Regenerate nudge**: prepended to your directions when you Shift-click **Regenerate**, pick a quick dial, or give a group member directions. Your text follows it.

**Quick dials** are the one-click buttons in that regenerate-directions box. The built-in ten are Shorter, Longer, More action, Slower, Spicier, Lighter, Funnier, Darker, More dialogue and Less repetitive. Edit them, reorder them, add your own, or bring the ten back with **Reset to defaults**, all in the **Regenerate quick prompts** card of Settings → Quick Actions. A dial with empty directions is hidden.

**Adventure Mode GM prompt** (a card on the prompt config, editable on a duplicate) is what Adventure Mode teaches the game master, in three layers. **Core** is the part the app's own parsers read, so it's always sent and can't be edited. **Ruleset** is how a roll is read (targets, modifiers, damage dice, rests, hp at zero) and **GM style** is how the game is run (when to roll, pacing, judging spoken words, using the campaign plan). **Load default text** puts the built-in wording in the box to edit, and **Reset to default** goes back to it. An adventure card can override either layer for itself; see Writing an adventure card.

**Response language** (its own card in Settings → System Prompts) is added to every AI request, whichever prompt config is active: chat, summaries, scene notes, chat titles, lorebook entries and Card Forge. Leave *Language* blank and the AI writes in the interface's language; fill it in (Japanese, 简体中文, Brazilian Portuguese) to use another. It covers lorebook keywords too, since an English keyword never fires in a chat written in another language. Turn off *Character replies too* to keep the roleplay itself in the card's own language while everything else follows the setting.

**Response length** and the auto-continue / auto-swipe behaviors are covered under Sampling & generation settings.

**Reasoning controls:** for thinking models, set the effort and how it's requested *per generation config*, in the *Reasoning* section under the config's model in Settings → Generation Configs (see Providers for each field). Chain-of-thought is folded into a collapsible *Thinking* block and is **never sent back** to the model, whichever way that block is displayed. Editing a reply that has one gives you separate boxes for the thinking and the reply; clearing the thinking box deletes the block. Whether the block starts expanded is a single global setting in Settings → Chat Display.

> **Tip:** **Model still thinks with effort set to None?** GLM 4.5+ and Qwen 3 ignore `reasoning_effort` entirely; whether they think is decided by their chat template. At effort *None* the app handles them itself: your last message gets the model's in-prompt tag (`/nothink` for GLM, `/no_think` for Qwen), and GLM, Qwen, Granite and Gemma models are also sent `chat_template_kwargs` turning thinking off. The config's *"Don't think" tag* only overrides the automatic tag; it's sent at effort *None*, and only ever to a GLM or Qwen model id. *Also send `chat_template_kwargs`* sends that flag at every effort tier, for another model that reads it; it needs a tier chosen (it isn't sent on *Auto*). The tag works even through gateways that drop unknown request fields. **DeepSeek has no in-prompt switch**: on the official API effort *None* sends `thinking: { type: "disabled" }`; on NanoGPT it sends `reasoning_effort: none`. Note DeepSeek V4 thinks by default, so *Auto* there means thinking ON.
