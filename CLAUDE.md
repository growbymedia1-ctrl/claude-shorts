# claude-shorts

Standalone interactive shortform video creator. Extracts viral-ready short clips from longform videos using an interactive Claude Code workflow with Remotion-rendered premium captions.

## Architecture

- **Claude Code** is the orchestrator — reads transcripts, scores segments, presents to user interactively
- **faster-whisper** handles GPU-accelerated transcription with word-level timestamps
- **Remotion** renders 1080x1920 vertical video with animated captions (single-pass)
- **FFmpeg** extracts audio, cuts segments (stream copy), and does platform-specific export encoding

## Key Commands

```bash
bash setup.sh          # Install Python + Node dependencies (one-time)
bash install.sh        # Copy skill to ~/.claude/skills/shorts/
```

## Python Virtual Environment

Scripts use `~/.video-skill/` venv if it exists (shared with claude-video), otherwise `~/.shorts-skill/`. Activate before running Python scripts:

```bash
source ~/.video-skill/bin/activate   # or ~/.shorts-skill/bin/activate
```

## Remotion

The Remotion project lives in `remotion/`. After `bash setup.sh`:

```bash
cd remotion && npx remotion preview  # Preview compositions in browser
node remotion/render.mjs             # Headless render (used by pipeline)
```

## Temp Files

Pipeline stores intermediate files in `$SHORTS_TMP` (defaults to `/tmp/claude-shorts/`). Override with `export SHORTS_TMP="/path/to/dir"` for systems with limited `/tmp` space. The directory is created automatically and cleaned between runs.

## Dependencies

- FFmpeg (system) — audio extraction, segment cutting, export encoding
- Python 3.10+ with faster-whisper, mediapipe, numpy, opencv-python
- Node.js 18+ with remotion, @remotion/captions, zod
- NVIDIA GPU recommended (NVENC encoding, CUDA transcription)

## Owner's video-editing rule (always apply)

When the owner of this repo asks for a video to be edited or made:

- **Use these repos to edit, not a custom pipeline.** Set up and use whichever of the owner's repos the video needs:
  - `hyperframes`: graphics, layout and rendering
  - `claude-shorts`: picking clips and captions
  - `whisper`: transcription and word timings
  - `ffmpeg-skill`: cutting, colour, audio and export
  - Others when relevant: `motion-skills`, `threejs-skills`, `motionclone`, `yt-dlp`, `ghost-editor`, `awesome-claude-video-skills`
- **Report blockers before working around them.** If a repo can't be used (install fails, model download blocked), tell the owner what is blocked before substituting anything.
- **Deliver a real video file.** The final deliverable is always a rendered, downloadable MP4:
  - H.264 video with AAC audio, playable on iPhone and in Photos
  - 1080×1920 (9:16) by default
  - Verified before sending
  - Never HTML or code instead of the video
- **Keep colour grading natural.** Neutral white balance, accurate brown skin tones with no whitening, highlights recovered, no beauty filters, heavy sharpening or Instagram-style looks.
