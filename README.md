# Video Subtitles Generator

Generate Descript-like word-highlighted subtitles for videos using OpenAI Whisper and MoviePy.

Based on: [Supertranslate.ai - Descript-like Word Highlights](https://github.com/ramsrigouthamg/Supertranslate.ai)

## Features

- Automatic speech-to-text transcription with word-level timestamps
- Word-by-word highlighting as the audio plays
- **✨ Edit transcripts before video generation** - Always pauses to allow fixing errors
- **Bold white text with black outline and shadow** - Professional social media style
- **Customizable highlight colors** - Orange, cyan, yellow, or any color you want
- **Automatic timestamp reuse** - Fast regeneration with different styles (~30s vs 5+ minutes)
- Configurable subtitle appearance (font, size, colors, stroke, shadow)
- Automatic line breaking based on duration and character limits
- Outputs MP4 videos ready for social media
- Support for iPhone videos (MOV files with multiple audio streams)

## Requirements

- Python 3.8+
- FFmpeg (must be installed separately)
- ImageMagick (for text rendering)

### Install System Dependencies

**macOS:**
```bash
brew install ffmpeg imagemagick
```

**Ubuntu/Debian:**
```bash
sudo apt-get install ffmpeg imagemagick
```

**Windows:**
- Download FFmpeg from https://ffmpeg.org/download.html
- Download ImageMagick from https://imagemagick.org/script/download.php

## Installation

1. Clone or download this repository:
```bash
cd /Users/joopsnijder/Projects/video_subtitles
```

2. Create and activate a virtual environment (recommended):
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install Python dependencies:
```bash
pip install -r requirements.txt
```

Note: If you encounter issues with moviepy, you may need to install it separately:
```bash
pip install moviepy
```

## Usage

### Supported Video Formats

The script supports all common video formats including:
- `.mp4` (H.264, H.265)
- `.mov` (QuickTime)
- `.avi`
- `.mkv`
- `.webm`
- And any other format supported by FFmpeg

### 🎨 Quick Start - Professional Style (Recommended)

Generate subtitles with **bold white text, black outline, and customizable highlight color**:

```bash
# Default orange highlight
python generate_with_style.py video.mov

# Cyan highlight (like TikTok/Instagram)
python generate_with_style.py video.mov --color cyan

# Yellow highlight
python generate_with_style.py video.mov --color yellow

# Any color you want
python generate_with_style.py video.mov --color "#FF00FF"
```

**Features:**
- Always pauses after transcription to let you edit the transcript
- Automatically reuses timestamps from previous runs (saves 5+ minutes!)
- Bold Impact font with black outline and shadow
- Word-by-word highlighting in your chosen color
- Professional social media ready

### Basic Usage (Simple Style)

For basic subtitles without the enhanced styling:
```bash
python generate_subtitles.py input_video.mp4
```

This will create `input_video_subtitled.mp4` with word-highlighted subtitles.

### Edit Workflow (Manual Two-Step)

If you need more control over the editing process:

```bash
# Step 1: Transcribe video
python edit_and_generate.py video.mov

# Step 2: Edit the JSON file to fix errors
# Open video_timestamps.json in your editor

# Step 3: Generate video with corrected transcript
python edit_and_generate.py --step2 video_timestamps.json video_audio.mp3
```

**See [EDIT_WORKFLOW.md](EDIT_WORKFLOW.md) for detailed instructions.**

### 🎛️ Advanced Style Customization

Customize every aspect of the subtitle style:

```bash
python generate_with_style.py video.mov \
  --color yellow \
  --stroke-width 4 \
  --outline-width 5 \
  --shadow-offset 8 \
  --fontsize 100 \
  --max-words 3
```

**Style Options:**
- `--color` - Highlight color (orange, cyan, yellow, red, lime, or hex code) [default: orange]
- `--stroke-width` - Inner stroke width in pixels [default: 3]
- `--outline-width` - Outer outline border width (8-directional) [default: 4]
- `--no-outline` - Disable outline border
- `--shadow-offset` - Shadow offset in pixels [default: 6]
- `--no-shadow` - Disable drop shadow
- `--fontsize` - Font size (auto-adjusted by default)
- `--font` - Path to custom font file
- `--max-words` - Maximum words per subtitle line [default: 4]
- `--max-duration` - Maximum duration per line in seconds [default: 3.0]
- `-o, --output` - Custom output path

### Specify Output File

```bash
python generate_with_style.py video.mov -o output_video.mp4
```

### Basic Script Advanced Options

For the basic `generate_subtitles.py` script:

```bash
python generate_subtitles.py input_video.mp4 \
  --model medium \
  --max-chars 80 \
  --max-duration 3.0 \
  --max-gap 1.5 \
  --fontsize 80 \
  --keep-intermediate
```

### Command-line Arguments

- `input`: Input video file path (required)
- `-o, --output`: Output video file path (default: input_subtitled.mp4)
- `-m, --model`: Whisper model size - tiny, base, small, medium, large (default: medium)
- `--max-chars`: Maximum characters per subtitle line (default: 80)
- `--max-duration`: Maximum duration per subtitle line in seconds (default: 3.0)
- `--max-gap`: Maximum gap between words before line break (default: 1.5)
- `--fontsize`: Font size for subtitles (default: 80)
- `--keep-intermediate`: Keep intermediate audio and JSON files

### Whisper Model Sizes

| Model  | Parameters | English-only | Multilingual | Required VRAM | Relative Speed |
|--------|-----------|--------------|--------------|---------------|----------------|
| tiny   | 39 M      | ✓            | ✓            | ~1 GB         | ~32x           |
| base   | 74 M      | ✓            | ✓            | ~1 GB         | ~16x           |
| small  | 244 M     | ✓            | ✓            | ~2 GB         | ~6x            |
| medium | 769 M     | ✓            | ✓            | ~5 GB         | ~2x            |
| large  | 1550 M    | ✗            | ✓            | ~10 GB        | 1x             |

## Configuration

Edit [config.json](config.json) to customize default settings:

```json
{
  "subtitle_settings": {
    "max_chars": 80,
    "max_duration": 3.0,
    "max_gap": 1.5
  },
  "video_settings": {
    "bg_width": 1080,
    "bg_height": 1920,
    "bg_color": [28, 163, 236]
  },
  "text_settings": {
    "font": "Helvetica-Bold",
    "fontsize": 80,
    "color": "white",
    "highlight_color": "blue"
  }
}
```

## Examples

### Example 1: Process iPhone Video with Orange Highlight
```bash
python generate_with_style.py IMG_1234.mov
# Creates IMG_1234_styled.mp4 with orange highlights
```

### Example 2: TikTok/Instagram Style (Cyan)
```bash
python generate_with_style.py video.mov --color cyan
```

### Example 3: Custom Style with Thicker Outline
```bash
python generate_with_style.py video.mov \
  --color yellow \
  --stroke-width 4 \
  --outline-width 6 \
  --shadow-offset 8
```

### Example 4: Regenerate with Different Color (Fast!)
```bash
# First run: transcribes (~5 minutes)
python generate_with_style.py video.mov --color orange

# Second run: reuses timestamps (~30 seconds!)
python generate_with_style.py video.mov --color cyan -o video_cyan.mp4
```

### Example 5: Shorter Subtitle Lines
```bash
python generate_with_style.py video.mov --max-words 2 --max-duration 2.0
```

## Workflow

The `generate_with_style.py` script performs these steps:

1. **Detect Resolution**: Analyzes video dimensions and adjusts font size automatically
2. **Check for Existing Timestamps**: Looks for `{video}_timestamps.json` to skip transcription
3. **Extract Audio** (if needed): Extracts audio from input video to MP3
4. **Transcribe** (if needed): Uses OpenAI Whisper "large" model for Dutch transcription
5. **Save Timestamps**: Saves word timestamps to JSON for reuse
6. **🛑 PAUSE**: Shows transcript preview and asks if you want to edit
7. **Edit** (optional): Opens JSON in your text editor to fix errors
8. **Split Lines**: Organizes words into subtitle lines based on timing and length
9. **Generate Video**: Creates video with styled, word-highlighted subtitles

**Time Savings:**
- First run with transcription: ~5-10 minutes
- Subsequent runs reusing timestamps: ~30 seconds

## Intermediate Files

The script creates files during processing:

- `{input_name}_audio.mp3`: Extracted audio (saved for reuse)
- `{input_name}_timestamps.json`: Word-level timestamps (saved for reuse)
- `{input_name}_styled.mp4`: Final output video

**Timestamp Reuse:**
These files are automatically saved and reused on subsequent runs. This means:
- First run: Full transcription (~5-10 minutes)
- Subsequent runs: Skip transcription (~30 seconds)

To force re-transcription, delete the `*_timestamps.json` file:
```bash
rm video_timestamps.json
```

## Transcript Editing

The `generate_with_style.py` script **always pauses** after transcription to let you edit:

1. Script shows a preview of the transcript
2. Prompts: "Wil je het transcript bewerken? (j/n)"
3. If yes: Opens JSON in your text editor
4. Make your edits and save the file
5. Press ENTER to continue to video generation

**What you can edit:**
- Fix spelling errors
- Correct names/company names
- Adjust capitalization
- Fix misheard words

## Working with iPhone/Mobile Videos

The script works perfectly with videos recorded on mobile devices:

### iPhone Videos (.MOV)
```bash
# Direct processing
python generate_subtitles.py IMG_1234.mov

# With smaller model for faster processing
python generate_subtitles.py IMG_1234.mov --model small

# Custom output name
python generate_subtitles.py IMG_1234.mov -o social_media_video.mp4
```

### Tips for Mobile Videos
- iPhone videos are typically in `.mov` format (H.264 or HEVC)
- The script automatically handles different codecs
- For vertical videos (portrait mode), the default 1080x1920 resolution works great
- Use `--model medium` for best transcription quality
- Use `--model tiny` or `--model small` for faster processing on battery

## Troubleshooting

### ImageMagick Policy Error

If you get a policy error, edit ImageMagick's policy file:

**macOS/Linux:**
```bash
# Find policy.xml
convert -list policy

# Edit and comment out the TEXT restriction:
# <policy domain="path" rights="none" pattern="@*"/>
```

### CUDA/GPU Support

For faster processing with NVIDIA GPUs, install PyTorch with CUDA:
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### Memory Issues

If you run out of memory:
- Use a smaller Whisper model (tiny or base)
- Process shorter video clips
- Close other applications

## License

This project is based on code from [Supertranslate.ai](https://github.com/ramsrigouthamg/Supertranslate.ai).

## Credits

- OpenAI Whisper for speech recognition
- MoviePy for video processing
- Original notebook by ramsrigouthamg
