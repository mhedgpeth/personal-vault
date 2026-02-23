# Getting Songs into Moises

Use `yt-dlp` to download audio from YouTube, then import into Moises via Files App.

### Requirements

- YouTube Premium subscription
- `yt-dlp` installed (`brew install yt-dlp`)
- [Moises](https://moises.ai) Pro subscription

### Workflow

1. Find the song on YouTube, copy the URL
2. Download audio: `yt-dlp -x --audio-format m4a --audio-quality 0 -o "~/Music/duo-set/%(title)s.%(ext)s" "URL"`
3. Import into Moises on iPad via **Files App** from `~/Music/duo-set/`
4. Moises will run Hi-Fi stem separation (included with Pro)

### Tips

- Use the video URL only (strip playlist params like `&list=...` — yt-dlp handles it but cleaner without)
- `--audio-quality 0` gives best quality audio extraction
- m4a format works well with Moises (MP3, WAV, FLAC, M4A, MP4, MOV, WMA all supported)
- For a capella songs (e.g. Mercedes Benz), you don't need stem separation
