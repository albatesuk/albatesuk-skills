---
name: website-explainer-video
description: Research a product's website and build a 30–60s explainer video as a frame-exact JavaScript animation (GSAP), with animated captions, brand-matched visuals, and a 4K/1080p/web export pipeline with music. Use when the user wants an explainer, promo or product video made from a website or product docs.
---

# Website → explainer video

Make a high-production 30–60s explainer video for a product, built as a single-page JavaScript animation, then render it frame-by-frame to video files. The video's job: **after one viewing, someone in the target audience understands what the product does for them and where to get it.** One viewing is the test: if a viewer would need to watch it twice to follow it, it's too busy.

Work in stages and **stop for approval at each gate**. Do not render or capture video until the user approves the beats.

**Helper tools.** Stages 4, 6 and 7 use five small tools: `stills`, `check-early`, `capture`, `export` and `add-music`. They don't ship with this skill. Write each one into the project's `tools/` folder when a stage first needs it, for the platform you're running on, following [references/tools.md](references/tools.md).

**Talking to the user.** Assume the user isn't technical. Everything you say to them must be in plain words. Never mention GSAP, timelines, Playwright, ffmpeg, ProRes, CRF, LUFS, codecs or file formats by name unless they use those terms first. Say "the preview", "the video files", "the high-quality version" and "the website version" instead. When you need a decision, explain the choice in terms of what they'll get, not how it's made.

---

## Stage 0 — Explain the plan (before any work)

Unless the user has clearly done this before, start with a short, friendly message in plain words that covers:

- **What you'll make:** a 30–60 second video that explains their product, with captions that work with the sound off, their brand's look, and their web address at the end.
- **How it works:**
  1. You read their website.
  2. You write a short script for them to approve.
  3. You build a preview they can watch in their browser and ask for changes to.
  4. Only once they're happy, you make the final video files.
  5. Music and adding the video to their website are optional extras.
- **What you need from them:**
  - their website address, or the folder with its files
  - a folder for the project (suggest one)
  - anything they already know: who the video is for, a length, or a line they want in it
- **Time:** making the final files can take 20 minutes to an hour, and you'll give an estimate when you start.

Keep it to a few short lines, then ask for anything missing and begin Stage 1. If they've already given the website and folder, go ahead with Stage 1 in the same reply.

**Software check.** Before Stage 3, check whether Node.js and Google Chrome (or Playwright's Chromium) are installed. Before Stage 6, check for ffmpeg and the encoders that [references/tools.md](references/tools.md) needs. If anything is missing, tell the user in plain words: say what's needed ("a free tool for building the video files"), that you can install it for them, and roughly how long it'll take. Ask before installing, then use the platform's usual installer (Homebrew on macOS, winget on Windows, the system package manager on Linux). If you can't install something, give them one simple step to do it themselves.

## Stage 1 — Research (no output files yet)

When you report what you found, write it for the user: what the product does, who it's for, and what you'd build the video around. Leave out the technical details of how you'll build it.


1. Read the product's website (the live site and/or local source if the user gives a path). Prioritise the homepage, any "how it works" page, the page that best explains the core idea, pricing, and audience- or use-case pages. If the user says the product's name has changed, use the current name throughout.
2. Extract:
   - **The unique selling point in one sentence**, phrased as an outcome for the viewer.
   - **The audience's strongest fear or desire**, in concrete terms. This is the hook. Pick the thing that would keep a buyer up at night (a security breach, a lost customer, wasted money) or the thing they most want, not a polite paraphrase of it ("not sure what's allowed"). Ask yourself whether this person would actually lose sleep over it.
   - **For a service rather than a product**, the "how we work" or approach page is the backbone: the method *is* what the product does. Note its stages and what each one delivers.
   - **Who the video is for** and how familiar they are with the field. Their vocabulary decides the script's vocabulary.
   - **A concrete worked example** the site already uses (a sample customer, scenario, dataset or user story). Building the video around the site's own example makes the video and site reinforce each other. If there isn't one, invent a specific, believable one and say so.
   - **Brand tokens**, read from the live site's computed styles rather than guessed: how the logo is made (image, or styled text, and in which font, colours and gradient direction), primary and accent colours, gradients, heading and body fonts.
   - **Reusable assets**: logos, icons, avatars, illustrations, screenshots, product videos. Note their resolution. Anything much under the video's width will look soft; plan to rebuild those UI screens as crisp HTML/SVG instead.
3. Note the style of any existing illustrations or imagery; it's the obvious starting point for the look pass.
4. If a path the user gives doesn't exist, check the parent folder for a near match before asking. Don't search their wider home directory.

## Stage 2 — Script (GATE: user approves)

Write `script.md` with a beat table: **time · visuals · on-screen caption · (optional) voiceover**. Aim for 45–58s.

**Settle the hook first.** If you aren't sure it's the strongest angle, offer the user two or three one-line hooks with a recommendation before writing the full script. A whole script built on a weak hook gets thrown away. When the user asks for a different angle, write it as a separate script (`script-<angle>.md`) so they can compare the two.

Rules:
- **Benefits, not features.** Describe what changes for the viewer, not the mechanism behind it.
- **Use the audience's words.** Identify the industry jargon the target viewer wouldn't use, ban it, and use plain alternatives. List the banned terms and their replacements at the bottom of the script.
- **It must work with the sound off.** Websites and social feeds autoplay muted. The captions alone must tell the whole story; voiceover or music is an extra layer.
- **Captions sound like a person talking.** Write full, natural spoken sentences. No clipped slogans, no paired fragments ("Your secrets stay yours. AI works for you."), and no abstract nouns standing in for real things ("the slow work" → "the paperwork your team spends hours on"). Read each caption aloud: if nobody would say it to a colleague, rewrite it.
- **Name the thing, every time.** No vague "it" or "this" ("get a grip on it", "get something out of it"): say what it refers to.
- **No consultant-speak or category clichés** ("pays for itself", "earning its keep", "unlock value", "transform"). Say the concrete benefit the buyer cares about: saves time, cuts costs, moves the business faster.
- **The problem beat must point at the cause the product fixes.** A pain the product doesn't address is a dead end. Frame the pain so the reveal answers it directly: if the product is guidance, the problem is that nobody has been shown how; if it's automation, the problem is hours lost to manual work.
- **When the user writes a line, use it word for word**, including their choice of highlight.
- **Promise only what the business actually commits to.** Check every caption and on-screen claim against the site's own wording. Watch for lines that quietly widen the offer, such as "tailored to your business", which commits the business to custom work it may not offer. When in doubt, say it more generically.
- **Keep the method to about three steps.** If the site lists more stages, group them into its phases and show the phases. Six steps in under a minute is too much to take in.
- **Each caption carries one idea** and stays on screen for 4–8s. Fewer captions held longer beat many quick ones.
- A beat structure that works for most products:
  1. **Problem** (0–6s): the viewer's everyday pain, shown rather than told.
  2. **Why the usual way falls short** (6–10s).
  3. **Product reveal** (10–15s): a logo moment, then the very first thing a user does in the product.
  4. **What it does: 2–3 beats** (15–44s). Show the product *doing the work* on the worked example, with one visual per beat. The core selling-point beat gets the most time.
  5. **Payoff** (44–48s): the result in the user's world (a notification, a finished report, a happy customer). Visuals here should show the benefit (icons, a before and after) rather than text that repeats the caption.
  6. **End card** (48s to the end): logo, one-line tagline, **the domain in large type**, and a call to action. Hold it for at least 4s.
- Ask only the questions that change the build: voiceover or music only, aspect ratio (default 16:9), which tagline to use if the live site and local source differ, and any claims you're unsure the user will stand behind ("in minutes", "free consultation", "results in a week"). Ask even about claims already on the site; users often won't put them in a video. List every number and its source in a "Claims to check" section of the script.

## Stage 3 — Mockup animation (GATE: user reviews in the browser)

Build in the folder the user names: `index.html`, `style.css`, `main.js`, `assets/` (copied from the site) and `tools/` (see [references/tools.md](references/tools.md)). Use placeholders only where no real asset exists, and list them for the user.

### Readability and pacing (check these before showing anything)
Most viewers watch on a phone, where a 1920 frame plays at about a fifth of its size, and they read one thing at a time.
- **Minimum type sizes on the 1920 stage:** captions about 56px, scene text 40px, headings and key words 56–76px. Sources and fine print can go down to about 26px, but nothing the story depends on.
- **One idea per card**, shown in sequence. Use big numbers ("9 tools in use", "4 nobody approved") instead of tables, lists of rows or labelled diagrams. Leave out small labels on icons and persistent widgets (progress paths with titles, chip rows); if a label isn't readable on a phone, cut it.
- **Only one thing changes at a time.** Make the change, then hold while the caption is read. Never animate one side of the screen while something else happens on the other, and don't bring in a new element before the viewer has read the last one. Background drift and a single continuous effect that *is* the point of the beat (say, files streaming out) are fine.
- **Show progress with big step headings** ("Step 1 of 3" plus a short title) rather than a persistent diagram with small text.
- **Keep the corner lockup readable:** if `domain | value proposition` would be too small, drop the value proposition (it's on the reveal and the end card anyway).
- Check by scaling a few stills to 390px wide and stacking them (`ffmpeg -vf scale=390:-1` then `vstack`). If any word can't be read, fix it before showing the user.

### Architecture (non-negotiable: this is what makes capture frame-exact)
- A fixed **1920×1080 `#stage`**, scaled to fit the window with a CSS transform. Review controls (play/pause, scrubber, chapter buttons, ←/→ for 1s, `,`/`.` for one frame) sit outside the stage and are hidden when the URL contains `?capture`.
- **One paused GSAP timeline** (`gsap.timeline({paused:true})`, loaded from cdnjs) holds every animation. Use no CSS animations or transitions anywhere.
- Continuous motion (blinking cursors, wobble, clock hands, drifting backgrounds, floating particles) runs as **drivers**: pure functions of timeline time, called from the timeline's `onUpdate` and after every seek. Drivers write to *inner* wrapper elements, so they never fight GSAP over the same `transform`.
- All randomness uses a **seeded RNG** (e.g. mulberry32), so every playback and capture is identical.
- Expose `window.__seek(t)`, `window.__duration`, `window.__tl` and `window.__ready`. `__ready` resolves after `document.fonts.ready`. Support `?t=12.5` to open on a specific frame.
- If the logo is text-based, rebuild it as **live text** in the brand font and colours rather than using a small PNG, so it stays sharp at 4K.

### Helpers that prevent common bugs
```js
// Fades from nothing start hidden, so nothing shows before its tween begins.
const F = (t, from, to, pos) => {
  if (from.autoAlpha === 0) gsap.set(t, { autoAlpha: 0 });
  return tl.fromTo(t, from, { ...to, immediateRender: false }, pos);
};
// Entrance: any x/y/rotation set in `from` returns to 0 unless `to` says otherwise.
const IN = (t, pos, from = {}, to = {}) => {
  const rest = {};
  ['x', 'y', 'rotation'].forEach((k) => { if (k in from && !(k in to)) rest[k] = 0; });
  return F(t, { autoAlpha: 0, scale: 0.6, ...from },
    { autoAlpha: 1, scale: 1, duration: 0.55, ease: 'back.out(1.8)', ...rest, ...to }, pos);
};
```
- Give every animated element a class such as `.a` (`visibility:hidden; opacity:0`) as its starting state.
- Measure layout (text widths, aligning one element onto another) **lazily**, using function-based tween values, because web fonts aren't loaded when the timeline is built. Convert `getBoundingClientRect` values to stage coordinates by dividing out the stage scale.
- When an element animates into a container, make sure it sits **above** that container in DOM or z-order, or the move happens out of sight. The same goes for full-screen layers such as the end card's background: give them an explicit `z-index` below the logo and text meant to sit on top, or the logo vanishes behind them.
- SVG paths drawn with `strokeDashoffset` and round line caps show dots at rest; hide them until drawing starts.
- Emoji render differently per platform and some look faint; check them in renders, and prefer icons or SVG for anything important.
- Display fonts with "SC" or all-caps designs uppercase everything. Set URLs and anything that must read as lowercase in the body font.

### Captions (animated subtitles)
- Make captions a designed element, not TV subtitles. A pattern that works: each line is a slightly tilted card that springs in, with words popping in one at a time.
- Mark key phrases in the caption text (e.g. `*like this*`) and render them as a **brand-coloured pill** in the display font, with a small bounce.
- Give pills **horizontal margin (~0.3em)** and a **gentler overshoot** than plain words (e.g. `back.out(1.4)` against `back.out(3)`, and bounce scale ≤ 1.06). Otherwise they overlap the neighbouring words mid-animation.
- **Highlight sparingly:** one short pill per caption (1–4 words), on the word that carries the point. If everything is highlighted, nothing is.
- Punctuation after a highlighted phrase must not sit under the pill.
- Keep captions in one consistent position (usually bottom centre), and keep every scene's content clear of that zone.

### Branding through the video
- After the logo reveal, shrink the logo into a **persistent corner lockup** of `domain | one-line value proposition` (or just `wordmark | domain` if the value proposition would be too small to read), so anyone glancing at the video mid-way knows what it is and where to find it. Align the large logo exactly onto the lockup's wordmark before cross-fading between them.
- Bring the logo back to the centre for the end card.

### Making the product beats convincing
- Show the product working on the worked example, using simplified versions of its real UI patterns: inputs being filled in, results appearing, items being created, organised and sent.
- Prefer **transformation shots**: one input visibly becoming several tailored outputs, or a manual chore becoming an automatic one. They communicate value faster than feature lists.
- Carry the same objects across beats (the same card, person or document moving between scenes). That continuity makes the story easy to follow.
- Avoid visible repetition in generated content (grids, lists, calendars): shuffle with the seeded RNG so no row or column repeats a pattern.

## Stage 4 — Review loop

- Serve the folder with a static server. If the host app starts dev servers from a config file (e.g. Claude Code's `.claude/launch.json`), **read any existing config first** (including in parent folders the session uses) and give your entry a name that can't collide with another project's. A clash silently starts someone else's server.
- The `stills` tool renders the times you give it at 1920×1080 and builds a contact sheet. Review **mid-animation frames**, not only settled ones: overlaps and flashes hide there.
- Review the sheet for readability and pacing as well as bugs: a frame with three things mid-animation is too busy.
- The `check-early` tool reports any element that's already visible just before its fade-in starts. Prove the check works by briefly disabling the fix and confirming it flags elements, then restore the fix. Run it before every render.
- Iterate on the user's feedback. Fix root causes in the shared helpers, so one fix applies everywhere. When the user says it's too busy, cut elements and beats outright; don't just slow them down.

## Stage 5 — Look pass (optional, GATE)

If generated imagery is wanted, agree the direction first (usually the site's existing illustration style and brand palette). Ask for API keys only at this stage, then replace the placeholders. Often the brand-matched mockup is already good enough, so ask before doing this.

## Stage 6 — Capture & export (only after the beats are approved)

- Run `check-early` first and fix anything it reports.
- If the user only wants web delivery, skip the master: run `capture --web` to go straight to an H.264 MP4 at 1080p60. Make only the formats they ask for.
- Otherwise, the `capture` tool renders the 1920×1080 stage at 2× (3840×2160) into a **ProRes 422 HQ 10-bit master** at 60fps. Expect a few frames per second at 4K, so run it in the background and report an estimated finish time.
- Restart the render if the CSS or JS changes after it starts, because the page is loaded only once.
- The `export` tool builds the deliverables from the master:
  - Presentation and big screen: H.264 at 4K60 plus a 1080p60 fallback.
  - Web: H.264 at 1080p, a VP9 WebM, and a poster JPG.
- Verify every output with ffprobe (resolution, frame rate, frame count, duration) and look at a frame from each file.

## Stage 7 — Music

- Ask the user for the music file's path. Don't search their folders.
- The `add-music` tool handles the track:
  - Trims it to the video's length, keeping the start unless told otherwise, and pads a short track with silence.
  - Measures the trimmed section with `ebur128` and adjusts the gain to **-14 LUFS**.
  - Adds a ~20ms fade-in and a ~1.5s fade-out over the end card, and limits peaks.
  - Muxes it into every file without re-encoding the video, which takes seconds: PCM in the master, AAC in MP4, Opus in WebM.
- **Never shorten the video to fit the audio.** Afterwards, confirm frame counts are unchanged and that audio starts at 0.
- Report the source loudness, the gain applied, and the final loudness and peak.

## Stage 8 — Website embed (if asked)

- Use click-to-play rather than autoplay when the video has sound:
  - a poster frame that shows the core idea (usually not the end card)
  - a large play button and a label with the running time
  - `preload="none"`
  - on click, hide the overlay, show the controls and play; on `ended`, show the overlay again
- **Don't put `object-fit: cover` on the `<video>`.** Fullscreen on a non-16:9 screen would crop the sides; let it letterbox instead.
- Make sure the play button gets a pointer cursor (some CSS frameworks no longer add one to buttons by default).
- Keep any old video assets that other pages still use. Find every page that should get the new video, and don't put a video with captions in one language onto pages in other languages unless the user asks.
- Check the result in a production build, since dev servers can serve stale CSS. Don't commit or push unless asked; give the user a commit command that leaves out their unrelated untracked files.
