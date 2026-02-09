# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Python CLI utility for fetching and saving song lyrics for audio files on macOS. It scans directories for audio files (FLAC, MP3, M4A), extracts metadata, queries lyrics APIs, and saves lyrics files adjacent to the audio files.

## Commands

```bash
# Activate virtual environment (required before running scripts)
source venv/bin/activate

# Upgrade dependencies
pip install --upgrade mutagen requests

# Run the main multi-format lyrics fetcher
python3 audio_lyrics_fetcher.py ~/Music

# Run FLAC-only processor
python3 flac_lyrics_parser.py ~/Music

# Run universal handler
python3 universal_lyrics_finder.py ~/Music

# Deactivate virtual environment
deactivate
```

## Architecture

### Core Components

**LyricsFetcher Class** - Shared across all scripts, handles:
- HTTP session management with User-Agent spoofing
- Multi-API fallback chain: LRCLIB → ChartLyrics → lyrics.ovh → AllOrigins proxy
- Distinguishes synced lyrics (.lrc) from plain text (.txt)

**Audio Format Handlers**:
- `audio_lyrics_fetcher.py` - Multi-format (FLAC, MP3, M4A) with synced lyrics priority
- `flac_lyrics_parser.py` - FLAC-specific processor
- `universal_lyrics_finder.py` - Unified approach combining best features

### Processing Flow

1. Recursively scan directory for audio files (case-insensitive on macOS)
2. Extract artist/title from audio metadata tags
3. Query lyrics APIs in priority order with 2-second rate limiting
4. Save `.lrc` for synced lyrics, `.txt` for plain text
5. Skip existing lyrics files

### Dependencies

- **mutagen** - Audio metadata reading/writing
- **requests** - HTTP client for API calls
