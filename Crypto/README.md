# Crypto Cheatsheet

## Quick Start

    # Try CyberChef first: https://gchq.github.io/CyberChef/
    echo "SGVsbG8=" | base64 -d
    echo "48656c6c6f" | xxd -r -p
    echo "Hello" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

## 1. Encoding Identification

| Looks Like | Encoding | Decode |
|------------|----------|--------|
| A-Za-z0-9+/= | Base64 | base64 -d |
| A-Z2-7= | Base32 | base32 -d |
| 0-9a-f | Hex | xxd -r -p |
| 0/1 | Binary | perl -lpe '$_=pack("B*",$_)' |
| %XX | URL | python3 -c "import urllib.parse; print(urllib.parse.unquote('...'))" |

## 2. Classical Ciphers

### ROT13 / Caesar

    echo "Hello" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

### Vigenere

    pip3 install pycipher
    python3 -c "from pycipher import Vigenere; print(Vigenere('KEY').decipher('CIPHERTEXT'))"

### Substitution

Use quipqiup.com - paste ciphertext, auto-solves.

## 3. XOR

### Single-byte XOR brute force

    ciphertext = bytes.fromhex("...")
    for key in range(256):
        decoded = bytes([b ^ key for b in ciphertext])
        if b'flag' in decoded.lower() or b'CTF' in decoded:
            print(f"Key {key}: {decoded}")

## 4. RSA Attacks

### Given n, e, c

    from Crypto.Util.number import inverse, long_to_bytes
    p = ...
    q = ...
    e = 65537
    c = ...
    n = p * q
    phi = (p-1) * (q-1)
    d = inverse(e, phi)
    m = pow(c, d, n)
    print(long_to_bytes(m))

### FactorDB

Visit http://factordb.com/ and paste n.

### RsaCtfTool (Automated)

    git clone https://github.com/RsaCtfTool/RsaCtfTool.git
    cd RsaCtfTool
    python3 RsaCtfTool.py --publickey pub.pem --uncipherfile ciphertext.bin

### Common Attacks

| Attack | Condition | Tool |
|--------|-----------|------|
| Small e | e=3 | iroot(c, e) |
| Common modulus | Same n | Extended GCD |
| Wiener | Small d | RsaCtfTool |
| Fermat | Close p and q | RsaCtfTool |
| Shared prime | Two n share factor | GCD(n1, n2) |

## 5. Hash Cracking

### Identify hash

    hashid "5f4dcc3b5aa765d61d8327deb882cf99"

### Hashcat modes

| Hash | Mode |
|------|------|
| MD5 | 0 |
| SHA1 | 100 |
| SHA256 | 1400 |
| bcrypt | 3200 |
| NTLM | 1000 |

### Crack

    hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt
    john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

## 6. AES

### ECB Detection

    python3 -c "
    data = open('ciphertext.bin','rb').read()
    blocks = [data[i:i+16] for i in range(0, len(data), 16)]
    print('Repeating blocks:', len(blocks) - len(set(blocks)))
    "

### CBC Bit Flipping

If IV is controllable, flip bytes to change plaintext.

## Critical Tips

1. Try CyberChef first (Magic mode)
2. Base64 usually ends with =
3. For RSA: factor n with FactorDB
4. Rockyou.txt: /usr/share/wordlists/rockyou.txt
5. Check file length before contents
6. Use dcode.fr for classical ciphers
