# Steganography Cheatsheet

## Quick Start

    exiftool image.jpg
    strings image.jpg | grep -i flag
    binwalk image.jpg
    binwalk -e image.jpg

## 1. Image Stego

### Tools

| Tool | Use |
|------|-----|
| exiftool | Metadata |
| strings | Hidden text |
| binwalk | Embedded files |
| steghide | JPEG/BMP/WAV stego |
| zsteg | PNG/BMP LSB |
| stegsolve | Visual analysis |
| stegcracker | Brute force steghide |

### Commands

    exiftool image.jpg
    strings image.jpg | grep -i flag
    binwalk image.jpg
    binwalk -e image.jpg

    steghide info image.jpg
    steghide extract -sf image.jpg
    steghide extract -sf image.jpg -p "password"
    stegcracker image.jpg wordlist.txt

    zsteg image.png
    zsteg -a image.png
    zsteg -E "b1,rgb,lsb,xy" image.png > output

### Stegsolve

    java -jar stegsolve.jar
    # File -> Open -> image
    # Use arrows to cycle bit planes

## 2. Audio Stego

    exiftool audio.wav

    audacity audio.wav
    # Track -> Spectrogram

    steghide extract -sf audio.wav

    ffmpeg -i audio.wav -map_channel 0.0.1 output.wav

## 3. Video Stego

    ffmpeg -i video.mp4 frames/frame_%04d.png
    ffmpeg -i video.mp4 -vn audio.wav

## 4. Text Stego

### Trailing whitespace

    cat -A text.txt | grep -E " +$|\t+$"
    xxd text.txt

### Zero-width characters

    python3 -c "
    data = open('text.txt', 'rb').read()
    hidden = [c for c in data if c in [0xe2, 0x80, 0x8b, 0x8c, 0x8d, 0xef, 0xbb, 0xbf]]
    print('Hidden chars:', len(hidden))
    "

### Acrostic

    awk '{print substr($0,1,1)}' text.txt | tr -d '\n'

## Quick Reference

| Task | Command |
|------|---------|
| Metadata | exiftool image.jpg |
| Strings | strings image.jpg |
| Embedded | binwalk -e image.jpg |
| Steghide | steghide extract -sf image.jpg |
| LSB PNG | zsteg -a image.png |
| Audio stego | Audacity spectrogram |
| Video frames | ffmpeg -i video.mp4 frames/%04d.png |

## Critical Tips

1. Always exiftool first
2. Always strings
3. Always binwalk -e
4. Audio - check spectrogram
5. If "password" in challenge - steghide
6. Try steghide with empty password
