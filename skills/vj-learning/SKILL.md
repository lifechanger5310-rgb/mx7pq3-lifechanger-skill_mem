---
name: vj-learning
description: Visual-first adaptive learning skill — extends sj-learning with intelligent lens selection and rich interactive visuals. Picks only the lenses a topic actually needs, then renders each insight as the best visual type for that content (chart, diagram, stepper, card, etc). Works for any topic.
---

# 🎨 VJ-LEARNING — Visual-First Adaptive Knowledge Skill

---

## WHAT THIS SKILL IS

VJ-LEARNING is an extension of SJ-LEARNING.

It does NOT redefine how to explain things. That logic lives in SJ-LEARNING.

This skill adds two things on top:
1. **Selective lens application** — only use the lenses a topic actually needs, not all 10 by default
2. **Visual rendering** — every insight gets the best visual type, rendered via Claude's Visualizer tool

---

## STEP 0 — FETCH SJ-LEARNING FIRST

Before doing anything, load SJ-LEARNING:

- If a skill file is already in context (uploaded as `skills/sj-learning/SKILL.md` or similar) → use it directly
- If not → fetch from GitHub: `https://raw.githubusercontent.com/lifechanger5310-rgb/mx7pq3-lifechanger-skill_mem/main/skills/sj-learning/SKILL.md`
- Apply ALL tone, language, pitfall, analogy, and closing rules from SJ-LEARNING

SJ-LEARNING = the brain. VJ-LEARNING = the eyes.

---

## STEP 1 — TOPIC ANALYSIS (internal, never shown)

Before selecting lenses or visuals, silently run:

```
TOPIC ANALYSIS:
[ ] What TYPE is this? (coding / health / creative / life / concept / system / data / process)
[ ] Is there a meaningful origin story? (worth Lens 1?)
[ ] Are there real-world examples that ground it? (worth Lens 2?)
[ ] Does it have a sequence or process? (worth Lens 8?)
[ ] Does it have parts that connect? (worth Lens 7 map?)
[ ] Is there a strong analogy? (worth Lens 5?)
[ ] Is it technical/code-heavy? (worth Lens 6?)
[ ] Does it have a case study that teaches better than explanation? (worth Lens 9?)
[ ] What are the TOP pitfalls? (always check Lens 4)
[ ] What VISUAL TYPE makes each insight click instantly?
[ ] What card size/proportion does the output target? (default: 5.8" × 3.2" aspect ratio)
```

---

## STEP 2 — LENS SELECTION RULES

Do NOT apply all 10 lenses every time. Select only what the topic earns.

| Lens | Use when... | Skip when... |
|------|-------------|--------------|
| 1 Origin | History changes how you use it today | Topic is timeless concept, no meaningful origin |
| 2 Real World | Real examples ground abstract ideas | Topic is already concrete |
| 3 80/20 | Topic has many parts; most are low-value | Topic is already narrow/focused |
| 4 Pitfalls | Clear traps exist that beginners hit | Simple topic with no real failure modes |
| 5 Analogy | Concept is abstract or counterintuitive | Topic is already tangible/visual by nature |
| 6 Code/Memory | Technical, coding, system execution topic | Non-technical topic |
| 7 Thought Map | 3+ connected parts that form a system | Simple linear or standalone concept |
| 8 Progressive Build | Topic is a skill or has ordered steps | Lookup info, not a learnable skill |
| 9 Case Study | A real story teaches it better than explanation | Topic is pure concept with no real-world anchor |
| 10 Closing | ALWAYS — every topic needs a sharp close | Never skip |

**Minimum lenses per topic type:**
- Concept/theory → Lenses: 5, 7, 10 (+ others as earned)
- Coding/technical → Lenses: 4, 6, 7, 8, 10
- Health/medical → Lenses: 4, 5, 7, 10
- Creative/content → Lenses: 2, 4, 8, 9, 10
- Life/mindset → Lenses: 5, 9, 10
- Data/system → Lenses: 3, 6, 7, 10

---

## STEP 3 — VISUAL TYPE SELECTION

For every selected lens, pick the best visual type from this list. Render it using Claude's Visualizer tool (show_widget — HTML/React/SVG/Mermaid). Never use plain text where a visual would teach better.

| # | Visual Type | Best for | Tool/Format |
|---|------------|----------|-------------|
| 1 | Flow Diagram | Processes, sequences, request lifecycles | Mermaid, SVG arrow flow |
| 2 | Card UI | Summaries, comparisons, key facts | React/HTML component |
| 3 | Timeline | History, progressive stages, version evolution | React/HTML rendered |
| 4 | Chart / Graph | Data, comparisons, performance metrics | Recharts, Chart.js, D3 |
| 5 | Tree / Hierarchy | Parent-child structures, taxonomies, folder trees | React tree component |
| 6 | Table | Feature comparisons, structured lookups | HTML/React table |
| 7 | Progress Bar | Layer-by-layer build, skill levels, completion | HTML/React |
| 8 | Kanban Board | Workflow states, task stages, status flows | React component |
| 9 | Dashboard | Multi-metric topics, system overviews | React multi-widget |
| 10 | Comparison Cards | Two options, before/after, tradeoffs | React side-by-side |
| 11 | Mind Map | Concept webs, relationship clusters | Mermaid / React |
| 12 | Annotated Image | UI walkthroughs, anatomy of a thing | HTML overlay on SVG |
| 13 | Step Stepper UI | Progressive builds, tutorials, how-to flows | React component |
| 14 | Heatmap | Frequency, intensity, usage patterns | D3 / Recharts |
| 15 | Gauge / Meter | Single value with range (speed, risk, score) | React/SVG |

### Visual selection rules:

- **One visual per lens** — each lens that needs a visual gets its own Visualizer call
- **Match visual to insight type** — never use a flow diagram for comparison data; never use a table for a sequence
- **Interactive > static** — if the visual has a slider, toggle, or clickable state that teaches more, add it
- **Card proportions** — default output should respect the 5.8" × 3.2" aspect ratio (safe card size) unless user specifies otherwise
- **sendPrompt() on every visual** — every rendered widget should have at least one clickable "ask deeper" button that fires into chat
- **Dark mode mandatory** — all visuals must work in both light and dark mode using CSS variables
- **No gratuitous visuals** — if a lens insight is a single sentence fact, keep it text; visuals earn their place

---

## STEP 4 — OUTPUT STRUCTURE

### Format per lens:

```
[Lens badge + name]
[1–3 sentence insight in SJ-LEARNING tone]
[Visualizer tool call — best visual type for this lens]
[1 line connecting this lens to the next]
```

### Overall flow:

1. Start with topic type identification (1 sentence, casual)
2. Apply selected lenses in SJ-LEARNING order (1 → 10), skipping unused ones
3. Each lens = text insight + visual
4. Close with Lens 10 — sharp, short, what actually matters
5. Offer sendPrompt() buttons for natural next questions

### What goes in text vs visual:

| In text | In visual |
|---------|-----------|
| The insight sentence | The structure / relationship |
| Pitfall warning (🔴) | The flow or comparison |
| Analogy setup | The analogy mapped visually |
| Closing bullets | The summary card |

---

## STEP 5 — EXTRA INGREDIENTS

VJ-LEARNING allows adding ingredients beyond SJ-LEARNING when a topic needs them:

| Ingredient | When to add |
|-----------|-------------|
| **Live data simulation** | Topic involves changing values (e.g. stock, API rate limits) → add a slider/live widget |
| **Code playground hint** | Coding topic → after code lens, add a sendPrompt that opens a runnable example |
| **Comparison toggle** | Two competing approaches exist → add a toggle between them in the visual |
| **Risk gauge** | Topic has a risk/difficulty spectrum → add a gauge showing where this falls |
| **Cheat sheet card** | Dense topic with many facts → add a scannable card at the end |
| **Progress tracker** | Topic is a skill → add a "where you are now / where this leads" progress bar |

These are optional. Only add when the topic earns it. Never add for decoration.

---

## VISUAL DECISION QUICK MAP

```
Topic received
      │
      ▼
Does it have a sequence or steps?
   YES ──▶ Flow Diagram (Lens 7/8) + Step Stepper for build
   NO  ──▶ continue
      │
      ▼
Does it compare two or more things?
   YES ──▶ Comparison Cards or Table (Lens 2/3)
   NO  ──▶ continue
      │
      ▼
Does it have data or metrics?
   YES ──▶ Chart / Graph or Gauge (Lens 2/3)
   NO  ──▶ continue
      │
      ▼
Does it have 3+ connected concepts?
   YES ──▶ Mind Map or Tree (Lens 7)
   NO  ──▶ continue
      │
      ▼
Is it a skill with stages?
   YES ──▶ Progress Bar + Stepper (Lens 8)
   NO  ──▶ continue
      │
      ▼
Is it a system with states?
   YES ──▶ Dashboard or Kanban (Lens 7/9)
   NO  ──▶ Card UI (Lens 10 always)
```

---

## QUALITY CHECK (run before output)

```
[ ] Did I fetch/apply SJ-LEARNING rules (tone, pitfalls, 80/20)?
[ ] Did I run topic analysis and select only earned lenses?
[ ] Does every selected lens have the right visual type?
[ ] Did every visual use Visualizer tool (not plain ASCII)?
[ ] Is every visual dark-mode safe (CSS variables)?
[ ] Did I add sendPrompt() to at least one element per visual?
[ ] Is the output at card-proportions (5.8" × 3.2" aspect ratio)?
[ ] Did I close with Lens 10 — sharp and short?
[ ] Did I avoid gratuitous visuals (no visual without a teaching reason)?
[ ] Does output feel like a smart friend, not a template?
```

---

## TRIGGER PHRASES

VJ-LEARNING activates when the user says any of:
- "explain X visually"
- "teach me X with visuals"
- "use vj-learning"
- "visual explanation of X"
- "show me how X works"
- "make me understand X"

When triggered: fetch SJ-LEARNING → run topic analysis → select lenses → render visuals.
