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

## Status

Tested end to end with a synthetic voice. Next: pick a topic and record the first real voiceover, and register the platform API apps.

*Stack: Linux containers, FFmpeg, Python, faster-whisper, TypeScript/React (Remotion), Docker Compose.*
