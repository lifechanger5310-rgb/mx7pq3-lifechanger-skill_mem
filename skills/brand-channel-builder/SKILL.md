---
name: brand-channel-builder
description: Use this skill whenever the user wants to create, set up, or build a brand identity from scratch — a dedicated brand email, a YouTube Brand Account/channel, an Instagram Creator/Business account, or any combination of these — with "all features enabled" or "advanced/pro setup." Trigger on phrases like "create a brand channel," "set up YouTube/Instagram from scratch," "build my channel with all features," "enable all advanced features," or "new brand for [project name]." This skill ALWAYS first fetches current, up-to-date platform requirements via web search (thresholds, eligibility, feature names change often), THEN asks the user clarifying questions via ask_user_input_v0 before producing the guide — never skip straight to a generic step-by-step answer without doing both of these first.
---

# Brand Channel Builder

Builds a brand identity (email + YouTube + Instagram, or any subset) from scratch, fully configured with every relevant feature enabled, using live/current platform data rather than stale training knowledge.

## Workflow (do these in order, never skip a step)

### Step 1 — Scope check
Read the user's request. Determine which of these they want: (a) brand email only, (b) YouTube channel only, (c) Instagram account only, (d) two or more together, (e) unspecified "brand channel" → assume all three unless they've already set one up (check memory/conversation for signs they already have a channel/email).

### Step 2 — Fetch up-to-date data (MANDATORY, before asking questions)
Never rely on memorized thresholds — YouTube/Instagram change monetization tiers, feature-eligibility numbers, and tool names frequently. Before writing the guide, run web_search for whichever platforms are in scope, e.g.:
- "YouTube Partner Program monetization requirements [current year]"
- "YouTube channel feature eligibility verification [current year]"
- "Instagram professional account features [current year]"
- "Instagram Creator Marketplace Brand Collabs Manager eligibility [current year]"

Pull the most recent, official-leaning sources (support.google.com, youtube.com/creators, about.instagram.com preferred over SEO blogs where possible). Note any tiered thresholds (e.g. entry-level fan-funding tier vs full ad-revenue tier) — don't collapse them into one number.

See `references/platform_notes.md` for the last-verified snapshot (verified July 2026) as a fallback/starting point — but always re-search rather than trusting it blindly if it's been a while, since these numbers shift.

### Step 3 — Ask the user ONE QUESTION AT A TIME (MANDATORY, before the guide)
Do not batch multiple unrelated questions into a single ask_user_input_v0 call, and do not write the step-by-step guide until every item below is resolved. Go through them in order, one at a time, waiting for the reply before asking the next:

1. **Platforms in scope** — Email only / YouTube only / Instagram only / Both YouTube + Instagram / All three. (ask_user_input_v0, single_select)
2. **Content niche/type** — options tailored to what's plausible from context, plus an "Other" fallback. (ask_user_input_v0, single_select)
3. **Primary goal sequencing** — what matters first vs later (Monetization ASAP / Audience growth first / Credibility & professional look / Just get set up correctly) — allow the user to express an order, not just one pick. (ask_user_input_v0, rank_priorities if supported, else single_select for "right now" only)
4. **Brand/channel name** — free text, so ask conversationally in prose, not via ask_user_input_v0 (that tool is for short button choices only). Do not proceed until a real candidate name is given.
5. **Brand name analysis (mandatory sub-step, runs immediately after the name is given, before any further question):**
   - web_search the exact name + "youtube channel" and + "instagram" to check for conflicts with an existing unrelated creator/brand.
   - Note likely handle-availability pattern but be honest that search can't definitively confirm YouTube/Instagram handle availability — the final check happens live in-app during setup.
   - Assess catchiness/memorability qualitatively: length, ease of spelling when spoken aloud, whether it signals the niche, whether it's distinct enough to be searchable (not a generic word that gets buried).
   - If weak or conflicted, propose 3–5 concrete alternative names before letting the user proceed, and ask them to confirm the final choice (conversational, not ask_user_input_v0).
6. **Team size** — Solo creator / Solo now, may add editors later / Small team already. (ask_user_input_v0, single_select)
7. **Instagram account type** (if Instagram in scope) — Creator (personal brand/content) / Business (ads, storefront, partnerships). (ask_user_input_v0, single_select)

Skip any question whose answer is already obvious from the conversation — don't re-ask what's already known. Never skip the brand-name step (4) or its analysis (5).

### Step 4 — Produce the step-by-step guide (treat the reader as a first-time internet user)
Once fetch + all questions are resolved, write the full guide at maximum granularity — no skipped clicks, no assumed familiarity:
- For each in-scope item (email, YouTube, Instagram), lead with a short "pitfalls to avoid" list specific to that platform, informed by the user's stated goal sequence.
- Then give literal click-by-click steps: name the exact screen, the exact button/label text, and every fork-in-the-road decision (e.g. "Google will ask: 'For my personal use' or 'To manage my business' — choose the second one because..."). Explain *why* at each fork, not just *what* to click.
- Bake in the user's actual answers (niche, goal order, team size, brand name) directly into the guide rather than leaving it generic.
- Explicitly flag which numbers/thresholds/UI labels came from the live search in Step 2 (state it's current as of the search, since these move) rather than presenting them as evergreen facts.
- End with a compact "enable all advanced features" checklist table per platform so nothing is missed.
- If a UI label found via search is ambiguous or uncertain, say so plainly rather than inventing a confident-sounding label — a wrong literal instruction is worse than an honest "check the current wording here, it may read slightly differently."

## Hard rules
- Never present thresholds/feature names from memory alone — always search first (Step 2 is non-negotiable, even if `references/platform_notes.md` seems to already have the answer).
- Never write the full guide before Step 3 is fully resolved, including the brand-name analysis.
- Ask one question at a time, not a batch of three.
- Keep pitfalls signal-only, but the step-by-step portion should be maximally detailed/literal — these are different registers within the same guide, don't compress the steps for brevity.
