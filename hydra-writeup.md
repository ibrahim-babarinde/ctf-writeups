# Hydra — TryHackMe Writeup

## Overview
| Field | Details |
|-------|---------|
| Platform | TryHackMe |
| Room | Hydra |
| Path | Cyber Security 101 |
| Difficulty | Easy |
| Date | May 2026 |
| Author | Ibrahim Babarinde |

---

## What The Room Is About

This room introduces Hydra — one of the most widely used online
brute force tools in penetration testing. The room covers what Hydra
is, how it works, what protocols it supports, and how to use it
practically to brute force login pages and services. The goal is to
move beyond theoretical understanding and actually use Hydra against
real targets in a controlled environment.

---

## Tools Used

- Hydra
- rockyou.txt wordlist
- Terminal / Command Line

---

## Key Concepts Covered

### 1. What Is Hydra?

Hydra is a fast and flexible online password cracking tool used in
penetration testing to brute force login credentials across a wide
range of protocols and services. It works by systematically trying
username and password combinations from a wordlist against a target
login until it finds one that works.

The key word here is **online** — Hydra attacks live, running services
directly over the network. This is different from offline password
cracking tools like Hashcat or John the Ripper, which crack password
hashes that have already been obtained from a system.

Hydra is used during the **initial access** phase of a penetration
test, when you have identified a login page or service and want to
test whether weak or default credentials can get you in.

---

### 2. Protocols Hydra Can Attack

One of Hydra's biggest strengths is the sheer number of protocols
it supports. This makes it useful across almost any penetration
testing scenario involving authentication. Supported protocols include:

- SSH
- FTP
- HTTP / HTTPS (GET and POST form logins)
- SMB
- Telnet
- RDP
- VNC
- SMTP / IMAP / POP3
- MySQL / PostgreSQL
- Cisco AAA / Cisco Auth / Cisco Enable
- Asterisk
- AFP (Apple Filing Protocol)
- CVS
- Firebird
- SSHKEY
- Subversion
- TeamSpeak (TS2)
- VMware-Auth
- XMPP
- And many more

This range means Hydra is relevant whether you are attacking a web
application login, a remote SSH server, a database, or a network
device.

---

### 3. What Is A Wordlist?

A wordlist is a text file containing a large collection of possible
passwords, one per line. Hydra reads through the wordlist and tries
each password against the target login automatically.

The quality of your wordlist directly determines your chances of
success. The most famous wordlist in cybersecurity is **rockyou.txt**
— a real password list compiled from a massive data breach that exposed
over 14 million plaintext passwords. Because it contains passwords
that real people actually used, it is extremely effective against
targets using weak or common passwords.

**Why wordlists matter:**
Brute force attacks that try every possible character combination
(called a pure brute force) can take years against a strong password.
Wordlist attacks are far more efficient because they focus on passwords
that humans are actually likely to use — common words, names, dates,
and simple patterns.

---

### 4. Breaking Down A Hydra Command

```bash
hydra -l admin -P rockyou.txt ssh://10.10.10.1
```

Let's break this down part by part:

| Part | Meaning |
|------|---------|
| `hydra` | Calls the Hydra tool |
| `-l admin` | Sets a single username to test — in this case "admin" |
| `-P rockyou.txt` | Loads the rockyou.txt wordlist as the password list |
| `ssh://` | Specifies the protocol being attacked — SSH in this case |
| `10.10.10.1` | The IP address of the target machine |

So this command is saying: *"Try logging into the SSH service at
10.10.10.1 as the user 'admin', using every password in rockyou.txt
until one works."*

---

### 5. Key Flags — The Difference Between Lowercase and Uppercase

This is one of the most important things to understand about Hydra.
The case of the flag completely changes what it does:

| Flag | Meaning |
|------|---------|
| `-l` | Single username — you specify one exact username to test |
| `-L` | Username list — loads a file containing multiple usernames to test |
| `-p` | Single password — you specify one exact password to test |
| `-P` | Password list — loads a wordlist file containing multiple passwords |

**Examples:**

Testing one username with a password list:
```bash
hydra -l admin -P rockyou.txt ssh://10.10.10.1
```

Testing multiple usernames with multiple passwords:
```bash
hydra -L usernames.txt -P rockyou.txt ssh://10.10.10.1
```

Testing one username with one password:
```bash
hydra -l admin -p password123 ssh://10.10.10.1
```

Understanding these flags is critical because using the wrong one
can mean your attack never finds the credentials even if they are
in your list.

---

### 6. HTTP Form Brute Forcing

Beyond SSH, one of the most common real-world uses of Hydra is
attacking web application login forms over HTTP/HTTPS. The syntax
looks like this:

```bash
hydra -l admin -P rockyou.txt 10.10.10.1 http-post-form \
"/login:username=^USER^&password=^PASS^:Invalid credentials"
```

Breaking this down:
- `http-post-form` — tells Hydra this is an HTTP POST form login
- `/login` — the path to the login page
- `username=^USER^&password=^PASS^` — the form fields, where `^USER^`
and `^PASS^` are replaced by Hydra with each username and password
- `Invalid credentials` — the failure message Hydra looks for to know
a login attempt failed

---

## What I Learned

This room taught me how to practically use Hydra to brute force login
services — moving from knowing what brute forcing is conceptually to
actually executing it against a target.

The most important lessons were:

**1. Wordlists are everything**
The success of a brute force attack depends almost entirely on the
quality of your wordlist. rockyou.txt is the standard starting point
but in real engagements, custom wordlists built from information about
the target — their company name, location, founding year — are often
more effective.

**2. Lowercase vs uppercase flags matter**
The difference between `-l` and `-L`, `-p` and `-P` is small to look
at but massive in impact. Getting this wrong means your attack fails
silently — Hydra runs but never tries the right credentials.

**3. Brute forcing is loud**
Hydra sends hundreds or thousands of requests to a target in a short
time. In a real engagement this generates a lot of network noise and
will almost certainly trigger intrusion detection systems. Understanding
this helps me think about stealth and timing in real penetration tests.

**Next step:** Apply Hydra in real CTF scenarios on TryHackMe and
HackTheBox, and eventually use it during practice penetration tests
in my home lab against Metasploitable.

---

## Important Note On Ethics

Hydra is a powerful tool that can cause serious harm if used
irresponsibly. Brute forcing a login service you do not have
permission to test is illegal under computer misuse laws in most
countries. Every target I practice on is either a dedicated CTF
platform, a machine I own, or a machine I have explicit written
permission to test.

---

## Resources

- [TryHackMe — Hydra](https://tryhackme.com/room/hydra)
- [Hydra GitHub Repository](https://github.com/vanhauser-thc/thc-hydra)
- [SecLists — Wordlist Collection](https://github.com/danielmiessler/SecLists)
- [rockyou.txt Explained](https://www.kali.org/tools/wordlists/)

---

*Written by Ibrahim Babarinde | [GitHub](https://github.com/ibrahim-babarinde) | [TryHackMe](https://tryhackme.com)*
