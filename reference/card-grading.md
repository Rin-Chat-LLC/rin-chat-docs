# How character cards are graded

Source: https://rin.chat/card-grading/

This is the Markdown copy of the card grading rubric, for AI models and tools. Use it to check a card before it is graded; write cards with https://rin.chat/card-format.md.

Cards on aicharactercards.com get a letter grade from S to F, and Rin Chat can grade a card on your own machine with the same rubric and the same scale, so its grade predicts the site's. This page publishes that rubric: what each category rewards, the hard caps, the token penalty, the user agency rule, and the reasoning behind every choice.

The grade measures one thing: whether a card is structurally sound and ready to roleplay. It is not a measure of taste, and it never scores content.

Writing a card from scratch? Start with [The AICC card format guide](https://rin.chat/card-format/)

## What gets graded

Both graders read a card the way SillyTavern lays it out, as seven fields: `name`, `description`, `personality`, `scenario`, `first_mes`, `mes_example` and `alternate_greetings`. A native AICC card is folded into those fields first, exactly as a SillyTavern export folds it: `appearance` joins the description, behavior rules and speech style join the personality, and background joins the scenario. The [format guide](https://rin.chat/card-format#compat) shows the mapping.

Nothing else is scored. The system prompt, post-history instructions, depth prompt, lorebook, generators, regex scripts, images and metadata do not raise or lower a grade. Rin Chat shows whether a card has a system prompt, post-history instructions or a lorebook beside the grade, for information only.

The judge is a language model reading the card against the rubric below. On aicharactercards.com that model is GLM 4.7. In Rin Chat it is the model you choose.

## The scale

| Part | Points | Measures |
| --- | --- | --- |
| Field Appropriateness & Structure | 0 to 20 | Each field used for its own job. |
| Character Coherence & Identity | 0 to 20 | One consistent character across the fields. |
| Roleplay Utility & Immersion | 0 to 20 | Whether a user can start roleplaying right away. |
| Creativity & Originality | 0 to 20 | A fresh, clever concept, on a card that works. |
| Polish & Technical Clarity | 0 to 20 | Grammar, readability, formatting, information in the right place. |
| Alternate greetings bonus | 0 to 5 | Varied, clear roleplay entry points. |
| Token penalty | 0 or less | Permanent context that is too large. |
| **Total** | **out of 105** | The five categories, plus the bonus, minus the penalty. |

| Grade | Total | Meaning |
| --- | --- | --- |
| S | 90 and up | Elite: nearly flawless, immersive, usable and creatively excellent. |
| A | 80 to 89 | Excellent: fully functional and polished, ready to roleplay out of the box. |
| B | 60 to 79 | Good: a solid card with small structural or creative gaps. |
| C | 40 to 59 | Average: functional but missing polish, hooks or immersion. |
| D | 30 to 39 | Weak: incomplete or structurally flawed; needs editing to be usable. |
| F | Below 30 | Broken: non-functional or unusable for roleplay. |

> **B is the Verified Creator bar** On aicharactercards.com, B or better is the grade the Verified Creator program counts. A B card works well as it is; the climb from B to A is mostly polish, greeting variety and placing every sentence in the right field.

## The five categories

Each category is scored by the model's judgment, with hard caps for the failures that break a card no matter how good the rest is. A cap is a ceiling: a card that hits one cannot score above it in that category.

### 1. Field Appropriateness & Structure

- **Earns points:** every field doing its own job. The name is only the name; the description holds looks and background; the personality holds inner traits; the scenario sets the situation; the first message opens the roleplay; the examples show back-and-forth exchanges.
- **Loses points:** blank fields, and fields used for the wrong thing.
- **Cap:** several fields blank or misused: under 10.
- **Cap:** everything stuffed into one field: under 5.
- Short is fine. A brief field that does its job is not penalized; a missing or misused one is.

### 2. Character Coherence & Identity

- **Earns points:** the fields describe one person. Looks, traits, history and voice agree with each other, and the greeting sounds like the character the other fields describe.
- **Loses points:** contradictions between fields, a voice in the greeting that the personality does not support.
- **Cap:** missing scenario or first message: at most 8.

### 3. Roleplay Utility & Immersion

- **Earns points:** a user can start playing immediately. The first message or the alternate greetings establish context, tone, setting or relationship, and leave the user something to respond to.
- **Loses points:** a greeting that could open any card, missing key fields, and writing for the user (see [user agency](https://rin.chat/card-grading/#agency)).
- **Cap:** generic greetings or missing key fields: under 10.

### 4. Creativity & Originality

- **Earns points:** a concept that is unique or clever, with specific details that only this character has.
- **Loses points:** stock archetypes with nothing added. Creativity is rewarded only when structure and immersion are solid, so a brilliant idea in a broken card still scores low here.
- **Deduction:** 5 points if the character is named Elara.

### 5. Polish & Technical Clarity

- **Earns points:** clean grammar, readable text, consistent formatting.
- **Loses points:** typos, walls of text, inconsistent markup, and information placed in the wrong field.
- **Cap:** misplaced information: under 12.

### Bonus: alternate greetings

Up to 5 extra points when the alternate greetings give clear, varied entry points into roleplay: different situations, moods or stages of the relationship. A card with no alternate greetings gets no bonus, and loses nothing for it.

## The token penalty

Permanent tokens are the card text sent to the model on every turn of a chat: name, description, personality and scenario, as the grader sees them. Because a native card is folded first, appearance, behavior rules, speech style and background all count toward this figure. The greetings and example dialogue do not.

Rin Chat measures the card itself and applies the penalty in code:

| Measured (estimated tokens) | Penalty |
| --- | --- |
| Permanent tokens over 2,500 | -3 |
| Permanent tokens over 4,000 | -5 (instead of -3) |
| Permanent tokens over 6,000 | -8 (instead of -5) |
| Description alone over 1,500 | -1 more |
| First message over 1,200 | -1 more |
| Largest possible penalty | -10 |

On aicharactercards.com the token penalty is left to the grading model. The rubric has a place for it but sets no tiers, so whether a long card is penalized, and by how much, is the model's call there. Staying under the tiers above is the safest way to avoid it on both.

## User agency (puppeteering)

A card puppeteers when it writes the user's part for them. Both graders judge this in the first message and the alternate greetings only. Example dialogue is exempt, because it writes `{{user}}` turns by design. `{{user}}`, second-person 'you' and the user's persona all count as the same person.

The test is grammatical: who performs the action or has the feeling? When the user is the **subject** acting, speaking, deciding, thinking or feeling, that is puppeteering. When the user is the **object** of someone else's action, is being placed in the scene, or is told what they see or hear, it is not. When in doubt, the verdict is no puppeteering, and a penalty needs a quote in which the user is the one acting or feeling.

**Counts as puppeteering:**

> "You take her hand and follow her inside."
> "You nod." / "you say"
> "You blush" / "you can't help but smile"
> "{{user}} decides to stay."
> A `{{user}}:` line of dialogue in the greeting.
> Ending the greeting with the user having already replied.

**Does not count:**

> "She pulls you into the shadows."
> "The guard shoves you aside."
> "You find yourself at the edge of the market."
> "You step into the tavern." (an opening frame)
> "You hear music drifting from upstairs."
> "What do you think?" she asks.

| Verdict | What it looks like | Penalty (Roleplay Utility & Immersion) |
| --- | --- | --- |
| Minor | One or two small instances, such as a single emotional reaction ('you blush'). | 2 to 4 points off. |
| Heavy | Speaks for the user, makes their decisions, or recurs across several greetings. | Capped at 10. |

Rin Chat enforces this in code: any heavy verdict, or minor verdicts in two or more greetings, caps the category at 10, and a single minor verdict caps it at 18. It also checks the quoted evidence, and drops a verdict whose quote only shows the user as the object of an action or in an opening frame. On aicharactercards.com the same rule is applied by the grading model, for grades made from November 14, 2026 on; earlier grades were not re-run under it.

## Why the rubric works this way

### Structure is weighted equally with creativity

A model performs a character from the text it is given. If looks, traits and rules are tangled together, it has to guess which sentence is which, and it guesses differently from reply to reply. A well-structured card is one the model can read consistently, which is what makes the character feel the same on turn fifty as on turn one. An original idea that the model cannot read reliably does not survive a long chat, so structure and originality carry the same weight.

### Brevity is not penalized, misplacement is

A short field costs nothing if it does its job, and a lean card leaves more room in the context for the actual conversation. Misplaced text does real harm: a backstory in the personality field reads as a trait, a rule in the description reads as flavor, and a description in the greeting gets copied into every reply. The rubric rewards the right text in the right place, not more text.

### The grade is content-neutral

SFW or NSFW, gentle or dark, ethical or not: none of it is scored. A grade answers whether a card works for roleplay, and a dark card can be as well made as a wholesome one. Mixing taste or moderation into the number would make it mean two things at once and be useful for neither. Site rules are enforced by a separate moderation process, not by the grade.

### Puppeteering is penalized

The opening message is the first and most direct example of a reply that the model sees, and models imitate it. A greeting that writes what the user does or feels teaches the model that writing the user's turn is part of the job, and it will keep doing it. That takes the one thing roleplay depends on away from the user. Scene-setting stays allowed because placing the user somewhere is how an opening works.

### Tokens matter

Permanent text is paid for on every single reply, for the whole length of the chat. A bloated card fills context that the conversation needs, so the chat forgets its own history sooner. It also dilutes the card itself: when a few key traits sit among thousands of words of detail, the model gives each one less weight. The penalty pushes situational detail into a lorebook, where it costs tokens only when the scene needs it.

### Alternate greetings earn a bonus

Varied greetings make one card replayable: different openings, different moods, different points in a relationship. Each one is also another demonstration of the character's voice in action. Because they cost nothing on later turns, more good greetings is an improvement with no downside, so they add points rather than being required.

### The Elara deduction

Elara is overused in the community. It is a name models tend to default to when asked to invent a character, so on a card it often signals a name nobody chose. The deduction is small and specific: it nudges authors toward a name they chose rather than one a model defaulted to.

### Caps exist

Without caps, a card with brilliant prose and no first message could still average its way to a good grade, and a user would download it and find it does not start. Caps make sure a structural failure shows in the letter. A strong category cannot hide a broken card.

## Where the two graders differ

Rin Chat uses the site's rubric and scale, so its grade predicts the site's. The difference is that Rin Chat measures what code can measure and enforces the caps itself, while the site leaves every rule to the grading model.

|  | aicharactercards.com | Rin Chat |
| --- | --- | --- |
| Grading model | GLM 4.7 | The model you pick in Settings, Card Forge, Card grading |
| Blank fields and one-field stuffing caps | Applied by the model | Measured and enforced in code |
| Missing scenario or first message (coherence at most 8) | Applied by the model | Enforced in code |
| Missing first message or description | Applied by the model (utility under 10) | Enforced in code (utility at most 9) |
| Misused fields, misplaced information, generic greetings | Judged by the model | Judged by the model |
| Elara deduction | Applied by the model | Applied in code |
| Token penalty | Left to the model, no tiers | Code tiers, shown above |
| Bonus with no alternate greetings | Up to the model | Always 0 |
| Puppeteering | Greetings only, subject versus object test, applied by the model | Same rule; evidence checked in code; two minor verdicts count as heavy; one minor caps utility at 18 |
| Personality notes in the description | The field guide asks the description to avoid in-depth personality | Some personality in the description is not treated as a fault on its own |
| What you get back | A total and one paragraph of feedback | Per-category notes, specific findings with suggested fixes, and a verdict for each greeting |

## Limits of a grade

- **It is model-judged.** Different models grade differently, and the same model can score the same card differently on two runs. A grade from another model in Rin Chat is a prediction of the site's GLM 4.7 grade, not a copy of it.
- **Rin Chat's grade is steadier,** because its main caps, the Elara deduction and the token penalty come from code rather than from the model's reading. A card that breaks one of those rules lands under its cap every time, whichever model grades it.
- **It measures structure and roleplay readiness,** not whether you will like the character. A B card you love is still a card you love.
- **Writing to the rubric reads flat.** A card tuned to tick boxes, with every field filled in by formula, tends to lose the voice that makes people want to talk to it. Write for play first; the rubric mostly rewards what good play needs anyway.

### Getting a grade

- On **aicharactercards.com**, cards published there are graded by the site.
- In **Rin Chat**, use the award button in Card Forge, or Tools, Grade card in the card editor. Persona cards have no grade.

## How to raise your grade

- Fill the core fields: description, personality, scenario, a first message and at least one example exchange. Blank fields trigger the hardest caps.
- Put each sentence where it belongs: looks in appearance or description, traits in personality, the situation in the scenario. In an AICC card, the separate fields do this sorting for you.
- Make the first message a specific scene that shows the character's voice and leaves the user an opening.
- Read every greeting for the user as the subject. Rewrite any line where they act, speak, decide or feel.
- Add two or more alternate greetings, each a different situation.
- Keep permanent text under about 2,500 tokens. Move places, factions and side characters into lorebook entries.
- Give the character a specific, chosen name and at least one detail no other card has.
- Proofread, and keep formatting consistent between the greetings and the examples.
- Grade it in Rin Chat, fix the findings it lists, and grade again.
