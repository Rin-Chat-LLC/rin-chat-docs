# Voice: Text to Speech and Dictation

> Read this on [rin.chat](https://rin.chat/guide/voice/), with screenshots. Generated from the guide in Rin Chat 1.0.4; changes made here are overwritten by the next release.

## Voice: text to speech & dictation

Chats can **talk** — text to speech reads messages aloud — and **listen**: dictation types what you say into the composer. One **voice provider** covers both directions and each direction picks its own backend, so you can read aloud with a bundled voice while a local whisper server does the listening. Add one in Settings → Voice Providers and mark it active; model and voice are picked on the provider itself.

**Reading aloud.** Every message has a speaker button in its hover actions — that works on its own, always. The speaker icon in the **chat header** is separate: it turns on *auto-read* for that one chat, so each new reply is spoken as it finishes. It's off by default and remembered per chat. While a message is speaking you get a pause/resume button beside it.

**Your own voice.** A persona has a Voice section like any character, and your messages are read in that voice. Turn on **Read my messages too** under Settings → Voice Providers and a chat with auto-read also speaks each message as you send it; the reply waits for it to finish.

- **Native (system voices, free)**: offline, no setup, no cost. On Windows the app reads every voice installed under **Settings → Time & Language → Speech → Manage voices**, not just the three Windows ships with.
- **Bundled Piper**: small neural voices running inside the app, ~123 of them across many languages. Clearly better than the system voices with no OS setup. First use downloads about 43 MB of engine plus a few MB per voice; after that it's offline.
- **Bundled Kokoro**: larger, more expressive, English only, 28 voices graded by the model's author. Pick a **compute device**: `Auto`, `GPU (WebGPU, ~330 MB model)` or `CPU (~90 MB model)`. The panel shows the GPU and CPU it detected. First use downloads the model once.
- **Bundled Pocket TTS**: Kyutai's Pocket TTS running inside the app on the processor, in English, German, Italian, Portuguese or Spanish, with eight built-in voices. Set the voice to *This character's reference clip* and each character with a clip in its card's **Voice** section speaks in that voice: about 10 seconds of clean speech, best part first. First use downloads about 200 MB per language, or about 490 MB with `Higher quality`.
- **Chatterbox (local voice cloning)**: speaks in any voice from a short clip of it, on your own graphics card. Desktop only, and it needs an NVIDIA card with at least 4 GB of memory. Setup is below.
- **OpenAI-compatible API / local server**: point the base URL at OpenAI, or at a local TTS server (Kokoro-FastAPI, openedai-speech, LM Studio) and leave the key blank. Long replies are cut at about 4,000 characters.
- **NanoGPT**: the model and voice boxes offer NanoGPT's own catalog with prices. Cut at about 3,000 characters per message.

> **Tip:** The first run of a bundled engine downloads tens of megabytes with only a status line to show for it. Press **Test voice** in the provider editor once and let it finish before you judge whether it works.

**Setting up Chatterbox.** Choose *Chatterbox* as the provider's read-aloud engine and a **Chatterbox (local voice cloning)** card appears on it. Nothing is downloaded until you press **Install**, which fetches Python, PyTorch with CUDA, Chatterbox and the model: roughly 11 to 12 GB of disk the first time. The card says if the drive looks too full. It is Resemble AI's MIT-licensed model; Rin Chat installs and runs it but can't support or guarantee it, and everything it says carries Resemble's inaudible watermark.

- **Variant**: *Original* (exaggeration and pacing controls), *Turbo* (the default and the fastest; no style controls, the clip sets the delivery) or *Flash* (exaggeration only, and no built-in voice, so every character needs a clip). A check marks the ones installed, and the line under it gives each one's graphics memory and speed.
- **Running it**: speaking a line starts Chatterbox if it's installed and stopped, so the Start button is optional. **Start Chatterbox with Rin Chat** starts it whenever the app opens. If it stops on its own, its output on the card says why.
- **Precision**: *Full (fp32)* is slightly clearer; *Half (bf16)* uses about 1 GB less graphics memory. It applies the next time Chatterbox starts.
- **Exaggeration** (0.5 is neutral) is how dramatic the read is, **Pacing** is the fix for speech that's too fast (lower is slower), and **Temperature** is how much one reading differs from the next (lower is more consistent). Only the ones the variant uses are shown.
- **Free graphics memory when idle** unloads the model after 5 to 60 minutes without speech (15 by default, or Off). The next line then waits while it loads again.

**Saved voices** (Pocket TTS and Chatterbox). A saved voice is a reference clip you can reuse: about 10 seconds of clean speech as WAV or MP3. Add one in the provider's *Saved voices* block, or save a clip from *Test a voice* or from a character's **Voice** section. Any character can then speak with it, picked in its own Voice section. **Use as default** makes one the voice for characters with no clip or saved voice of their own.

**Voices you trained yourself** (bundled Piper only). Open *Add a voice you trained* under the voice picker, choose *+ Add a voice you trained* in *Stored on this machine* and pick the `.onnx`. Two rules, both enforced when you add it:

- The filename must be `lang_REGION-name-quality` — e.g. `en_US-egirl-medium.onnx`, where quality is `x_low`, `low`, `medium` or `high`. That name becomes the voice's id.
- Its `.onnx.json` must sit in the same folder — that's the file written beside the model when you exported it, and the engine can't speak without it.

Your voices appear at the top of the picker. Re-adding the same id replaces it, so you can iterate on a checkpoint without restarting. If app storage is ever cleared the row says *files missing, re-add* — a voice you trained can't be re-downloaded.

**Dictation.** The mic button sits beside Send. The default **Built-in recognizer** is free and needs no key — on Windows it may need "Online speech recognition" enabled under **Settings → Privacy & security → Speech**. For better accuracy, point the **OpenAI-compatible** backend at a local whisper server (faster-whisper, Speaches) and leave the key blank, or use OpenAI/NanoGPT with a key. A **Language** hint like `en` or `ja` helps; blank is auto.

**On this computer (Whisper)** transcribes offline, inside the app: nothing is sent anywhere. Pick it as the dictation engine in Settings → Voice Providers. The model downloads once on first use (Tiny is smaller and faster, Base is more accurate), and the text arrives each time you pause. Use it when Windows' own dictation hears nothing (common on Windows 10) or on Linux, which has no built-in engine. A phone connected through Remote Access uses its browser's own dictation.

- The mic stops itself after about 30 seconds of silence, and pressing **Send** stops it too — whatever it heard is transcribed and sent rather than thrown away.
- Dictation is ignored while a message is being read aloud, so the mic can't transcribe the character's own voice back into your box.
- NanoGPT rejects recordings over 3 MB — keep takes short there.

The **Where voice is offered** card in Settings → Voice Providers holds two master switches: **Read aloud** hides every speaker button when off, **Dictation** hides the mic. Other controls worth knowing: **Speed**, **Test voice**, and **Collapse stretched words** — which turns `Joooohn!!` into `John!` and is forced on for the bundled engines, because they sound out every extra letter as noise.
