# mirror-test

A proof-of-concept **video mirror** on GitHub: a video hosted as a Release asset,
streamed in the browser via a GitHub Pages player — **no viewer download needed**.

Live player: https://quant202602.github.io/mirror-test/

## Current video

**Big Buck Bunny** — © Blender Foundation · [peach.blender.org](https://peach.blender.org) ·
**CC BY 3.0** (Creative Commons Attribution, reuse allowed). Verified from the official
Blender YouTube channel license metadata. Mirrored here legally with attribution.

## The zero-local-download pipeline (`remote-mirror` workflow)

The interesting part of this repo: the video never passes through a personal computer.

```
source URL  ──curl──▶  GitHub Actions runner  ──ffmpeg remux──▶  GitHub Release asset ──▶ Release CDN ──▶ viewer's browser
```

### How to use

1. Go to **Actions → remote-mirror → Run workflow**
2. Fill in:
   - `video_url` — a **direct** HTTP(S) link to a video file
   - `asset_name` — e.g. `movie_720p.mp4`
   - `release_tag` — e.g. `v1.0`
3. The runner downloads, remuxes to browser-friendly MP4 (`faststart`, H.264 stream copy, AAC audio),
   and attaches it to the release. Your PC stores 0 bytes.
4. Point a player page `<source>` at
   `https://github.com/<owner>/<repo>/releases/latest/download/<asset_name>`

### Limitations (honest ones)

- **YouTube URLs do not work reliably here.** GitHub's datacenter IPs are usually
  bot-blocked by YouTube ("confirm you're not a robot"). The workflow is meant for
  sources with plain direct file URLs (archive.org, Blender/NASA CDNs, your own server).
  For YouTube, a local yt-dlp download → upload remains the dependable route.
- Releases allow assets up to **2 GiB**; Pages serves the *player*, the video streams
  from the Release CDN with HTTP Range support (seeking works without full download).
- **github.io accessibility from mainland China is intermittent** — this pipeline
  solves *hosting legality/simplicity*, not regional network blocking.

## Local development

Any static server works: `python3 -m http.server` in the repo root, open `http://localhost:8000`.

## License

Site code: MIT. The video itself: **CC BY 3.0** — © Blender Foundation.
