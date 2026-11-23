# YT2TXT — YouTube Transcript Grabber

A single-file web app that extracts YouTube transcripts/captions as **clean text**, **timestamped text** (`[m:ss]` lines), or **SRT** subtitles. It runs entirely in your browser — no install, no keys, no tracking.

**Open it:** <https://emili0o0.github.io/youtube-transcript-tool/> *(live once Pages finishes its first deploy)*

Or download [`index.html`](./index.html) and open it locally — it is fully self-contained.

## What it does

- Paste any YouTube link — `watch`, `youtu.be`, `/shorts/`, `/embed/`, `/live/`, `music.youtube.com`, playlist links (with `&list=…`), or a bare 11-character video ID
- Three output formats: readable paragraph text, timestamped lines, and standard SRT
- Every caption track the video actually has (manual + auto-generated), with your last language choice remembered
- Smart paragraph merging that breaks at pauses and sentence ends
- Copy to clipboard or download as `.txt` / `.srt`
- Playlist batch mode: fetch every video (first 100) with a progress bar and per-video status
- Local history of recent extractions (stored only in your browser's `localStorage`)
- Dark, YouTube-style interface; fully self-contained HTML file

## How it reaches YouTube

Browsers cannot call `youtube.com` cross-origin, so the app tries, in order:

1. **YouTube's own internal API directly from your browser** (no relay — usually works on home networks)
2. **Piped** API and its open media proxy for caption files
3. **Invidious** instances
4. **Generic CORS proxies** to fetch the watch page itself

Each source is tried until one answers; failures fall through automatically.

## Development

The core logic (URL parsing, json3/TTML/VTT caption parsing, formatting) sits between
`/*__CORE_START__*/` and `/*__CORE_END__*/` markers in `index.html` and can be unit-tested
in isolation with Node.

## Limitations

- Private, age-restricted, members-only, and captionless videos have no accessible transcripts
- Public relays run on datacenter IPs and are sometimes bot-challenged — the direct-from-browser path usually rescues those; otherwise wait a minute and retry
- Auto-generated captions contain no punctuation, so "clean text" for them is a stream of words broken into paragraphs by pauses

## License

MIT — see [LICENSE](./LICENSE).
