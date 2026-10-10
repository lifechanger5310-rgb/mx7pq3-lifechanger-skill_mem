# mx7pq3-lifechanger-skill_mem

> **Claude Session Continuation Guide** — This README is the handoff doc for resuming work in a new Claude session/ID after context/token limits. Read this first if you're continuing this project.

---

## 🗂️ Repository Purpose

This repo is the **source of truth for all Claude skills, memory, and active projects** for user `lifechanger5310-rgb` (Bog Naruto, bognaruto.bgmi@gmail.com).

- `memory/` — persistent memory files (master.md, context logs)
- `skills/` — 14+ SKILL.md files for custom Claude behaviors
- `projects/` — active project builds

---

## 🎬 ACTIVE PROJECT: YouTube Sprint Motion Graphic

**Project Path:** `projects/yt-sprint-motion-graphic/`

**Goal:** Build a motion-graphic / music-video style visual explanation of:
`YouTube_Success_Sprint_One_Month_Monetization_Playbook.docx`

Using three specific tools the user specified:
1. **HyperFrames** (https://github.com/heygen-com/hyperframes) — HTML-native video framework
2. **ACE-Step 1.5** (https://github.com/ACE-Step/ACE-Step-1.5) — local AI music generation
3. **FFmpeg** — frame stitching to MP4

---

## ✅ COMPLETED STEPS (as of 2026-10-10, Session session_01TzEKTtEJyNHKRdAzvsyZiB)

### Step 1 — DOCX Content Extracted
- File: `YouTube_Success_Sprint_One_Month_Monetization_Playbook.docx`
- Tool used: `pandoc -t markdown`
- 12 sections extracted: Reality Check, Prerequisites, Channel Setup, Niche Selection, Content Strategy, Shorts Strategy, Long-Form Strategy, Pitfalls Table, High-Leverage Tips, Monetization Path, One-Month Checklist, Final Words
- Key data points mapped:
  - YPP thresholds: 1,000 subs + 4,000 watch hours OR 10M Shorts views
  - Feb 2027 new rule: doubles to 8,000 hrs / 20M Shorts views
  - 3–5 hrs/day minimum time commitment
  - 1–3 Shorts/day; 15–45s optimal Short length
  - 50 ideas minimum before filming
  - 8–15 min optimal long-form length

### Step 2 — Tool Research (Deep)
- **HyperFrames** confirmed working pattern: HTML clips with `data-composition-id`, `data-start`, `data-duration`, `data-track-index` attributes; GSAP timelines registered to `window.__timelines`; renders via Puppeteer + FFmpeg
- **ACE-Step 1.5** confirmed NOT viable in cloud (no GPU, no model download, allowlisted network). Must be run on local machine (Poco F7 / MacBook). See LOCAL SETUP section below.
- **FFmpeg** v6.1.1 confirmed installed in cloud: `/usr/bin/ffmpeg`
- **Chromium** pre-installed at `/opt/pw-browsers/chromium`; `PLAYWRIGHT_BROWSERS_PATH=/opt/pw-browsers`

### Step 3 — Motion Graphic HTML Built ✅
- **File:** `projects/yt-sprint-motion-graphic/index.html`
- Self-contained HTML, 1280×720px, dark cinematic aesthetic
- 11 animated scenes, ~114 seconds runtime
- **Libraries:** GSAP 3.12.5 (cdnjs), TextPlugin registered
- **SFX:** Web Audio API code-generated (no external files needed)
  - `playImpact()`, `playWhoosh()`, `playBeat(vol)`, `playRiser(dur)`, `playChord(notes, dur, vol)`
- **Color palette:** `--bg: #040404`, `--green: #00ff88`, `--red: #ff2244`, `--gold: #ffd000`, `--blue: #00cfff`
- Controls: Play / Stop / Restart buttons
- HUD: progress bar, scene label, elapsed timer, watermark

**Scene Map:**
| Scene | Content | Time |
|-------|---------|------|
| S0 | Intro / Title | 0–10s |
| S1 | Reality Check | 10–22s |
| S2 | Prerequisites | 22–34s |
| S3 | Channel Setup | 34–45s |
| S4 | Niche Venn Diagram | 45–57s |
| S5 | 4-Week Plan Grid | 57–70s |
| S6 | Shorts Strategy | 70–81s |
| S7 | Never / Always Rules | 81–92s |
| S8 | High-Leverage Tips | 92–102s |
| S9 | Monetization Path | 102–109s |
| S10 | Final 3 Rules | 109–114s |

### Step 4 — Git Push Status
- Repo cloned to: `/home/claude/mx7pq3-lifechanger-skill_mem/`
- **Push blocked:** Claude GitHub App not authorized for `lifechanger5310-rgb` org
- **Fix:** User must visit: https://github.com/apps/claude/installations/select_target → grant access → return to Claude and say "push now"
- Once authorized, run: `git push origin main`

---

## 🔲 UPCOMING STEPS (pick up here in next session)

### Step 5 — HyperFrames CLI Setup & Render
```bash
# In the project directory:
cd projects/yt-sprint-motion-graphic/

# Install HyperFrames (requires Node.js 22+)
npx hyperframes@latest init

# The index.html is already written in HyperFrames-compatible pattern
# Move/link it to the HyperFrames project structure

# Render to MP4 via Puppeteer + FFmpeg
npx hyperframes render --input index.html --output output.mp4 \
  --width 1280 --height 720 --fps 30 \
  --chromium-path /opt/pw-browsers/chromium
```

💀 **PITFALL:** HyperFrames may require specific `data-*` attribute structure. Check if the current `index.html` scene divs need `data-composition-id` and `data-start` attributes on each clip. If so, refactor each `#s0`–`#s10` scene div.

### Step 6 — Puppeteer Frame Capture (Manual fallback if HyperFrames CLI fails)
```javascript
// render-frames.js
const puppeteer = require('puppeteer');
const fs = require('fs');
const path = require('path');

(async () => {
  const browser = await puppeteer.launch({
    executablePath: '/opt/pw-browsers/chromium',
    args: ['--no-sandbox']
  });
  const page = await browser.newPage();
  await page.setViewport({ width: 1280, height: 720 });
  await page.goto(`file://${path.resolve('index.html')}`);
  
  // Click play
  await page.click('#playBtn');
  
  // Capture 30fps for 114 seconds = 3420 frames
  const totalFrames = 114 * 30;
  const frameDir = './frames';
  fs.mkdirSync(frameDir, { recursive: true });
  
  for (let i = 0; i < totalFrames; i++) {
    await page.screenshot({ path: `${frameDir}/frame_${String(i).padStart(5, '0')}.png` });
    await page.waitForTimeout(1000 / 30);  // 33ms per frame
  }
  
  await browser.close();
})();
```

```bash
npm install puppeteer
node render-frames.js
```

### Step 7 — FFmpeg Stitch Frames to MP4
```bash
ffmpeg -framerate 30 \
  -i frames/frame_%05d.png \
  -c:v libx264 \
  -pix_fmt yuv420p \
  -crf 18 \
  -preset slow \
  output_silent.mp4
```

### Step 8 — ACE-Step 1.5 Local Music Generation (RUN ON LOCAL MACHINE ONLY)

> ⚠️ This CANNOT run in the Claude cloud environment. Run on your Poco F7 or MacBook.

```bash
# Prerequisites: Python 3.11+, git, 4GB+ VRAM
git clone https://github.com/ACE-Step/ACE-Step.git
cd ACE-Step

# Install uv (fast Python package manager)
pip install uv

# Run the Gradio UI
uv run acestep --model_name ACE-Step-v1-3.5B

# OR use REST API
uv run acestep --api --port 8001

# Generate music via REST
curl -X POST http://localhost:8001/generate \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Epic cinematic hip-hop beat, dark trap 808s, motivational energy, YouTube creator grind",
    "duration": 120,
    "bpm": 85,
    "format": "mp3"
  }'
```

💀 **PITFALL:** ACE-Step 1.5 default model is ~3.5B parameters, needs ~4GB VRAM minimum. If your GPU has less VRAM, try `--model_name ACE-Step-v1-1B` (smaller variant).

### Step 9 — Merge Audio into Video
```bash
# Add ACE-Step generated music to silent MP4
ffmpeg -i output_silent.mp4 -i music_acestep.mp3 \
  -c:v copy -c:a aac \
  -shortest \
  output_with_music.mp4

# If mixing music + Web Audio SFX recorded separately:
ffmpeg -i output_silent.mp4 -i music_acestep.mp3 -i sfx_recording.mp3 \
  -filter_complex "[1:a][2:a]amix=inputs=2:duration=shortest[aout]" \
  -map 0:v -map "[aout]" \
  -c:v copy -c:a aac \
  final_output.mp4
```

### Step 10 — Director Loop (Quality Control)
The Director Loop is a self-critique system:
1. Render preview at low quality (e.g., 15fps, scale 640×360)
2. Claude reviews screenshot of each scene and grades: typography, readability, timing, motion quality
3. Iterate on `index.html` before final render
4. Grade threshold: all scenes must score ≥ 8/10 before final 30fps render

```bash
# Quick preview render
ffmpeg -framerate 15 -i frames/frame_%05d.png \
  -vf "scale=640:360" -c:v libx264 preview.mp4

# Then review in Claude with: "Grade this motion graphic 1-10 on: 
#   typography, contrast, animation timing, information density"
```

### Step 11 — Final Git Push
```bash
# After GitHub App authorized:
cd /home/claude/mx7pq3-lifechanger-skill_mem
git add projects/yt-sprint-motion-graphic/
git add README.md
git commit -m "2026-10-10 commit 1: Motion graphic HTML + render pipeline docs

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01TzEKTtEJyNHKRdAzvsyZiB"
git push origin main
```

---

## 🔧 Environment Reference (Cloud Session)

| Tool | Status | Path/Version |
|------|--------|-------------|
| FFmpeg | ✅ Available | `/usr/bin/ffmpeg` v6.1.1 |
| Chromium | ✅ Available | `/opt/pw-browsers/chromium` |
| Node.js | check with `node --version` | needs 22+ for HyperFrames |
| Python | ✅ Available | system python3 |
| ACE-Step | ❌ Cloud-blocked | local machine only |
| HyperFrames CLI | 🔲 Not installed | `npx hyperframes init` |
| Puppeteer | 🔲 Not installed | `npm install puppeteer` |

**Repo clone path:** `/home/claude/mx7pq3-lifechanger-skill_mem/`
**Project file:** `projects/yt-sprint-motion-graphic/index.html`

---

## 📌 Quick Resume Commands for New Claude Session

```
"Continue the YouTube Sprint Motion Graphic project. 
Repo is at /home/claude/mx7pq3-lifechanger-skill_mem.
Read the README.md first. 
The HTML is done at projects/yt-sprint-motion-graphic/index.html.
Next step: HyperFrames CLI setup and Puppeteer frame capture.
FFmpeg is at /usr/bin/ffmpeg, Chromium at /opt/pw-browsers/chromium."
```

---

## 📁 Skills Index

| Skill | File |
|-------|------|
| Pitfall-first analytics | `skills/pitfall-analystic-skill/SKILL.md` |
| Think out loud default | `skills/think-out-loud-default/SKILL.md` |
| Banana Pro Director 20 | `skills/banana-pro-director-20/SKILL.md` |
| Brand Channel Builder | `skills/brand-channel-builder/SKILL.md` |
| Cinema Worldbuilder Pro | `skills/cinema-worldbuilder-pro-20/SKILL.md` |
| YT AI Playground Prompt | `skills/yt-ai-playground-prompt/SKILL.md` |
| DOLA Prompt Troubleshooter | `skills/dola-prompt-guideline-troubleshooter/SKILL.md` |

---

*Last updated: 2026-10-10 by Claude Sonnet 4.6 (session_01TzEKTtEJyNHKRdAzvsyZiB)*
