# Clipper

Clipper turns a narration script and a pile of raw video clips into an edited video. It generates TTS audio from the script, detects objects in each clip with YOLO, matches sentences to footage, then stitches a final cut.

Built at **[TartanHacks 2025](https://www.tartanhacks.com/)** as a **4-person collaboration**. I have been continuing the project independently since the hackathon.

This is still a prototype: the pipeline works, but the repo is a snapshot of a 24-hour build plus later experiments, not a polished product.

## How it works

1. **Script → speech** — ElevenLabs (or Amazon Polly in some helpers) narrates the script
2. **Clip analysis** — YOLOv8 labels objects in uploaded `.mp4` / `.mov` files
3. **Matching** — sentences are timed against the narration and matched to clips
4. **Export** — MoviePy assembles `outputs/final_video.mp4`

## Repo layout

```
src/
  runMe.py              # PyQt desktop UI (main entry point)
  main.py               # CLI pipeline (same steps, no UI)
  scriptToTTS.py        # ElevenLabs TTS
  sceneDetection.py     # YOLO object detection on clips
  scriptMatching.py     # Script ↔ clip matching
  videoProcessing.py    # Final edit / export
  TartanHacks2025/      # Original hackathon scripts
voiceover_app/          # Small Flask + Amazon Polly voiceover helper
data/                   # Local scripts, clips, and generated metadata
```

## Setup

**Requirements:** Python 3.10+, FFmpeg, and API keys (see below).

```bash
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

cp .env.example .env               # then fill in keys
```

Download YOLO weights on first run (`yolov8s.pt`); they are gitignored.

### Environment variables

Copy `.env.example` to `.env`. Never commit `.env`.

| Variable | Used for |
|----------|----------|
| `OPENAI_API_KEY` | GPT-based metadata / matching experiments |
| `ELEVENLABS_API_KEY` | Narration TTS |
| `AWS_ACCESS_KEY_ID` | Amazon Polly / Rekognition helpers |
| `AWS_SECRET_ACCESS_KEY` | same |
| `AWS_REGION` | e.g. `us-east-1` |

## Run

Put clips in `data/clips/`, then:

```bash
# Desktop app — paste a script, upload videos, process
python3 src/runMe.py

# Headless pipeline
python3 src/main.py
```

Optional Polly voiceover playground:

```bash
cd voiceover_app
pip install flask boto3
python src/server.py
```

## Status

Hackathon prototype plus independent follow-up. Expect leftover scripts, unused experiments, and a UI that is functional rather than finished. I am cleaning this up over time; this pass is docs and secrets only — the pipeline code is unchanged.
