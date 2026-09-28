# Forensics Cheatsheet

## Quick Start

    file suspicious_file
    strings -n 8 suspicious_file | grep -i flag
    exiftool suspicious_file
    binwalk suspicious_file
    binwalk -e suspicious_file

## 1. PCAP Analysis

### Wireshark (GUI)

    wireshark capture.pcap

### Tshark (CLI)

    tshark -r capture.pcap -q -z io,phs
    tshark -r capture.pcap -Y "http.request" -T fields -e http.host -e http.request.uri
    tshark -r capture.pcap -Y "ftp.request.command==USER || ftp.request.command==PASS" -T fields -e ftp.request.arg
    tshark -r capture.pcap --export-objects http,extracted/

### Wireshark Filters

    http.request.method == "POST"
    ftp
    dns
    tcp.stream eq 0
    http contains "flag"

## 2. Memory Analysis

### Volatility 3

    vol -f memory.raw windows.info
    vol -f memory.raw windows.pslist
    vol -f memory.raw windows.netscan
    vol -f memory.raw windows.cmdline
    vol -f memory.raw windows.dumpfiles --pid 1234
    vol -f memory.raw windows.strings | grep -i flag

## 3. Disk Images

### Mount read-only

    file disk.img
    mmls disk.img
    sudo mount -o ro,loop,offset=$((512*2048)) disk.img /mnt/forensics

### Sleuth Kit

    fls -r -p disk.img
    icat disk.img 1234 > extracted_file
    fls -d -r disk.img

## 4. Image Analysis

    file image.jpg
    exiftool image.jpg
    strings image.jpg | grep -i flag
    binwalk image.jpg
    binwalk -e image.jpg
    steghide extract -sf image.jpg

## 5. PDF Analysis

    pdftotext document.pdf output.txt
    pdfgrep -i "flag" document.pdf
    exiftool document.pdf
    pdfimages -all document.pdf extracted/

## 6. Office Files

    exiftool document.docx
    olevba document.docx
    unzip document.docx -d extracted/

## 7. Archives

    unzip archive.zip
    unrar x archive.rar
    7z x archive.7z

    zip2john archive.zip > hash.txt
    rar2john archive.rar > hash.txt
    john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

## Critical Tips

1. Always run file first
2. Always check metadata with exiftool
3. Always run strings
4. Always try binwalk -e
5. Check timestamps with stat
6. Look for files after EOF marker
