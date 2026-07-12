---
name: dola-prompt-guideline-troubleshooter
description: "Use this skill whenever a Dola AI / AI video (Seedance, Flow, etc.) prompt or image generation FAILS, gets flagged, returns a guideline/policy error, silently burns credits with no output, or produces output that doesn't match intent. Trigger on phrases like 'the video couldn't be generated,' 'no points were used,' 'guideline issue,' 'flagged,' 'rejected,' 'blocked,' 'policy violation,' or when the user wants to rewrite a prompt / recreate an image to get the same creative result without tripping the same filter again. Also trigger proactively before submitting a prompt/image the user describes if it looks likely to fail (vague motion terms, conflicting instructions, restricted keywords, real-person photo references). Does NOT cover circumventing likeness/identity protection on real people — that is explicitly out of scope and the skill will redirect to a fictional-character approach instead."
---

# Dola Prompt & Image Guideline Troubleshooter

Diagnoses why an AI video/image generation failed or got flagged, then rewrites the prompt or swaps the reference image to achieve the **same creative output** through a different, guideline-compliant route.

## Hard scope boundary — read first

💀 **This skill will NOT help rewrite prompts/images specifically to bypass likeness/identity protection on real people.** If the error is "For likeness protection, videos can't be generated from images of real people" — the fix is NOT a clever rewrite. The fix is:
1. Switch to a fictional/AI-generated character (consistent with the NULL RACE / BOG NARUTO pipeline — CHAR_BOSS, CHAR_JUMPER style assets), or
2. Use text-to-video instead of image-to-video for that shot.

If the user pushes for a workaround on this specific error, stop and redirect to option 1 or 2. Do not iterate wording to slip past it.

Everything else below is in scope.

## Phase 1 — Diagnose

Ask for (or infer from what's pasted): the exact error message, the full prompt text, and any reference image description/subject.

Run the error/prompt through this triage:

| Symptom | Likely Cause | Category |
|---|---|---|
| "No points were used" / silent fail, no output | Credit deduction failure, unsupported input combo, duration/resolution mismatch, server timeout | System/Technical |
| "Likeness protection... real people" | Input image detected as a real identifiable person | 🛑 Out of scope — redirect (see above) |
| Generic "policy violation" / "content flagged" | Restricted keyword in prompt text (violence, brand name, real person's name, sexual content, extremist symbols) | Content Policy |
| Output is garbled/warped motion | Vague or conflicting motion instructions ("fast slow-motion") | Prompt Quality |
| Output ignores half the prompt | Prompt too dense, buries the subject/action past the first 20–30 words | Prompt Structure |
| Output looks nothing like intent | Reference image ambiguous, or subject/action stated too late in prompt | Prompt Structure |

## Phase 2 — Rewrite Strategy (for in-scope issues)

**Content Policy hits (restricted keywords):**
- Identify the specific flagged term or concept (name it plainly to the user — don't guess silently).
- Replace real brand/IP names with generic or invented equivalents (e.g. "RWB Porsche 911" → "matte olive widebody sports coupe" if brand terms are the trigger).
- Replace real people's names with role/appearance descriptions ("a man in his 30s, confident stance" instead of naming a celebrity).
- Soften violence/weapon terms to implication or aftermath framing rather than the act itself, if the creative intent allows it.

**Prompt Structure issues:**
- Move subject + action to the first 20–30 words. Cut throat-clearing/style words from the opening.
- Split one dense paragraph into Shot 1 / Shot 2 / Shot 3 structure if the scene has multiple beats.
- Resolve contradictions (pick one pace, one lighting direction, one camera move per shot).
- For motion-critical shots, recommend switching to reference-to-video (a real reference clip) instead of describing motion in text — text is for spatial decisions, video references are for timing/motion.

**System/Technical fails:**
- Checklist: duration within model's allowed range (Seedance 2.0 Fast: 4–15s), resolution supported by tier (Fast = 480p/720p, not 1080p), reference asset count under model's max, reference video/audio combined length under 15s, credits actually available before submit.

## Phase 3 — Output Format

Always give:
1. **Diagnosis** — plain statement of what caused the failure (one line).
2. **Category** — Technical / Content Policy / Prompt Structure / 🛑 Out of scope.
3. **Rewritten prompt** (if applicable) — full replacement text, ready to paste.
4. **What changed and why** — 1–2 lines per change, so the user learns the pattern instead of depending on Claude every time.

Keep it signal-only, no fluff, per Gojira's standing chat rules. Pitfall-first is already satisfied by the diagnosis step — don't duplicate a separate pitfall block unless a NEW risk is introduced by the rewrite itself.

## Notes

- This skill assumes fictional characters/vehicles (per BOG NARUTO/NULL RACE convention) as the default creative approach — it does not need to ask permission to suggest fictionalizing a real reference, that's the standard first move for any real-person or real-brand flag.
- Model-specific limits (resolution, duration, input counts) drift over time — if unsure of current limits for the specific model in use (Seedance, Flow, etc.), web-search current specs rather than assuming.
