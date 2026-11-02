# Self-hosted video pipeline (in progress)

**Started:** 2026-11-01

## What it is

A pipeline on my homelab server that turns one recorded voiceover into an animated, captioned explainer video for YouTube, plus vertical short cuts for YouTube Shorts, TikTok and Instagram Reels. I set the goals (faceless, animated, $0 budget, my own voice) and an AI coding agent built and tested it under my direction.

## How it works

1. **Voice cleanup:** FFmpeg filters out rumble and noise, then normalises loudness to the -16 LUFS that streaming platforms expect.
2. **Transcription:** faster-whisper runs locally on the CPU and returns word-level timings.
3. **Scene plan:** a JSON file lines up scenes (titles, stats, bullet lists, quotes, stock footage) with the timings, and marks which parts become shorts.
4. **Rendering:** a Remotion (React) template renders 1920×1080 and 1080×1920 versions from the same plan, with animated backgrounds and karaoke-style captions.
5. **Scheduling:** Postiz, a self-hosted social media scheduler, runs in Docker beside it.

## Problems solved

- On the vertical version, captions overlapped the scene text: a percentage padding was based on the frame's width, not its height. Fixed by laying out against explicit heights.
- Speech recognition split words like "two-factor" and "81%" into separate pieces; added a merge step.
- Renders took about 2 minutes per 20 seconds of video. Swapping CSS blur for gradients and giving the container more cores made them about 5× faster.
- A library update (PyAV 19) broke transcription; pinned a working version.

## Security choices

- Runs in its own unprivileged container. Scheduler secrets are generated on the server and never leave it.
- Platform posting goes through official APIs only, with no shared logins.

## Descent template (2026-11-01)

I planned the channel format (calm narration in my own voice, long videos with shorts cut from them) and chose the visual style; the AI agent then rebuilt the template to that plan around one signature visual: an animated descent driven by an altitude track.

- **Data-driven HUD:** an altitude ruler and live temperature and pressure readouts, interpolated from a reference atmosphere table (pressure interpolated logarithmically). Each environment is one JSON file, so the same template can do the ocean or other places.
- **Scene types:** chapter cards, count-up facts, comparison bars, a self-drawing temperature graph, public-domain NASA imagery with automatic credits, and face-cam cuts synced to the narration.
- **Sound:** an ambient bed synthesised from filtered noise (Python, NumPy, SciPy), mixed by altitude, so there are no third-party sound licences to track.
- **Research:** the AI-drafted script was fact-checked against sources, with a source table I reviewed. Two first-draft numbers were wrong and got fixed.
- **Tuning:** the film-grain overlay made files 6× larger; refreshing it every third frame cut a 4.4-minute render from 832 MB to 131 MB.

## 3D and real footage (2026-11-02)

I asked for richer animation and real footage. The agent added:

- **Real-time 3D in the render:** a Venera-style lander and rotating planet shots in three.js, rendered on the CPU (no GPU in the server). The planet uses NASA's Magellan radar map and a procedurally generated cloud texture, with a shader for the atmosphere glow.
- **Public-domain footage pipeline:** search-and-fetch tools for NASA's image and video library and the Goddard Scientific Visualization Studio, which normalise clips to 1080p30 and write the credits automatically.
- **Texture-based parallax** cloud and haze layers, and a perspective ground plane.
- **Troubleshooting:** the first 3D render was killed by the kernel's out-of-memory killer. I confirmed it in the host logs, then resized the container and tuned render parallelism and the video cache.

## Status

The template is tested end to end with a synthetic voice and placeholder face cam. Next: record the first real narration.

*Stack: Linux containers, FFmpeg, Python, faster-whisper, TypeScript/React (Remotion), Docker Compose.*
