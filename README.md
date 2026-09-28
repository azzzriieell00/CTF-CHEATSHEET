# CTF-CHEATSHEET

Professional CTF competition reference. Commands, payloads, and workflows for fast solving.

## Categories

| Category | Folder | Focus |
|----------|--------|-------|
| Web | Web/ | SQLi, XSS, IDOR, SSRF, LFI, Auth Bypass |
| Crypto | Crypto/ | RSA, AES, XOR, Classical Ciphers, Hashing |
| Forensics | Forensics/ | PCAP, Memory, Disk, File Carving |
| Reversing | Reversing/ | ELF, APK, Anti-Debug, Patching |
| Stego | Stego/ | Image, Audio, Video, Text |
| Misc | Misc/ | OSINT, Encoding, Miscellaneous |

## Quick Reference - Most Common Commands

### Reconnaissance
    file <file>
    strings <file>
    exiftool <file>
    binwalk <file>
    binwalk -e <file>

### Web Testing
    curl -s "URL?id=1'"
    curl -s "URL?q=<script>alert(1)</script>"
    ffuf -u URL/FUZZ -w wordlist -ac

### Crypto
    echo "BASE64" | base64 -d
    hashcat -m 0 hash.txt rockyou.txt
    python3 -c "print(bytes.fromhex('HEX'))"

### Forensics
    tshark -r capture.pcap -z io,phs
    vol -f memory.raw windows.pslist
    sudo mount -o ro,loop disk.img /mnt

### Reversing
    r2 -A binary
    ghidraRun
    gdb ./binary

### Stego
    steghide extract -sf image.jpg
    zsteg -a image.png
    audacity audio.wav

## The 15-Minute Rule

If a challenge takes over 15 minutes with no progress:
1. Move to another challenge
2. Come back with fresh eyes later
3. Share with a teammate
4. Do not tunnel vision

## Challenge Decision Tree

    What did they give you?
    |-- A URL
    |   |-- Login form? Try SQLi, default creds
    |   |-- Search box? Try XSS, SQLi
    |   |-- URL params? Try IDOR, SQLi, LFI
    |   |-- API endpoint? Test with curl
    |-- A file (.pcap, .raw, .img, .jpg, .exe)
    |   |-- .pcap - Wireshark/tshark
    |   |-- .raw/.mem - Volatility
    |   |-- .img/.dd - Autopsy/Sleuth Kit
    |   |-- .jpg/.png - exiftool, strings, binwalk
    |   |-- .exe/.elf - radare2, Ghidra
    |   |-- .pyc - uncompyle6
    |-- Numbers (RSA)
    |   |-- FactorDB, RsaCtfTool
    |-- Encoded string
    |   |-- CyberChef magic mode
    |-- Nothing obvious
        |-- Check metadata, strings, binwalk

## Essential Tools Installation

    sudo apt update
    sudo apt install -y \
        curl wget git python3 python3-pip \
        exiftool steghide binwalk foremost sleuthkit \
        wireshark tshark hashcat john ffuf gobuster nmap \
        gdb radare2 hexedit xxd file ruby-full zlib1g-dev \
        sonic-visualiser audacity p7zip-full unzip zip \
        pdfgrep poppler-utils seclists

    pip3 install pwntools pycryptodome requests beautifulsoup4
    sudo gem install zsteg

## Encoding Reference

| Encoding | Example | Decode Command |
|----------|---------|----------------|
| Base64 | SGVsbG8= | echo "..." \| base64 -d |
| Base32 | JBSWY3DP | echo "..." \| base32 -d |
| Hex | 48656c6c6f | echo "..." \| xxd -r -p |
| Binary | 01001000 | perl -lpe '$_=pack("B*",$_)' |
| URL | %48%65 | python3 -c "import urllib.parse; print(urllib.parse.unquote('...'))" |
| ROT13 | Uryyb | echo "..." \| tr 'A-Za-z' 'N-ZA-Mn-za-m' |

## Flag Formats

| Competition | Format |
|-------------|--------|
| Hack4Gov | flag{...} or HACK4GOV{...} |
| picoCTF | picoCTF{...} |
| HTB | HTB{...} |
| Generic | flag{...} or CTF{...} |

## Time Management

| Difficulty | Points | Time Budget |
|------------|--------|-------------|
| Easy | 1-30 | 5-10 min |
| Moderate | 31-70 | 15-25 min |
| Hard | 71-100 | 30-45 min |

Priority: Easy challenges first. 10 Easy (300 pts) beats 3 Hard (300 pts).

## Team Communication

When stuck:
1. Post challenge name
2. Post current progress
3. Post exact command/output
4. Ask for hint

Share hints freely. Do not solve for teammates unless asked.

---
Last Updated: 2026-09-28
Version: 1.0
