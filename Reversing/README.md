# Reversing Cheatsheet

## Quick Start

    file binary
    readelf -h binary
    strings binary | grep -i flag
    objdump -d binary | grep -A 20 "<main>"

## 1. Static Analysis

### radare2

    r2 -A binary

Inside r2:
    aaa              # Analyze all
    afl              # List functions
    s main           # Seek to main
    pdf              # Print disassembly
    izz              # List strings
    iI               # File info
    V                # Visual mode (q to exit)

### Ghidra

    ghidraRun

Steps:
    1. File -> New Project
    2. File -> Import File
    3. Double-click binary
    4. Yes to analyze
    5. Find main in Functions panel

### objdump

    objdump -d binary | less
    objdump -d binary | grep -A 20 "<main>"

## 2. Dynamic Analysis

### gdb (with pwndbg)

    gdb ./binary

Inside gdb:
    set disassembly-flavor intel
    disas main
    b main
    run
    ni              # Next instruction
    si              # Step into
    info registers
    x/s $rdi        # Examine string
    x/20x $rsp      # Examine stack

### strace

    strace ./binary
    strace -f ./binary 2>&1 | grep "open\|read\|write"

### ltrace

    ltrace ./binary

## 3. APK Reversing

    apktool d app.apk
    jadx-gui app.apk
    grep -ri "flag\|password" output/

## 4. Python Bytecode

    uncompyle6 binary.pyc > source.py

## 5. Pwntools Template

    from pwn import *

    elf = ELF('./binary')
    p = process('./binary')
    # p = remote('challenge.ctf.com', 1337)

    print(p.recvuntil(b'Enter:'))
    payload = b'A' * 64 + p64(0xdeadbeef)
    p.sendline(payload)
    p.interactive()

## 6. Anti-Debug Bypass

### ptrace

    LD_PRELOAD=./ptrace_stub.so ./binary

### Patching

    r2 -w binary

Inside r2:
    wa nop @ addr      # Write NOP
    wx 9090            # Write hex bytes

## Critical Tips

1. Run strings first
2. Run file first
3. Use Ghidra for complex binaries
4. Check for anti-debug (ptrace)
5. Look for dead code
6. Try dynamic if static fails
