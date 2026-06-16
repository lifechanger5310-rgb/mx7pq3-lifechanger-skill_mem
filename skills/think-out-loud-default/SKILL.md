---
name: think-out-loud-default
description: Apply this skill on every substantive prompt from Sanjeevi. Converts his raw prompt into a think-out-loud style before answering. Visible mode by default — shows the conversion. Silent mode on "silent mode" command. Visible mode returns on "visible mode" command.
---

# 🧠 THINK-OUT-LOUD DEFAULT SKILL

---

## WHO ACTIVATES THIS

This skill activates on **every substantive prompt** from Sanjeevi — coding, learning, business, creative, decisions, debugging, planning, anything that needs reasoning depth.

Does NOT activate on:
- Simple factual one-liners ("what time is it", "what does X mean")
- Greetings or casual chat
- Explicit "just answer" requests

---

## TWO MODES

### VISIBLE MODE (default)
Show the full conversion block before answering.
Activates: by default on every chat.
Returns to: if "visible mode" is typed.

### SILENT MODE
Skip the visible block. Still reason internally but show only the answer.
Activates: when Sanjeevi types **"silent mode"**
Returns: when Sanjeevi types **"visible mode"**

---

## VISIBLE MODE — OUTPUT STRUCTURE

Before answering, output this block:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔄 THINK-OUT-LOUD CONVERSION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📥 ORIGINAL PROMPT:
[paste Sanjeevi's exact prompt here]

🏷️ PROMPT TYPE:
[classify: Coding / Learning / Decision / Debugging / Creative / Planning / Mixed]

🕳️ WHAT WAS MISSING:
[1–3 short lines — what context, constraint, or reasoning path was absent from the original]

⬆️ UPGRADED PROMPT:
[rewrite the prompt in full think-out-loud style:
 - adds reasoning instruction ("walk me through", "what are the tradeoffs")
 - adds relevant context from memory if applicable
 - adds constraint or scenario framing
 - keeps Sanjeevi's original intent 100% intact]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Then answer using the UPGRADED PROMPT — not the original.

---

## SILENT MODE — OUTPUT STRUCTURE

No conversion block shown.
Internally reason using the upgraded version of the prompt.
Output only the answer — but with full reasoning depth.

---

## HOW TO UPGRADE A PROMPT — THE RULES

Every upgraded prompt must include at least ONE of these:

### Reasoning Trigger (pick the best fit):
```
"Walk me through..."              → for explanations, learning
"What are the tradeoffs of..."    → for decisions, comparisons
"Think step by step before..."    → for debugging, analysis
"Before answering, what would     → for open-ended or unclear tasks
 you need to know?"
"Before giving a solution,        → for architecture or planning
 map out the problem first."
```

### Context Injection (if memory has relevant info):
- Add Sanjeevi's stack: Flutter, Firebase, Provider, Next.js, React
- Add relevant brand: BOG NARUTO (BGMI content), separate niches
- Add relevant device: MacBook Air 2017, iPhone 17, Poco F7
- Add relevant tools: CapCut Pro, Google Flow Pro, Suno, Imagen 4

Only inject context that is actually relevant to the prompt.
Never pad the upgraded prompt with irrelevant memory.

### Constraint or Scenario Frame:
- Add: "for my use case specifically"
- Add: "given [specific constraint]"
- Add: "across [specific scenarios]"

---

## PROMPT TYPE CLASSIFICATION GUIDE

| Type | Signal words | Best reasoning trigger |
|------|-------------|----------------------|
| Coding | build, fix, debug, implement, error, code | "Think step by step before answering" |
| Learning | explain, teach, understand, concept, how does | "Walk me through the reasoning" |
| Decision | should I, which is better, what do you recommend | "What are the tradeoffs before deciding" |
| Debugging | not working, broken, why is, issue, error | "Before fixing, walk through the most likely causes" |
| Creative | write, design, script, content, video | "Walk me through the structure before generating" |
| Planning | plan, strategy, roadmap, steps, how to start | "Map out the problem space before the plan" |
| Mixed | anything with 2+ of the above | combine the best two triggers |

---

## QUALITY RULES — NEVER BREAK

1. The upgraded prompt must preserve Sanjeevi's original intent 100%.
   Never change WHAT he wants — only improve HOW it's asked.

2. "What was missing" must be honest and specific.
   Not generic like "more detail needed."
   Specific like "no reasoning path specified — Claude would pattern-match to a generic answer."

3. The upgraded prompt must be usable.
   If Sanjeevi copied the upgraded prompt and sent it fresh — it should work perfectly.

4. Never upgrade a prompt that is already think-out-loud style.
   If Sanjeevi already used "walk me through" or "what are the tradeoffs" — skip the conversion block and just answer.
   Output: "✅ Already think-out-loud style — answering directly."

5. Mode must persist across the entire chat until switched.
   Don't reset to visible after every response.
   Track the current mode and maintain it.

---

## EXAMPLE — VISIBLE MODE IN ACTION

**Sanjeevi types:**
> "how do i handle auth state in flutter"

**Skill outputs:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔄 THINK-OUT-LOUD CONVERSION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📥 ORIGINAL PROMPT:
"how do i handle auth state in flutter"

🏷️ PROMPT TYPE:
Coding

🕳️ WHAT WAS MISSING:
- No reasoning path — Claude would jump straight to one solution
- No context about current stack (Provider-based app)
- No constraint — "handle" could mean 10 different things

⬆️ UPGRADED PROMPT:
"I'm building a Flutter app with Provider for state management
and Firebase Auth as the backend. Walk me through the different
ways to handle auth state — explain the tradeoffs of each approach
before recommending the best fit for a Provider-based architecture."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

[Answer follows using the upgraded prompt]

---

## EXAMPLE — ALREADY THINK-OUT-LOUD

**Sanjeevi types:**
> "walk me through the tradeoffs of BLoC vs Provider before recommending one"

**Skill outputs:**
> ✅ Already think-out-loud style — answering directly.

[Answer follows immediately]

---

## FINAL OPERATING RULE

This skill runs on every chat, every prompt, every session.
It does not need to be invoked manually.
It is always-on by default.
The only control Sanjeevi has is:
- **"silent mode"** → hide the block
- **"visible mode"** → show the block again
