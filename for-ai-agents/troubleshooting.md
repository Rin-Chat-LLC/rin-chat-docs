# Troubleshooting Rin Chat

> For AI models and tools that help someone set up Rin Chat or fix a problem with it. Generated from the app's own Guide on every release (this copy is version 1.0.4), so it matches what the app does. "Settings → …" names a place in the app's Settings.

- Ask which provider and model they use and the exact error text before guessing. Most problems are the provider's key, credit, model name or limits.
- Rin Chat stores chats and keys on the person's computer. Never ask them to paste an API key, password or recovery key into a chat.
- For anything not covered here, the full guide is https://rin.chat/updates/guide.md and support is https://rin.chat/contact.

## Getting started

Rin Chat is a local-first roleplay chat app. Your characters, chats and API keys are stored **encrypted on this device** and are never uploaded. What does leave your machine: the requests you send to *your* AI provider; anonymous usage counts (Usage statistics lists every field); a version check against `rin.chat` at launch; and — only when you use the feature that needs it — card downloads from aicharactercards.com (importing by ID or link); with a linked aicharactercards.com account and your yes, a daily check of the cards you got from there (card IDs and a fingerprint of each card's name, description, personality and scenario) for updates, ratings and reviews; voice runtime files from `rin.chat` and voice/embedding models from `huggingface.co` (first time you enable text-to-speech or vector memory), and the webpage you paste into the forge's attach-a-page. None of these carry your chats, cards or keys. You set up a local account with a password at first run; see Account & security.

**The app at a glance.** Everything is reached from the title bar: **Library** (your characters, and where chats start; its **Personas** row is who *you* are. Tabs along its top hold **Lorebooks** for reusable lore, **Generators**, **Document styles** and, with a linked account, **My listings**), **Tools** (tabs for **Card Forge**, which builds a character, a lorebook, a generator or a document style by talking it through, **Image Studio**, **Story Maker** and **Direct Chat**; hover either button to jump straight to a tab), and **Guide** (this page). To their right: the **☰** menu, the **gear** for Settings, a **phone** icon for reading chats on your phone, and a **search** button — `Ctrl`+`K` from anywhere.

**Basic and advanced settings.** Settings starts in Basic on a new install: the essentials for setting up a provider, chatting and making things look the way you like. Turn on **Show advanced settings**, under the search box at the top of the Settings list, to see everything, from sampler tuning to regex scripts. Search and Ask Rin always find every setting, and opening an advanced one turns advanced settings on. An advanced setting you changed stays visible in Basic, marked *changed*, so a tweak is never hidden.

▶ Take the app tour — the two-minute look around, whenever you want it.

**Three steps to your first chat:**

1. Add a provider in Settings → LLM Providers — an OpenAI-compatible endpoint (LM Studio, Ollama, KoboldCpp, OpenRouter, NanoGPT, etc.) with its URL and API key. Then pick your **model** in Settings → Generation Configs, which is where a provider and a model are paired up.
2. Go to the **Library**. Two characters are already there to try — **Jinx Wilder** and the **Adventure Mode Narrator**. Add your own by importing a card (drag in a `.json`/`.png`, or use Import) or creating one with **+ New**.
3. Open the character → **New chat** → type a message and press `Enter`.

> **Tip:** No provider yet? A local one like LM Studio or Ollama needs no API key — just point the URL at it and pick a model.

**Questions, bug reports, or just want to hang out?** Join the community Discord: discord.rin.chat.

## Troubleshooting & FAQ

The questions that come up most, and what to check first.

**I have a ChatGPT / Claude subscription — can I use that?** No. A consumer subscription only covers those companies' own apps. Rin Chat needs **API access**, which is billed separately: you sign up with a provider, create an **API key** (a long secret string you paste into Settings → LLM Providers), and pay per message instead of monthly. Providers that resell many models behind one key — OpenRouter, NanoGPT — are the easiest start. The alternative is free: run a model on your own machine with LM Studio, Ollama or KoboldCpp, where there is no key and no bill.

**Where is everything stored?** By default in `Documents/rin-chat/<your account name>/`. You can move it in Settings → Data & Backup.

**The model returns nothing, or only thinking.** A reasoning model spends its token budget thinking before it writes. Extra headroom is only added when you pick an explicit effort tier — on *Auto* there is none. Set an effort level in the config's *Reasoning* section, or raise **Max response tokens** — both in Settings → Generation Configs.

**The developer gave me a fix code.** Paste it into Settings → Diagnostics → **Apply a settings fix** and press **Check the fix**. A window lists every change beside what is set now; untick any you want to keep, then apply. **Undo** in the same place puts them all back. Only codes signed by Rin Chat's developer are accepted, and a fix can only change generation and chat display settings, never keys, providers or your data, so a code posted by anyone else will not work.

**The update offer comes back every time I open the app (Windows).** Rin Chat is installed in two folders. Updates go to the folder the installer last used, but your shortcut opens the other, older copy, so each launch is the old version asking to update again. The app says so in a strip at the top and lists every copy in Settings → Updates, with a button to show each one in its folder. Open Rin Chat from the updated copy and uninstall the others in Windows Settings → Apps, leaving *delete app data* unticked: your characters and chats are stored apart from the app and are not touched.

**Updating asks for administrator access (Windows).** Rin Chat is installed in a folder only an administrator can change, so Windows asks for approval when an update installs. Settings → Updates shows the folder and a **Fix permissions** button that gives your account permission to change that folder, after one administrator prompt, so later updates install without asking. In Program Files or another system folder, the prompt is expected and stays.

**Replies get cut off mid-sentence.** Raise *Max response tokens* in your generation config, or turn on *Continue truncated replies* (Settings → Automation, off by default), which carries on a reply that hit the technical token cap. It *only runs while Length is set to Auto* — with Short/Medium/Long, hitting the cap is treated as the limit you asked for. Either switch Length to Auto, or raise the cap.

**An error number came back:**

- **401 / 403**: the API key is missing, wrong, or lacks access to that model.
- **404**: the model id doesn't exist on that endpoint. The model field is free text; check the spelling against the provider's list.
- **402**: out of credit. A brand-new key with no money on the account returns this.
- **429**: rate-limited, or too many requests too quickly.
- **400**: often the prompt is longer than the model's context window; lower **Max context tokens** in Settings → Generation Configs.
- **Connection refused / failed to fetch**: a local server (LM Studio, Ollama, KoboldCpp) isn't running, or the URL/port is wrong.

> **Tip:** Use **Test** in Settings → LLM Providers, or ask the same question in Direct Chat — it sends no card, persona or lore, so a clean answer there means the problem is your card or settings, not the provider.

**My lorebook entry never triggers.** Keyword scanning reads only the **last 4 messages** by default — a keyword mentioned earlier has already scrolled out of the scan window. Give the entry a keyword that keeps recurring, raise that entry's own scan depth, or set it to always-on. Check what actually fired in **Session info → Lorebook**.

**Keyboard shortcuts do nothing.** Shortcuts with no modifier (`E`, `C`, `←`, `→`, `/`, `?`) are deliberately ignored while your cursor is in a text box, so typing can't fire them. Click outside the message box first.

**There's no Regenerate button on an older reply.** Regenerate, swipes and Continue act on the *last* message only — rewriting an older one would strand every reply written against it. Delete back to it (Shift-click Delete removes that message and everything after), or branch from it.

**Where is my data, and can I move it?** Everything lives in the app's data folder, which you can relocate in Settings → Data & Backup. To move to another computer, export a full backup there and restore it on the other machine — see Export, import & backup for what the file does and does not protect.

**I forgot my password.** There's no master reset — the vault key is derived from your password. Two things can get you back in, and both must exist *before* you're locked out: your **recovery kit**, or an **AICC account linked while unlocked**. With neither, the data cannot be decrypted by anyone, including us. Set one up now if you haven't: Account & security.

**My browser says the connection isn't private (phone remote).** Expected — the desktop serves HTTPS with a certificate it generated itself, which no public authority has signed. The traffic is still encrypted. Compare the fingerprint shown in Settings → Remote Access and continue. Phone remote access covers pairing problems.

**Something looks wrong / I want to report a bug.** Session info → Exchanges shows the exact request and response (your API key redacted) — that, and what you expected instead, is almost always enough to diagnose it.

## Providers & the LLM API

A **provider** is the AI that writes the replies. Rin Chat doesn't ship a model — you point it at any **OpenAI-compatible** chat-completions endpoint (a cloud service or a model running on your own machine). Manage them in Settings → LLM Providers.

**A provider is only the endpoint** — label, URL and key. *Which model to run there* (and its fallback, tool calling, context cap and reasoning) belongs to a generation config instead. So you set an API up once and point as many configs at it as you have models, rather than pasting the same key into a second provider just to change model.

**There is no “use this provider”.** You switch by making a *config* active — it carries its provider with it, so choosing *Fast local* or *Big model* switches endpoint, model and samplers in one move. Both screens can do it: **Use this one** on a config, in Settings → Generation Configs or beside the config on its provider’s page. The provider holding the active config is marked **Active** in the provider list.

**Add one:** Settings → LLM Providers → **+ Add provider**, pick a preset (or *OpenAI-compatible (custom)*), fill in the URL + key and save. A generation config is created alongside it and made active; pick your model there. Each provider row lists the configs using it.

**Common setups:**

- **Contest Prize API**: if your linked aicharactercards.com account won a contest prize, it appears under LLM Providers and in setup. Like the alpha proxy it needs no URL or key and offers a fixed list of models, for chat only (no images). The prize is a daily token allowance: the provider row and the line under the chat show what is left today, when it resets and when the prize ends. When the day's tokens are used up, replies say so plainly and stop until the reset.
- **LM Studio**: start its local server, URL `http://localhost:1234/v1`, no key. Load a model in LM Studio first so it appears in the list.
- **Ollama**: `http://localhost:11434/v1`, no key. Pull a model with `ollama pull <name>` first.
- **KoboldCpp**: `http://localhost:5001/v1`; llama.cpp, whatever host:port it prints, ending in `/v1`. No key for local.
- **Text Generation WebUI** (oobabooga) — start it with `--api --listen --listen-host 127.0.0.1`, URL `http://127.0.0.1:5000/v1`, no key. All three flags: with `--api` alone the server answers but refuses the app's requests, and you get “Failed to fetch”. `--public-api` is only for reaching it from another machine, and the address it prints changes every launch, so don't use that one here.
- **OpenRouter**: `https://openrouter.ai/api/v1`, key from openrouter.ai → Keys. Hundreds of hosted models.
- **NanoGPT**, **OpenAI**, **DeepSeek**, **Grok (xAI)**, **ofox** — pick the preset and paste your key; the URL is filled in for you.
- **Z.AI**: the same, and it has three models that are free to use. See *Free models on Z.AI* at the end of this section.

**The Local app card.** When a provider is LM Studio, Ollama or KoboldCpp (picked as its Type, or a custom URL on that app's usual port), its page in Settings → LLM Providers gets a **Local app** card that checks this computer and shows whether the app is installed and running. From there:

- **LM Studio**: Rin Chat doesn't install it, but once you've opened LM Studio once it can start and stop LM Studio's server for you, and start it whenever Rin Chat opens.
- **Ollama**: Rin Chat can install it for you, download models into it by name, and start it, or start it with Rin Chat. The card also warns when Ollama's context length is too short for most characters and says where to raise it.
- **KoboldCpp**: a single program with no installer. Where Rin Chat can manage it, it downloads it, runs it with a model you choose, and stops it when Rin Chat closes. Otherwise you run it yourself: pick a model file in its launcher, then click Launch.
- The card lists the app's models. **Use in Rin Chat** on a loaded one makes it the model of a generation config on this provider, and where the app allows it, **Load** and **Unload** are there too.

LM Studio, Ollama and KoboldCpp are third-party apps, so take problems with them to their makers. **See which models fit this computer** on the card opens the model finder, described in Diagnostics & usage.

**Starting from zero (no key yet)?** Two things trip almost everyone up. First: a ChatGPT or Claude *subscription* is not an API key — API access is a separate, pay-per-use account with the provider. Second: on credit-based services (OpenRouter, NanoGPT) a fresh key returns errors until you **add credit** to the account — do that before your first message, not after the 402. Costs are per-token and small at roleplay scale: on a mid-priced hosted model an evening of chatting is typically cents, not dollars — $5 of credit lasts most people weeks (big premium models cost more; the provider's pricing page has exact numbers). If you just want something that works well for roleplay without research, make an OpenRouter account, add a few dollars, and try a popular mid-size model from their roleplay rankings — you can switch models freely later, per chat, without touching the provider.

Signing up for NanoGPT? Our referral link, nano-gpt.com/r/ALQgPBCK, takes 5% off your API costs and helps support Rin Chat.

The preset only prefills the URL — every field stays editable, so a non-standard port or a self-hosted gateway is just a custom URL. A bare host gets `https://` added for you, and a remote `http://` address is upgraded to `https://`; `localhost` and LAN addresses (`192.168.x`, `10.x`, `172.16–31.x`) keep plain `http`. The orange line under the field tells you what will actually be used.

**Test** (in the provider form) asks the endpoint for its model list and reports what came back — the fastest way to tell a wrong URL from a wrong key. An endpoint that publishes no list still works; you just type the model id yourself.

**Choosing a model** happens in Settings → Generation Configs, on the config you are editing. There are two boxes. The top one is the **model id actually sent** — type any id the endpoint accepts. The **search box under it** browses the list fetched from the config's provider; picking an entry fills the id in. The list loads by itself and the ↻ button re-fetches and reports errors. An empty list isn't fatal — plenty of endpoints don't publish one, so just type the id. ⭐ marks models included in your NanoGPT subscription (no per-token cost).

**The rest of the config's model settings:**

- **Fallback model**: if the main model comes back *overloaded* (HTTP 429/5xx) before anything has streamed, the turn is retried *once* on this model, on the same provider. It doesn't cover bad keys, wrong URLs, or a failure mid-reply.
- **Test**: sends a tiny "say ok" to this exact provider + model and reports what came back. Proves the pair actually works before you chat with it.
- **Supports tool / function calling**: off by default. Turn it on and a card's *model-triggerable* generators are offered to the model as OpenAI `tools`. Without it those generators never fire. Some local servers reject the field, which is why it's opt-in.
- **Max context tokens**: how much history this model is sent. When the endpoint reports a window for the chosen model a **Use N** button offers it, and an empty field is filled in for you. Blank = no cap. Because the model lives on the config, this is per model rather than a ceiling you have to remember to keep under some other number.

**Reasoning / thinking** (the collapsed section under the model) is *per config*, because effort tiers, the wire format and the markers all differ by model and gateway:

- **Show thinking as a block** (on): folds the model's chain-of-thought into a *Thinking* block. Off does not stop the model thinking; the app just stops recognizing it, so the raw `<think>` tags show as text. Either way the thinking is **never sent back** to the model.
- **Effort**: *Auto (provider default)* sends nothing, so the provider's own default applies. Then *None (don't think)*, Minimum, Low, Medium, High and *Maximum (xhigh)*.
- **How to send it**, because gateways disagree on the field name:
  - *Auto (match the provider)*, the default: picks the shape this endpoint takes. Leave it here unless you've been told otherwise.
  - `reasoning_effort` (OpenAI, NanoGPT, Gemini, xAI).
  - `reasoning: { effort }` (OpenRouter, NanoGPT).
  - *Both*, for gateways that only read one of the two.
  - `thinking: { type }` (official DeepSeek API).
  - *Don't send (provider rejects the field)*: no reasoning field at all, for an endpoint that refuses requests carrying one.
- **Hide reasoning**: the model still thinks, but the provider is asked not to send the thoughts back. To stop it thinking at all, use Effort *None*.
- **"Don't think" tag** and **Also send `chat_template_kwargs`**: two more off-switches, for models that ignore `reasoning_effort` (see the tip in Prompting & steering).
- **Thinking prefix / suffix**: the markers used to split thinking out, default `<think>` / `</think>`. Leave the suffix blank and the block is treated as never-closing, which also stops the message editor splitting it into its own box.

**Learned fixes** get their own card right after the Reasoning section once a model has rejected something. Reasoning rows show the tier a model turned out to accept (or "no reasoning fields sent"); prompt-shape rows list models whose chat template only takes one system message (the app merges them there). The current config's model is listed first and marked, the rest fold away. **Clear** forgets it, and the next request tries your setting again — do that if a gateway changed upstream and the entry has gone stale. These are learned *per model* and shared by every config using that model, so one config's discovery spares the others the same rejected call.

> **Tip:** Keys are stored **encrypted in your vault** and redacted in the debug log and Session Info, so sharing a log never leaks them.

**The active generation config** — provider, model and samplers together — answers every chat, except where something explicitly overrides it: a group member, the impersonation model in Settings → System Prompts, the summary and embedding models in Settings → Memory & Context, the Card Forge, and Direct chat.

**Troubleshooting:**

- **Empty model list / "Failed to fetch":** the URL is unreachable — check the server is running and the address (including `/v1`) is exact. Press ↻ to see the actual error; the silent auto-fetch never shows one.
- **"Failed to fetch" while the server IS running (LM Studio and other local servers):** the server is blocking browser requests — turn on *Enable CORS* in its settings (LM Studio: Developer tab → server settings). The provider's Test button now says so explicitly when this is the cause.
- **401 / 403:** bad or missing API key.
- **404:** the base URL is missing its path — most endpoints need to end in `/v1`.
- **"Temporarily overloaded" (429/5xx):** the provider's problem, not yours. Wait, set a *fallback model*, or use the model switcher offered right under the error.
- **"No model selected for this provider":** the active config has no model id — set one in Settings → Generation Configs or from the model name in the chat topbar.

Tune sampling (temperature, penalties, response length, etc.) in Sampling & generation settings. Coming from SillyTavern? **Import from SillyTavern**, under the list in Settings → Generation Configs or System Prompts, turns a preset file into new configs and shows you what came across first.

#### Free models on Z.AI

Z.AI, the company behind the GLM models, offers three models that cost nothing to use: **GLM-4.7-Flash**, **GLM-4.5-Flash** and **GLM-4.6V-Flash**. Setting it up:

1. Sign up or log in at z.ai/model-api.
2. Create an API key on the API keys page. It has to be a Standard API key: a Coding Plan key only works on Z.AI's coding endpoint, not here.
3. In Settings → LLM Providers, click **+ Add provider**, set **Type** to **Z.AI** and paste the key. The URL is filled in for you.
4. A generation config is made with it, already set to GLM-4.7-Flash. To pick another model, open the config in Settings → Generation Configs; the free ones are marked **Free** in the model list.

**Which one to use:**

- **GLM-4.7-Flash** is the strongest of the three for chat. It thinks before it answers, so a reply starts a little later, and the thinking shows as a block you can open. Its config starts with room for a long reply, because the thinking counts toward the reply length.
- **GLM-4.5-Flash** is a bit faster, and it can run two requests at once where the others run one. That makes it a good pick for background work: in Settings → Memory & Context, under Chat memory, set **Model** to your Z.AI config with GLM-4.5-Flash as its model, so summaries don't wait behind your replies on GLM-4.7-Flash. Card Forge and Direct Chat have the same kind of model picker.
- **GLM-4.6V-Flash** can see pictures, so use it when you attach images. The other two can't; pictures in a chat running one of them are left out of what is sent.

**Limits.** Z.AI lets each account run only one request at a time on GLM-4.7-Flash and GLM-4.6V-Flash, and two on GLM-4.5-Flash. Rin Chat keeps to that for you: if a summary, a title or a Card Forge helper is already running on the same model, your next request waits its turn instead of failing, and your replies go ahead of background work. So a reply can take a few seconds longer to start right after a long one. If Z.AI still says it is busy, Rin Chat waits and tries again a few times before showing an error. The free models cost nothing to use; Z.AI's other models are paid, and choosing one without balance on your account shows an error saying so.

## Sampling & generation settings

Everything about *what* answers and *how* lives in Settings → Generation Configs, as named **configs** you keep several of and switch between. A config is a complete setup: its **provider**, its **model** (plus fallback, tool calling, context and reasoning), and the samplers below them — so switching config switches the model and its settings together, in one click, from the chat header. **A blank sampler is left off the request entirely**, so the server uses its own default and unsupported knobs are simply ignored — that's the right state for anything you aren't deliberately tuning.

**The two that matter most** (top of the panel):

- **Max response tokens** (default 400) — the hard ceiling on one reply.
- **Max context tokens** (with the model, above the samplers) — the history budget; oldest turns are trimmed to fit. It sits next to the model because it belongs to it: what one model can take says nothing about the next.

**The everyday samplers:** Temperature (default 0.9), Top P, Top K, Min P, Top A, Frequency / Presence / Repetition penalty, and Seed (`-1` or blank = a new random seed each time; set a number to reproduce a reply). Two of the penalties work differently:

- **Frequency penalty** (0 = off) pushes a word down more the more often it has already appeared, so it targets words the model leans on.
- **Presence penalty** (0 = off) pushes down any word that has appeared at all, however often, which nudges the model toward new words and topics.

Both run from -2 to 2; negative values do the opposite and encourage repeats. Small values are usually enough.

**Advanced samplers** sit in a collapsed group because most models and most gateways ignore them; they're mainly for local backends. Like every sampler, a blank one isn't sent. Each has its off value in its row:

- **Typical P** and **Tail-free (TFS)** (1 = off) trim unlikely words in different ways, each an alternative to Top P. Lower means safer word choices.
- **Smoothing factor** (0 = off) reshapes the odds around the likeliest word: low values even them out for more variety, higher values sharpen them toward the top choice.
- **DRY** ("don't repeat yourself") penalizes a phrase that repeats one already in the chat. *DRY multiplier* turns it on and sets its strength (0 = off), *DRY base* (around 1.75) is how fast the penalty grows with the length of the repeat, and *DRY allowed length* (around 2) is how long a repeat may run, in tokens, before it counts. The usual first thing to try against a model that loops on the same sentences.
- **XTC** ("exclude top choices") sometimes removes the most likely words, so the model reaches for less obvious ones. *XTC threshold* is how likely a word must be to count as a top choice, and *XTC probability* is how often the removal happens (either at 0 = off). For prose that has gone stale; too much and replies lose the thread.
- **Mirostat** steers toward a steady level of surprise instead of using fixed cutoffs. *Mirostat mode* is Off, Mirostat 1 or Mirostat 2; *Mirostat tau* (around 5) is the target, higher being more varied, and *Mirostat eta* (around 0.1) is how quickly it adjusts.

> **Tip:** Higher temperature = more varied/creative but less coherent. If replies drift or repeat, lower the temperature or add a small repetition/DRY penalty — not both at once, or you won't know which helped.

**Stop sequences** (one per line) halt generation the moment the model produces one. You can't type a real newline into a one-per-line box, so write `\n` and `\t` — they're converted for you.

```
\nUser:
        </s>
```

The **Other settings** card at the bottom of a config holds what the app may do with the model and what happens to a finished reply: **Tool / function calling** (see Providers), and the four below.

**CFG scale** and its **Negative prompt** are only sent when the scale is above 1, and only some local backends (KoboldCpp, text-generation-webui, TabbyAPI) implement them. **Trim incomplete sentences** drops a dangling half-sentence off the end of a finished reply — it's skipped for replies that fired a generator or ended on an adventure choices block, since those endings are already complete. **Reset this config** puts the samplers and dials back to their defaults, stop sequences included — your provider, model, context size, fallback and tool-calling setting stay as they are.

**Response length** — the *Length* dropdown in the reply-tools row above the composer: Auto, Short (~80 words), Medium (~240), Long (~650). It works by *asking* the model for that many words and to finish its sentence; the matching token cap is only a runaway guard. This control is per session and resets to **Auto** each time you reopen a chat — it isn't saved.

> **Tip:** **Replies coming back as thinking and nothing else?** The extra token budget for chain-of-thought is only added when you've actually picked a reasoning *effort tier* on the config. On *Auto* — the shipped default — a Short reply gets a small cap and a thinking model can spend all of it deliberating. Pick an effort (or *None*) in the config's *Reasoning* section in Settings → Generation Configs and the budget adjusts.

**Auto-continue** (*Continue truncated replies*, off by default, in Settings → Automation): when a reply stops because it hit the token limit rather than finishing, the app seamlessly resumes it — up to *Max continuations* (default 2). Two things to know: it needs a provider that reports `finish_reason` (most OpenAI-compatible APIs do), and it **only runs while Length is set to Auto** — with Short/Medium/Long, hitting the cap is the limit you asked for, so continuing past it would defeat the setting. If a Short reply cuts off mid-sentence, that's the trade; switch to Auto or raise the config's Max response tokens.

**Auto-swipe** (off by default, same panel): re-rolls a reply that looks bad — empty, shorter than *Minimum length* characters, or containing one of your *blacklist* phrases — up to *Max attempts* (default 3). Each attempt is kept as a swipe you can arrow back to. It judges only the text you'd actually read: a thinking block and hidden generator rolls don't count toward the length, and a blacklisted phrase the model merely *thought* won't trigger a re-roll.

```
Minimum length: 200
        Blacklist:  I cannot
                    As an AI
```

**Switching quickly:** click the model name in the chat topbar for **Model & generation** — provider, model, reasoning effort, generation preset and prompt config in one place. These are your **global** defaults, the same ones in Settings — not per-chat overrides — and they apply from the next reply onward. (For genuinely per-chat model changes, use a group member's override.)

**Reasoning models:** set how hard they think — including *don't think* — in the *Reasoning* section under this config's model. A model that refuses your setting is handled for you: the app reads which tiers the model says it accepts, uses the cheapest, remembers it for that model so the next request is a single call, and tells you once. Review or clear those in the *Learned fixes* card after the Reasoning section.

## Diagnostics & usage

Two pages for checking on the app itself: Settings → Diagnostics and Settings → Usage. Both are desktop only.

**Diagnostics** holds these cards:

- **Card health check**: the tips at the top of the card editor. *Show health check tips* turns them on or off, and *Dismissed tips* brings back the ones you hid.
- **Debug log**: records the last ~300 exchanges with your AI provider, plus errors, and expires them after 24 hours. *Export the log* writes a plain-text file that includes your prompts and chat content (keys redacted), so only share it with someone you trust. *Clear the log* discards everything recorded, and *Get help* opens the community Discord, which is the support channel. More in Export, import & backup.
- **Apply a settings fix**: for a fix code from the developer; see Troubleshooting & FAQ.
- **Models that fit this computer**: measures this computer's RAM and graphics memory, lists GGUF models on Hugging Face (search them with the box), and shows the largest version of each one that fits: *Fits on the graphics card*, *Partly in RAM, slower*, or *Too big*. Sizes include about 1.5 GB for the conversation, so treat the verdict as a guide. Rin Chat runs none of these itself; each row hands the model to an app that does.
- **Guided tours**: *Tour of the app* (the title bar, the library, lorebooks, and how providers and generation configs fit together) starts straight away; *Tour of the chat* starts the next time you open a chat.

On a row of the model finder: **Download in Ollama** while Ollama is running (otherwise **Copy Ollama command**), **Open in LM Studio**, **Open on Hugging Face**, and **Run with KoboldCpp** once Rin Chat has a KoboldCpp of its own, which downloads that file into your models folder. To set one of the apps up as a provider, see the Local app card in Providers & the LLM API.

**Usage** counts every model call made on this device: chats, Card Forge, summaries, titles and every other helper, including ones made with no chat open and those from a phone using remote access. Each chat's own count is under Statistics in Session info.

- **Totals**: requests, tokens sent and tokens received for today, the last 30 days and all time, plus what was served from or written to the provider's cache, and the cost where the provider reports one. Its line says whether the counts came from the provider or are estimates.
- **Last 30 days**: tokens sent and received per day.
- **By model**, **By provider** and **By purpose** (replies, swipes, chat memory, the story director, Card Forge and so on): the same numbers broken down.
- **History**: *Clear usage history* forgets every recorded request on this device. Chats keep their own counts.

## Updates

Rin Chat checks for a new version once when it starts, and shows a strip at the top if there is one. It **never installs or restarts by itself** — a surprise relaunch could interrupt a reply. **Later** dismisses the strip until next launch.

- **Check manually** with **Check for updates** in the **This version** card of Settings → Updates, which also shows the version installed now. Unlike the launch check it tells you when you're already up to date, and shows the release notes *before* you install.
- Installing downloads in the background and applies on the **next restart**. A **Restart now** button appears when it's ready.
- On Linux, a `.deb` install can't replace its own files, so the update is handed to your package manager behind the system password prompt. If your desktop has no way to ask for that password, the app points you at the download page instead of a button that can't work.
- If an update is rejected for a **bad signature**, that's the release, not your install — the app refuses to run a file that doesn't match what was published.

> **Tip:** **Only move forward.** Installing an *older* version can leave your vault in a mixed state — the storage layout and account format change between releases, and older versions can't read newer data. Keep your recovery kit saved before updating, and don't run an older installer.
