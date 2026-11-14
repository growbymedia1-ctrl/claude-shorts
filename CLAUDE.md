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

## Video edit rule: use ALL repos

For every video edit, first inspect and use all 11 attached repos: claude-shorts, whisper, hyperframes, ffmpeg-skill, claude-video-effects, motion-skills, threejs-skills, motionclone, ghost-editor, yt-dlp, awesome-claude-video-skills. Do not skip a repo without checking what it offers; say in the delivery note which repos were used and why any did not fit.

- **claude-video-effects gets priority.** Read its SKILL.md and scripts before building any effect, and use its effects heavily (face-tracked effects, zooms, overlays, transitions) on talking-head footage.
- No background music unless asked. Keep the speaker's original colours (only a faithful HDR→SDR conversion).
- Deliver iPhone-ready MP4 (1080×1920, H.264 + AAC, yuv420p, faststart), plus a 15 s preview when asked.
