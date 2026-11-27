# AI Image Generation: In Chat, ComfyUI and Image Studio

> Read this on [rin.chat](https://rin.chat/guide/images/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

## Image generation in chats

With an **image provider** set up, the app can draw the story as you play it. Add one in Settings → Image Providers: NanoGPT, OpenAI, Grok, any OpenAI-style image API, an OpenRouter image model, or a local server — **Automatic1111 / Forge**, **SwarmUI** or **ComfyUI (your own workflow)**. Local setup has its own section: Local image servers. Keys stay on this device and are never exported.

**Image** in the row below the composer (or `/image`, `/img`, `/sd`) opens the subject picker: *the character*, *their face*, *your persona*, *the whole story*, *the last message*, *the last message verbatim*, and *the background*. Each one reads the conversation, has your *text* model write a prompt, and renders it with your image provider. You can also set a **shape** (portrait, square or landscape — blank uses whichever suits the subject) and add a line of steering (*"wide shot, night time, keep the jacket on"*).

To skip the AI entirely, type into the instructions box and press **My own prompt**, or type `/image a red bicycle` — either sends your text exactly as written. You can also jump straight to a subject with its id, e.g. `/image face`.

- **You see the prompt before it renders**: edit it, or press **Rewrite** with a note about what's wrong and the model writes a new one. Fixing a word is free; rerolling a wrong picture costs a generation. Turn the check off once you trust it: it's *Show the prompt before generating*, in the **When a picture gets drawn** card of Settings → Images.
- **"The whole story" reads further back than the others.** The gear beside it sets how many messages it takes in (12 by default, up to 100) and whether the chat's summary and memory tree come too. Every other subject always reads the last 12 and never the memory — deliberately, so a close-up of a face isn't pulled off by what happened an hour ago.
- **Rerolls sit beside each other** like message swipes — same prompt, new seed, every attempt kept and steppable with `‹ ›`. **Set as card art** promotes any picture to the character's card image (through a 2:3 crop step), and the gallery icon beside the Image button lists everything drawn in this chat — click one to jump to where it happened.
- **A consistent look** comes from three layers. The card carries an *image prompt prefix* ("female, green eyes, brown hair", or a LoRA tag) and a matching negative, and those travel with the card. A *common prompt prefix* and *negative prompt* in the **House style** card of Settings → Images apply to everything you generate. On top of both sits the **style preset** chosen as *Default style* on that same page; edit the presets with **Edit styles** there, in a card's image generator or with the pencil in Image Studio. A prompt you typed yourself gets none of the prefixes; the negative still applies. The pieces join in that order: style, common prefix, the card's prefix, then the subject. With *Include your persona's look* on, your persona card's image prefix follows the character's in pictures of your persona and of the whole story. The prompt dialog's *Negative prompt* box is added to all of those for that picture, and remembered for the next.
- **The model never sees the picture**: only its prompt, posted as the message's text. Generated images are left out of context by default; the eye icon in the message's hover actions, or *Let the character read generated images* in settings, puts that prompt back in so they can react to it.
- **"Send me a picture of yourself":** switch on *Generate when a message asks for a picture* and a request inside the story draws one after the reply. It's deliberately narrow — merely mentioning a photograph won't fire it — and it draws with no prompt dialog at all, even if the check is on, because being shown a prompt editor is what breaks the illusion.
- **The character sends one of its own:** switch on *Character can send pictures* (shown once you have an image provider) and the character is told it may add a picture to a reply by writing `[[picture: what it shows]]`. You never see the marker; when the reply finishes, the picture is drawn the same way as a requested one and lands under it, in the speaking character's look in a group. At most one per reply, only for a reply to a message you sent (a regenerate or swipe draws none), and off by default, because each picture costs a generation. *Wait between pictures* (60 seconds unless you change it; 0 means no wait) holds back one that comes too soon: the reply then says *Picture skipped*, and its **Draw it** draws that picture anyway.

**How prompts are worded** is a setting, because it belongs to the image model rather than to the picture. Flux, Chroma and SD3 are trained on sentences; SDXL, Pony and Illustrious descend from tag lists and read a sentence poorly. *Prompt style instruction* in Settings → Images has a one-click **Use tag style** for the second family. Image Studio's **Improve**, **Prompt from character** and **Describe a picture**, and the card art auto-prompt, follow it too unless *Use it beyond chat* is off. A prompt you type and send as it is never changes.

> **Tip:** Hosted image APIs (NanoGPT, OpenAI, Grok, OpenRouter) accept a prompt and little else — they ignore the negative prompt, the seed, and steps/CFG. Those reach the local backends (A1111/Forge, SwarmUI, ComfyUI) and, for the negative only, the NanoGPT models that list one, such as `bria-fibo` and `qwen-image`. The Negative prompt boxes appear only where it is sent. OpenRouter image models ignore the size too; the request asks for portrait framing in words instead. Some models also cap prompt length — `z-image-turbo` at about 1,200 characters, `hidream` at 3,000 — and your style prefix counts toward it, so a long preset can fail a short-looking prompt with a bare error.

Pictures are stored encrypted alongside your card art and are never deleted, including rerolls you stepped past — branching a chat shares them rather than copying them.

## Local image generation (ComfyUI)

Generating on your own GPU costs nothing per picture and keeps everything on your machine. Add any of these in Settings → Image Providers.

**Automatic1111 / Forge** — start it with the API enabled and point the Server URL at it, normally `http://localhost:7860`. The **Model** box lists the server's checkpoints; leave it blank to draw with whatever is loaded. For anime and realistic checkpoints, add two providers, one per checkpoint, and switch the active one, or give a Discord channel its own. A LoRA needs no switching: put `<lora:name:1>` and its trigger words in a prompt prefix.

**SwarmUI** — Server URL, normally `http://localhost:7801`. The **Model** box becomes a dropdown listed from the server (the names are long folder paths, so don't type them), and any **Preset** you saved in SwarmUI can be applied to every generation. A preset supplies its own sampler, LoRAs and numbers; a preset prompt containing `{prompt}` wraps yours, and the picture size always stays the one Rin Chat asked for.

**ComfyUI** runs *your own workflow*. Two small nodes let Rin Chat drive it, and nothing else about the graph changes — your samplers, LoRAs, upscalers and ControlNets are untouched, and the workflow still runs on its own in ComfyUI.

- Get the node pack from github.com/blueprintcoders/comfyui-rinchat and copy or clone the folder into ComfyUI's `custom_nodes` directory, so you end up with `ComfyUI/custom_nodes/comfyui-rinchat/`. Restart ComfyUI.
- In ComfyUI, add **Rin Chat · Input** (under the *Rin Chat* category) and wire the outputs you want: `positive` and `negative` into your text encoders, `width`/`height` into the latent, `seed` into the sampler. Unused outputs can dangle.
- Feed your finished image into **Rin Chat · Output**. It previews like any preview node and writes to ComfyUI's temp folder, so chat rerolls don't fill your output directory — add your own Save Image if you want copies.
- Press **Queue** once. That's the whole setup: Rin Chat adopts the last workflow you queued in ComfyUI that contains these nodes and, with one ComfyUI provider, re-adopts it whenever you edit and Queue again. No file exports.
- In Rin Chat, add the ComfyUI provider, point it at `http://localhost:8188`, and press **Test**. It tells you separately whether the server is reachable and whether the nodes are installed — two problems with completely different fixes.

**Several workflows**, say one per LoRA for different Discord channels: add a ComfyUI provider for each. Queue a workflow in ComfyUI, open its provider and press **Use last queued**, then do the next. Each provider keeps the workflow it took, and the provider list shows each one's LoRAs or checkpoint so you can see they differ. **Follow ComfyUI** on a provider makes it take whatever you queue most recently instead; it is on by itself when a provider is the only one on its server, and off when another shares it, since providers following one queue would all end up with the same workflow.

To pin one exact graph from a file, use **Load workflow** with a file saved by ComfyUI's **Save (API Format)**, not the ordinary save, which has no node ids to drive. The **Key** field matters if one workflow contains more than one Rin Chat Input, or to let several following providers share a server: give each workflow's nodes and its provider the same key. Leave it blank otherwise.

> **Tip:** **Steps and CFG belong to the model, not to us.** A distilled *turbo* model wants roughly **8 steps at CFG 1**; a standard one wants about 25 at CFG 6–7. Running the first at the second's numbers doesn't give a slightly worse picture, it gives a burnt one — which looks exactly like a broken integration. On ComfyUI, leaving both blank keeps whatever your workflow uses — but Automatic1111 has no such passthrough: blank sends **28 steps at CFG 6.5**, so set them deliberately there.

Running the server on another machine? Start ComfyUI with `--listen` so it accepts connections beyond localhost, use that machine's address instead of `localhost`, and check the port and firewall. A local render can take minutes; Rin Chat waits up to ten before giving up.

## Image Studio

A generator on its own: no character, no chat, just an image provider and what you type. It is a tab of **Tools** in the title bar, shown once an image provider is configured. A render keeps going while you are on another tab or screen. Press `Esc` to leave.

The screen has three parts: the **form** on the left, the **picture** you are looking at in the middle with everything that made it beside it, and the **gallery** of everything you've made on the right (under the picture on a narrow window).

#### The form

- **Image provider.** With more than one set up, the menu at the top switches between them. It is the same setting as Settings → Image Providers, so chats draw with your choice too.
- **Presets** save a set-up whole: the prompt *and* every dial, because they only mean anything together; a prompt that looks right at 8 steps and CFG 1 looks like mud at 30 and 7. Choosing one makes it active, and the save icon then updates it in place. Your last set-up is remembered whether you saved it or not. The button beside them, **Load settings from a picture**, fills the form from a PNG made in Rin Chat, A1111, Forge, SwarmUI or ComfyUI; dropping the PNG on the prompt does the same.
- **Prompt and negative prompt**, both expandable to full screen. The count beside the label is how many characters are sent, styles included, against what the chosen model takes; it turns red past it. Select words and press `Ctrl`+`↑` or `Ctrl`+`↓` to weight them as `(words:1.1)`; with nothing selected it weights the word under the cursor, and back at 1 the brackets come off.
- **Above the prompt:** **Improve** has your text AI turn a short idea such as "cozy cabin, night" into a detailed prompt you edit before using it (wildcards and LoRA tags are kept). **Describe** has it look at a picture (the one you're viewing, a file or a character's art) and write a prompt for one like it; it needs a text model that takes images. **Recent** brings back any of your last 20 prompts. **Tools** holds the rest, below.
- **Wildcards.** Write `{red|blonde|black} hair` and each picture picks one option at random. Tools, **Wildcards** saves named lists, one option per line, and `__name__` in a prompt picks one line of that list. A picture's details show the prompt it actually got.
- **Model**: a per-generation override, separate from the provider's configured model, so you can try one without changing your setup. ComfyUI has none; the workflow owns its checkpoint.
- **Start from a picture** (image to image) appears when the provider and model can work from an image: A1111 and Forge, SwarmUI, the NanoGPT and Alpha Tester models that support it, and OpenRouter models whose listing says they take an image as input. On OpenRouter the picture is a reference, so there is no strength to set. Drop or paste a picture, drag one in from the gallery, choose a file or use **A card's art**, then set **Strength**: lower keeps more of the picture, higher follows the prompt more.
- **Styles.** Prefix and suffix style presets, the same ones the chat and card generators use; the button beside them opens the editor. With a style active, a `Sends:` line shows the full prompt that will actually go out.
- **Size** as one-click buttons (Square, Portrait, Landscape, Tall, Wide) or exact width and height. Every preset is a multiple of 16 and near one megapixel, which is what current models are trained at; an unusual shape costs more quality than any sampler setting. **Swap** flips width and height, and **Save size** adds the current size as a button of your own.
- **Steps, CFG and Seed.** Blank steps and CFG mean "the provider decides", except on A1111 and Forge, where blank means 28 steps at CFG 6.5. Seed 0 or blank is random. The batch size renders up to eight per click; on ComfyUI that's one run of the workflow's batch, everywhere else a loop, and a fixed seed there walks forward one per picture, so you get variations rather than copies.
- **Local backends** (A1111, Forge, SwarmUI) list the server's own **LoRAs** (each with a weight), **samplers** and **schedulers**. A1111 and Forge also have **Hires fix**: draw the picture, then redraw it bigger for finer detail, with its own scale, upscaler, denoise and steps. ComfyUI's **Project** is free text handed to the Rin Chat Input node's `project` output, for filename prefixes or folder routing.
- **Sweep** (A1111, Forge, SwarmUI, ComfyUI) renders several seeds across a list of CFG and steps values in one go, such as 4 seeds × CFG 4, 6, 8, and opens the results in Compare as a grid.
- **Keep in All images**, at the foot of the form. On, every picture you make is stored with your other images, shows in **Library → All images** under Image Studio, and is waiting here next time; turning it on keeps the pictures already on screen. Off, a picture lasts only while the screen is open unless you star it, file it in a collection or write a note on it. **Settings in saved pictures**, under it, decides whether Save writes the prompt and settings into the file.

#### Generating and the queue

**Generate** (or `Ctrl`+`Enter` from anywhere on the screen) queues a run with the form as it is right then, so you can change the prompt and press it again while the first is still going. Waiting runs are listed under the button, each with its own cancel; **Stop all** ends everything. With A1111, Forge, SwarmUI or ComfyUI the picture shows as it renders, with the time left (ComfyUI needs `--preview-method auto` for the picture itself). With NanoGPT the price of the run shows beside the button. When the last picture lands while you're in another app, a notification says so.

#### Tools

- **Prompt from character** writes a prompt from any character's description and appearance with your text AI. Add *directions* like "full body, beach outfit" to steer what it pulls out, then edit the result before using it.
- **Expression sprites** makes a character's whole set of expressions from its card picture. Each line (joy, anger, sadness and the rest, with the words each adds to the prompt) becomes one image-to-image run on one shared seed, and is saved to the character as an expression as it lands, in the card editor's Expressions (see the card editor). A few words describing the character help keep it the same person. Only offered when the provider and model can start from a picture. To use a face you just made, set it as the card image first.
- **Generate from a list** takes prompts one per line and queues each as its own run with the current settings, styles and batch size, up to 100. Unlike a wildcard, every line runs once.
- **Wildcards** edits the named lists described above.

#### The picture and its details

Beside the picture: provider, model, styles, prompt, size, steps, CFG, seed, how long it took and when it was made. Click any line to copy it. A cropped or upscaled picture lists the one it came from, and the original lists what was made from it; click one to jump to it. Under the details:

- **A note** of your own. The gallery search finds it, and writing one keeps the picture.
- **Collection** files the picture under a name; the star keeps it as a favorite.
- **Save to Downloads** saves it with the prompt and settings inside the PNG the way A1111 writes them, so Civitai and A1111's PNG Info read them, plus Rin Chat's own copy so styles and LoRA weights come back exactly. JPEG and WebP are turned into PNG to carry them.
- **Regenerate** runs its exact settings and seed again; **Vary** runs them with a new seed; **Use these settings** loads them back into the form; **Reuse seed** and **Copy seed** do what they say.
- **Upscale**: 2× or 4× on this PC is free and uses your graphics card when it can (the first time downloads the upscaler); A1111, Forge and NanoGPT offer their own as well. The bigger copy lands in the gallery beside the original.
- **Use as** puts this picture somewhere: the **start image** for image to image, a character's **card image** or a persona's **avatar** (after a crop), one **expression sprite** under the name you give it, or the **Chats background**. After setting a background it offers to **match your colors** to it: buttons, links, your messages and selections take the picture's colors in Theme & Colors, and Undo puts yours back. A picture without strong colors isn't offered.

Right-click the picture or any thumbnail for Copy image, Save, Crop, Upscale 2×, Use these settings, Describe as a prompt, Remove from this list and Delete everywhere. **Crop** takes a standard shape or a free one and lands the result beside the original at full resolution; `Ctrl`+`Z` undoes it. With the picture in view, `S` saves it, `F` stars it, `Ctrl`+`C` copies it, `Delete` takes it off the list and `Shift`+`Delete` deletes it everywhere.

#### The gallery

- **Find** pictures by prompt or note, show only starred ones or one collection, limit them to today, the past week or month, sort newest, oldest or starred first, and pick small, medium or large thumbnails. It remembers all of that. `←` and `→` step through it.
- A thumbnail's **×** takes it off the list (it stays in All images); Shift-click or long-press deletes it everywhere. Drag a thumbnail into a folder, Discord or another app and it arrives as a named file.
- **Select** picks several at once to compare, star, file, save or delete together.
- **Compare** shows the picked pictures side by side, with the settings that differ under each and the shared ones listed once. With two, **Slider** lays one over the other with a divider to drag. **Save as one image** puts the comparison in a single PNG with the differences written under each, ready to post.
- **Manage collections** (beside the collection filter) renames, merges (give two the same name) or removes collections, and **Export** saves one as a ZIP with every picture's settings inside and a settings.json listing them.
