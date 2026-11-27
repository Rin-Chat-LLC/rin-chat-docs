# Rin Chat Discord Bot

> Read this on [rin.chat](https://rin.chat/guide/discord-bot/), with screenshots. Generated from the guide in Rin Chat 1.0.7; changes made here are overwritten by the next release.

## Discord bot

A character can answer in your Discord server. You make the bot in Discord, paste its token into Settings → Discord bot, and choose which channels it answers in and as whom. Rin Chat is the bot's brain: while the app is open and unlocked on this computer, the bot is online and replies are written with your active generation config, on your own provider and key, so they count toward your usage. Close the app, sign out or lock it, and the bot goes offline. Desktop only.

**Give the bot its own generation config.** By default it writes with whichever config is active. Pick one under *Generation config* in the bot's settings to give it its own model, provider and context size: the bot sends as much channel history as that config's Max context tokens allows, so a smaller one keeps the cost of each reply down in a busy server. The line under the setting shows that size, and warns when it is over 15,000 tokens: in a busy channel every reply then sends close to all of it. About 10,000 is plenty for Discord.

**You are responsible for what it posts.** Everything the bot says goes through your provider and your key. If the model writes something that breaks Discord's Terms of Service or Community Guidelines, or your provider's terms, your Discord account, bot or server can be restricted or banned, and you may be held responsible. Pick a character and a model you trust with that, and keep an eye on its channels. Rin Chat provides the bot as is, with no warranty, and takes no responsibility for what it posts.

**Setting it up**

1. In the Discord developer portal, create a **New Application**.
2. Under **Bot**, turn on **Message Content Intent**. Without it Discord refuses the connection, and the settings panel says so.
3. Choose **Reset Token**, copy the token and paste it into the panel. It is checked with Discord, then stored encrypted; the app never shows it again, only its last four characters.
4. Once it connects, the panel shows an **invite link** that asks for exactly seven permissions: View Channel, Send Messages and Read Message History to talk, Change Nickname and Manage Webhooks to look like the character, Embed Links and Attach Files to show expression pictures. Open it and add the bot to a server you manage. A bot invited before it asked for some of these can be given them by opening the link again.
5. Back in the panel, every text channel the bot can see is listed by server. Turn a channel on, pick the character it answers as, and when it answers.

**When it answers**, per channel:

- **When @mentioned**: only messages that mention the bot.
- **When @mentioned or replied to**: also a Discord reply to one of the bot's own messages.
- **Every message**: everything in the channel, except people replying to each other.

In the first two, the bot only sees the messages it answers, so a scene two people play between calls is lost to it. Turn on **Read every message** for the channel and it saves what everyone posts there, still answering only when called, so its next reply knows what happened in between. Those messages cost nothing when they arrive: no reply, no notes about people, no pictures. They only add to the history a reply sends, which your generation config's context size still caps. It is off unless you turn it on, because it keeps everything posted in that channel in the character's chat on this computer.

It never answers itself, other bots, webhooks or Discord's system messages; a post through one of its own channel webhooks counts as itself, and a reply to one counts as a reply to the bot. The **per-user cooldown** ignores a person's messages for that many seconds after one was answered, and the **reply length cap** limits how long a reply can be, on top of your generation config's own limit. **Pause** takes the bot offline without removing anything.

**Where the conversation lives.** Each channel gets an ordinary chat on its character, named like *Discord: #general (My Server)*, so you can read it in the library. Every message it answers, or every message with *Read every message* on, is saved with the name of the person who wrote it; an edit or delete in Discord changes the chat too. The model sees who said what, and `{{user}}` is whoever it is answering. The character's lorebooks, your global lorebooks, your system prompt and your regex scripts apply as usual, and generators roll as they do in a chat. The chat memory features do not: no summaries, memory tree, pins or recall for these chats, only the usual trimming when the history gets long. A reply is posted as plain text, without thinking or choice buttons, split into several messages past Discord's 2000-character limit. It never pings anyone, with one exception below: a channel with its own identity pings the one person it answers.

**Remembering people.** With *Remember people* on, a character keeps short notes about each person it talks to, and how it feels about them: warmer or cooler, fading back toward neutral over the following days. It writes these itself as it replies; you never see them in Discord. Only the person being answered is recalled, and each character remembers only what it learned. Anyone can ask the character in Discord to forget something about them, and it deletes the closest note. The **What characters remember about people** card lists the notes per character, where you can edit, add or delete them; turning the switch off stops new notes and hides the old ones from replies without deleting them.

**Expression pictures.** A character with expression images can put one under a reply when the moment calls for it, chosen by the character from its own expressions, at most two per channel every ten minutes. It needs the Embed Links and Attach Files permissions. Turn it off with *Expression pictures* in the bot's settings.

**How it looks.** The bot takes the character's name and picture, chosen per channel:

- **Server identity** (the default): the bot's nickname and picture in that server are set from the character, so it looks the same in every channel there. A bot has one face per server, so when several channels answer as different characters, the panel lets you pick which one the server shows.
- **Own identity**: the channel gets a webhook, and replies there are posted through it with that character's own name and picture, whatever the server identity is. A webhook post cannot be a Discord reply, so it starts with a mention of the person being answered, and that one person is the only one it can ping. Without the Manage Webhooks permission the channel says so in the panel and posts as the bot until it is given. There is no "is typing…" in these channels: Discord only shows typing for the bot itself, under its server name, not for the character.

Each name is the character's own (cut to Discord's limit) or a custom one you type: a server nickname can be 32 characters, a webhook name 80, and Discord refuses a webhook name containing "discord" or "clyde". The picture is the card's art, cropped square from the top. Changes are sent only when the character, its art or the name changes, because Discord limits how often a bot can rename itself; **Apply now** sends a server's names and pictures again and says whether they went through. The webhooks' secrets stay sealed on this computer like the bot token and are left out of backups.

**What people in Discord can do.** Talk to the character, and nothing else. Their messages reach the model as chat and nothing more: no tools, no image generation, no access to your settings, files or other chats. The phone remote cannot see or change the bot either. Discord bot replies show as their own purpose in Settings → Usage.
