# Lyrics Finder

A macOS CLI utility that automatically fetches and saves song lyrics for your audio library. It scans directories for audio files, reads metadata, queries lyrics APIs, and saves lyrics files alongside the audio.

## Supported Formats

- FLAC
- MP3
- M4A / ALAC

## Prerequisites

- Python 3
- macOS (case-insensitive filesystem support)

## Dependencies

- [mutagen](https://mutagen.readthedocs.io/) — reads audio metadata tags (artist, title) from FLAC, MP3, and M4A files
- [requests](https://requests.readthedocs.io/) — HTTP client for querying lyrics APIs

## Installation

Clone the repository and set up a virtual environment:

```bash
git clone https://github.com/dhomak/lyricsfinder.git
cd lyricsfinder

# Create a virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

## Virtual Environment Management

The virtual environment isolates this project's dependencies from your system Python.

```bash
# Activate before each session
source venv/bin/activate

# Verify activation (should show the venv path)
which python3

# Upgrade dependencies
pip install --upgrade mutagen requests

# Deactivate when done
deactivate
```

If you need to recreate the environment from scratch:

```bash
rm -rf venv
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Usage

Make sure the virtual environment is activated, then run:

```bash
python3 audio_lyrics_fetcher.py <directory>
```

### Examples

```bash
# Scan your Music folder
python3 audio_lyrics_fetcher.py ~/Music

# Scan an external drive
python3 audio_lyrics_fetcher.py /Volumes/Music/Albums

# Scan the current directory
python3 audio_lyrics_fetcher.py .

# Use a custom delay between API requests (default: 2 seconds)
python3 audio_lyrics_fetcher.py ~/Music --delay 1.0
```

### Options

| Flag | Description | Default |
|------|-------------|---------|
| `directory` | Root directory to search for audio files | (required) |
| `-d`, `--delay` | Delay between API requests in seconds | `2.0` |

## How It Works

1. Recursively scans the target directory for FLAC, MP3, and M4A files
2. Extracts artist and title from each file's metadata tags
3. Queries lyrics APIs in priority order: **LRCLIB** then **lyrics.ovh**
4. Saves synced (timed) lyrics as `.lrc` files and plain text lyrics as `.txt` files
5. Skips files that already have lyrics; upgrades `.txt` to `.lrc` when synced lyrics are found

## Output

Lyrics files are saved next to the audio file with a matching name:

```
Album/
  01 - Song.flac
  01 - Song.lrc      # synced lyrics (preferred)
  02 - Track.mp3
  02 - Track.txt     # plain text lyrics (fallback)
```

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE).
