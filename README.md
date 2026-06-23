# Stem Arranger

A single-file, browser-based tool for arranging musical stems into a full song
and exporting one clean WAV per instrument. Everything runs locally in your
browser — no upload, no server, no dependencies.

Open [`stem-arranger.html`](stem-arranger.html) in any modern browser to use it.

## What it does

Drop in a folder of stems organised by section and loop, sequence them however
you like, preview the result in real time, then export a separate WAV for each
instrument spanning the entire arrangement.

## Expected folder structure

The app reads the last three path components of each audio file as
`section / loop / filename`:

```
Stems/
├── Intro/
│   ├── Loop 01/   ← KICK.wav, KIT.wav, SHAKER.wav…
│   └── Loop 02/
├── Verse/
├── Chorus/
└── Bridge/
```

- **Section** — the top-level musical part (Intro, Verse, Chorus, …). Recognised
  section names are auto-sorted into a sensible musical order.
- **Loop** — a variation within a section. A folder with only `section/file.wav`
  (no loop folder) is treated as the `Default` loop.
- **Filename** — the leading token before the first `_` is used as the
  instrument name (e.g. `KICK_8 Bar Chorus_Loop 01.wav` → `KICK`). A `N bar`
  hint in the filename is used to detect tempo.

Supported audio formats: `wav`, `mp3`, `m4a`, `aif`, `aiff`, `ogg`, `flac`.

## Features

- **Load a folder of stems** directly from disk (uses the directory file picker).
- **Section library** grouped by section, colour-coded, with per-loop stem and
  duration info.
- **Build an arrangement** by clicking loops to append them; drag blocks to
  reorder; swap loop variations per block.
- **BPM & time signature** — tempo is auto-detected from bar counts in filenames
  (median across loops) and can be overridden; beats-per-bar is configurable.
- **Per-block trimming** — crop start/end by beats or whole bars with a visual
  beat timeline.
- **Live preview** — play, pause, stop, seek, and see the current block
  highlighted, all synced via the Web Audio API.
- **Export stems** — renders each instrument to its own WAV
  (`INSTRUMENT_arrangement.wav`) covering the whole song using an
  `OfflineAudioContext`.

## Usage

1. Open `stem-arranger.html` in your browser.
2. Click **Load Stems Folder** and pick your `Stems/` directory.
3. Click loops in the left panel to add them to the arrangement.
4. Reorder, swap variations, and trim blocks as needed.
5. Set the BPM and beats/bar if the auto-detected values need adjusting.
6. Press play (or Space) to preview.
7. Click **Export Stems** to download one WAV per instrument.

## Notes

- Everything is processed client-side; your audio never leaves your machine.
- Best results in Chromium-based browsers (the folder picker uses
  `webkitdirectory`).
- Export length is bounded by browser memory — very long arrangements with many
  stems may need to be split up.
