# Helper tools

Write these into the project's `tools/` folder the first time a stage needs them, on the platform you're running on. They aren't shipped with the skill, because paths, encoders and shells differ between machines.

## Shared rules

- **Write them in Node** (`.mjs`, ES modules) so the same files run on macOS, Linux and Windows. Add `playwright-core` (or `playwright`) as a dev dependency in the project's `package.json`. Use shell scripts only if the user asks; see "Shell scripts" below.
- **Nothing is product-specific.** Take these from arguments or a small `tools/config.mjs` rather than hard-coding them:
  - the page URL, defaulting to the static server's address with `?capture=1`
  - an output slug, e.g. `acme-explainer`
  - the video duration, read from `window.__duration` in the page, or with `ffprobe` from the master
- **Browser:** launch Playwright with `channel: 'chrome'` when Google Chrome is installed, because it renders fonts and colour the way viewers will see them. Otherwise fall back to Playwright's bundled Chromium (`npx playwright install chromium`). Say which one you used.
- **Before doing any work**, every page tool must:
  - log `pageerror` events to the console
  - wait for `window.__ready` after `goto`
- **ffmpeg:** call it with `spawn`/`execFile` and argument arrays, not a shell string, so paths with spaces work on every OS. Check that the encoders you need exist (`ffmpeg -hide_banner -encoders`) before a long run, and say clearly what's missing.
- **Outputs** go under `out/`. Create directories as needed.

## stills

Renders review frames and a contact sheet. Usage: `node tools/stills.mjs 2.8 7.9 15 22.4 [--phone]`.

- **Rendering:**
  - Viewport 1920×1080, `deviceScaleFactor: 1`.
  - For each time: `__seek(t)`, then screenshot to `out/stills/t-<time>.png` (zero-padded so the files sort).
  - Stamp the time label as a temporary DOM element in the page (bottom-right; never top-left, where the lockup sits), screenshot, then remove it. Many ffmpeg builds lack `drawtext`, so don't rely on ffmpeg for the label.
- **Contact sheet:** with more than one time, build `out/stills/sheet.png` with ffmpeg:
  - scale each frame to 960×540, then `xstack` two columns
  - layout `x_y` = `(i % 2) * 960` _ `floor(i / 2) * 540`, `fill=black`
- **`--phone`:** also writes `out/stills/phone.png`. Scale each still to 390px wide (`scale=390:-1`) and `vstack` them. This is the phone-readability check.

## check-early

Finds elements that are visible before their fade-in starts. Usage: `node tools/check-early.mjs`. Exit non-zero if anything is found, so it can gate a render.

- **Tweens to check:** inside the page, take every tween from `window.__tl.getChildren(true, true, false)` where `tw.vars.startAt?.autoAlpha === 0`.
- **The check:** for each one, seek to `tw.startTime() - 0.02` (skip it if that's negative). Then report every `Element` target that is visible.
- **Visible means:** walking up from the element to `#stage`, no ancestor has `visibility: hidden`, `display: none` or `opacity < 0.02`.
- **Output:** print how many tweens were checked. Then print each problem as `<time>s <#id or class + first 30 chars of text>`.

## capture

Renders the video frame by frame. Usage: `node tools/capture.mjs [--fps 60] [--scale 2] [--from 0] [--to <duration>] [--web] [--out <file>]`.

- **Page setup:**
  - Viewport 1920×1080 with `deviceScaleFactor: scale`; scale 2 gives 3840×2160.
  - Load the page once, wait for `__ready`, and read `__duration` for the default `--to`.
- **Frame loop:** frames = `round((to - from) * fps)`. For each frame:
  - `__seek(from + f / fps)`
  - `page.screenshot({ type: 'png' })`
  - write the PNG to ffmpeg's stdin
  - respect back-pressure: if `write()` returns false, wait for `'drain'`. Without that, memory blows up at 4K.
- **ffmpeg input:** `-f image2pipe -framerate <fps> -c:v png -i -`
- **ffmpeg output:**
  - Master (default `out/<slug>-master-4k60.mov`): `-c:v prores_ks -profile:v 3 -pix_fmt yuv422p10le -vendor apl0`. That's ProRes 422 HQ, 10-bit.
  - `--web` (default `out/<slug>-web.mp4`, use `--scale 1`): `-c:v libx264 -preset slow -crf 22 -profile:v high -pix_fmt yuv420p -movflags +faststart`.
- **Progress:** print once per second of video, with current time, rendering fps and ETA. At the end, print the frame count and wall time.
- **After rendering:** close stdin, wait for ffmpeg to exit, check its exit code, then close the browser.
- **Background runs:** the run can take a long time, so start it in the background and log to `out/capture.log`. The page is loaded only once, so restart the render if the CSS or JS changes.

## export

Builds deliverables from the master. Usage: `node tools/export.mjs [--poster <seconds>] [--only 4k,1080,web,webm,poster] [--music <file>]`.

Make only the formats the user wants. Write each one to `out/deliverables/<slug>-<name>`.

| Output | ffmpeg settings |
| --- | --- |
| `4k60.mp4` | `-an -c:v libx264 -preset slow -crf 14 -profile:v high -level 5.2 -pix_fmt yuv420p -movflags +faststart` |
| `1080p60.mp4` | `-an -vf scale=1920:1080:flags=lanczos -c:v libx264 -preset slow -crf 14 -profile:v high -level 4.2 -pix_fmt yuv420p -movflags +faststart` |
| `web.mp4` | `-an -vf scale=1920:1080:flags=lanczos -c:v libx264 -preset veryslow -crf 22 -profile:v high -pix_fmt yuv420p -movflags +faststart` |
| `web.webm` | `-an -vf scale=1920:1080:flags=lanczos -c:v libvpx-vp9 -b:v 0 -crf 34 -row-mt 1 -deadline good -cpu-used 2` |
| `poster.jpg` | `-ss <poster> -frames:v 1 -vf scale=1920:1080:flags=lanczos -q:v 3`. Pick a frame that shows the core idea, usually not the end card. |

If a music file is given, run add-music afterwards. Finish by listing the files with their sizes.

## add-music

Adds a music track to the master and every deliverable without re-encoding the video. Usage: `node tools/add-music.mjs <music file>`. The user gives the path; don't search for one.

1. **Find the length:** read the video length `LEN` from the master (or the web MP4) with `ffprobe`. Never hard-code it.
2. **Measure the loudness:** run the first `LEN` seconds of the track through `-af ebur128 -f null -` and parse the integrated loudness: the last `I:` line, in LUFS.
3. **Work out the gain:** gain = `-14 - I` dB.
4. **Build the mix** as `out/music-mix.wav`, 48 kHz `pcm_s24le`:
   - `-t LEN`
   - filters `volume=<gain>dB,afade=t=in:d=0.02,afade=t=out:st=<LEN-1.5>:d=1.5,alimiter=limit=0.89:level=false,apad=whole_dur=<LEN>`
   - `apad` pads a short track with silence, so the video is never cut to fit the audio.
5. **Mux into each file:**
   - `-i <video> -i <mix> -map 0:v:0 -map 1:a:0 -c:v copy <audio args> -t <LEN>`
   - write to a temp file next to the target, then rename it over the target
   - don't use `-shortest`
6. **Audio codecs:**
   - `.mov` master: `-c:a pcm_s24le`
   - `.mp4`: AAC at 256k for 4K/1080p60 and 160k for web, plus `-movflags +faststart`. Use `aac_at` if listed (macOS, best quality), else `libfdk_aac`, else the built-in `aac`.
   - `.webm`: `-c:a libopus -b:a 128k`
7. **Verify:** use `ffprobe` to check that every file's video frame count is unchanged and its audio starts at 0. Report:
   - the source loudness and the gain applied
   - the final integrated loudness and true peak, measured with `ebur128=peak=true` on one output

## Shell scripts

If the user wants shell scripts instead of Node:

- **macOS ships bash 3.2:** under `set -u`, expanding an empty array errors, so write `${arr[@]+"${arr[@]}"}`.
- **Exit status:** don't pipe a script into `tail` or similar, because the pipe hides its exit status.
- **Windows:** bash scripts don't run on Windows without WSL or Git Bash, so ask before choosing bash there.
