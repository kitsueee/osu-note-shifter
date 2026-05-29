# osu! Note Shifter

A desktop tool for retiming `.osu` beatmap files. Load a beatmap, set a new start time and song length, preview the shifted notes, then copy or save the result.

## Features

- **Shift all notes** to a new start time — every hit object and timing point moves by the same offset
- **Trim by song length** — notes beyond your specified end time are automatically removed
- **Live preview** — see the first 20 notes before/after the shift, with trimmed notes highlighted in red
- **Copy to clipboard** — copies the `[HitObjects]` section ready to paste directly into a `.osu` file
- **Save as new file** — exports a full modified `.osu` file (named `original_shifted.osu` by default)
- Supports hold notes (mania long notes) — end times are shifted and capped correctly

## Usage

1. Click **Browse** and select a `.osu` beatmap file
2. The current start and end times will be auto-filled
3. Enter your **New song length** (e.g. `2:30:000` for 2 min 30 sec)
4. Enter the time you want the **first note shifted to** (e.g. `0:00:000`)
5. Click **Preview** to see the result
6. Hit **Copy .osu to clipboard** to grab just the `[HitObjects]` section, or **Save .osu file** to export the full file

### Time format

All time fields use `M:SS:mmm` — minutes, seconds, milliseconds.

| Input | Meaning |
|-------|---------|
| `0:00:000` | 0 ms |
| `0:05:500` | 5.5 seconds |
| `1:30:000` | 1 minute 30 seconds |
| `2:45:250` | 2 min 45.25 sec |

You can omit the milliseconds and just type `1:30` if you don't need that precision.

## Running from source

Requires Python 3.8+ with tkinter (included in standard Windows Python installs).

```bash
python osu_note_shifter.py
```

## Building a standalone .exe

```bash
pip install pyinstaller
pyinstaller --onefile --windowed --name "osu_note_shifter" osu_note_shifter.py
```

The executable will appear in the `dist/` folder.

> **Note:** If you see an error about `enum34`, run `pip uninstall enum34` first, then retry.

## How it works

The tool reads the raw `.osu` file line by line and:

- Parses `[HitObjects]` to find every note's timestamp
- Calculates the offset: `target_start - earliest_note_time`
- Applies the offset to every hit object timestamp (including hold note end times)
- Applies the same offset to every `[TimingPoints]` entry so BPM snap stays intact
- Removes any notes whose shifted time exceeds the specified song length

## License

MIT
