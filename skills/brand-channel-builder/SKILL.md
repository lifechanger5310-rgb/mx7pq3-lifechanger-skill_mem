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

### Step 3 — Ask the user via ask_user_input_v0 (MANDATORY, before the guide)
Do not write the step-by-step guide until you've asked. Use ask_user_input_v0 with short, mutually exclusive options. Cover, as relevant to scope:

1. **Brand/channel name or handle** — if not already given, ask as free text isn't supported by the tool, so infer from conversation or ask a single_select of likely candidates only if genuinely ambiguous; otherwise state the assumption inline instead of burning a question slot.
2. **Content niche/category** (e.g. Gaming, Cinematic/Film, Tech, Vlog, Business/Brand) — shapes category selection and keyword suggestions.
3. **Primary goal** — Monetization ASAP / Audience growth first / Portfolio-brand credibility / Just getting set up correctly.
4. **Team size** — Solo creator / Solo now but may add editors later / Small team already.
5. **Platforms in scope** (if not already clear from Step 1) — Email only / YouTube only / Instagram only / Both YouTube + Instagram / All three.
6. **Content type for Instagram** if in scope — Creator (personal brand/content) / Business (ads, storefront, partnerships).

Keep this to 1–3 questions per ask_user_input_v0 call (tool max), and skip any question whose answer is already obvious from context — don't re-ask what's already known.

### Step 4 — Produce the step-by-step guide
Once fetch + questions are done, write the full guide, pitfall-first per session defaults:
- For each in-scope item (email, YouTube, Instagram), lead with a short "pitfalls to avoid" list, then numbered setup steps.
- Bake in the user's answers directly (e.g. if niche = Gaming, suggest gaming-relevant channel keywords/category; if goal = Monetization ASAP, front-load the YPP/Instagram monetization tool section with the exact current thresholds fetched in Step 2).
- Explicitly flag which numbers came from the live search (cite naturally, e.g. "as of your search, thresholds are X") so the user knows it's current, not memorized.
- End with a compact "enable all advanced features" checklist table per platform so nothing is missed.

## Hard rules
- Never present thresholds/feature names from memory alone — always search first (Step 2 is non-negotiable, even if `references/platform_notes.md` seems to already have the answer).
- Never write the full guide before asking the Step 3 questions, unless the user has explicitly pre-answered all of them in their message already.
- Keep answers plain-English, signal-only, pitfall-first — no filler preambles.
