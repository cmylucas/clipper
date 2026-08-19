# Voiceover helper

Small Flask app that synthesizes speech with **Amazon Polly**. It is a side experiment from Clipper, not the main pipeline.

Uses the default AWS credential chain (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION`, or a local profile).

```bash
pip install flask boto3
python src/server.py
```

Then open [http://127.0.0.1:5000](http://127.0.0.1:5000).
