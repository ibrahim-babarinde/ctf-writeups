# Burp Suite: The Basics — TryHackMe Writeup

## Overview
| Field | Details |
|-------|---------|
| Platform | TryHackMe |
| Room | Burp Suite: The Basics |
| Path | Cyber Security 101 |
| Difficulty | Easy |
| Date | May 2026 |
| Author | Ibrahim Babarinde |

---

## What The Room Is About

This room introduces Burp Suite — the industry standard web application
testing toolkit used by penetration testers and bug bounty hunters worldwide.
The room covers the core features of Burp Suite and how to use them
practically in a real web application testing scenario, rather than
just understanding them theoretically.

---

## Tools Used

- Burp Suite Community Edition
- FoxyProxy (browser extension for routing traffic through Burp)
- Chromium / Firefox browser

---

## Key Concepts Covered

### 1. What Is Burp Suite?

Burp Suite is a powerful web application security testing tool developed
by PortSwigger. At its core, it captures and enables the manipulation of
all HTTP and HTTPS traffic flowing between a browser and a web server.

This is critical in penetration testing because almost every modern
application communicates through HTTP/HTTPS. Being able to intercept,
read, and modify that traffic means you can:

- See exactly what data is being sent between client and server
- Modify requests before they reach the server
- Test how the application responds to unexpected or malicious input
- Identify vulnerabilities like SQL Injection, XSS, and authentication flaws

Think of Burp Suite as a man-in-the-middle that sits between your
browser and the target web application, giving you full visibility
and control over every request and response.

---

### 2. The Proxy

The Proxy is the heart of Burp Suite. It works by intercepting all
traffic between your browser and the web server before it is sent,
allowing you to read and modify requests and responses in real time.

**How it works in practice:**
- You configure your browser to route traffic through Burp's proxy
- Every request you make in the browser gets caught by the proxy
- You can read the full request — headers, parameters, cookies, body
- You can modify any part of it before forwarding it to the server
- The server never knows the request was intercepted and modified

This is the foundation of almost every web application attack. Before
you can exploit anything, you need to see and control the traffic.
The Proxy gives you exactly that.

---

### 3. The Repeater

The Repeater allows you to capture, modify, and resend the same HTTP
request as many times as you want. Each time you send it, you see
the server's response immediately.

**Why this matters:**
Manual exploitation is rarely a one-shot process. When testing for
vulnerabilities like SQL Injection or Cross-Site Scripting (XSS), you
need to try many different payloads and observe how the server responds
to each one. The Repeater makes this efficient — you capture the request
once, then modify and resend it dozens of times without having to
interact with the browser at all.

**Example use case:**
You find a login form and suspect it is vulnerable to SQL Injection.
You capture the login request in the Proxy, send it to the Repeater,
and start modifying the username field with different SQL payloads:

```
' OR 1=1--
' OR 'a'='a
admin'--
```

Each time you send a modified request, you watch the response to see
if the application behaves unexpectedly — which would confirm the
vulnerability.

---

### 4. The Intruder

The Intruder automates the process of sending large numbers of requests
to an endpoint, each with a different payload. It is Burp Suite's
built-in attack automation tool.

**Common use cases:**

- **Brute Force Attacks** — systematically trying thousands of username
and password combinations against a login form until one works
- **Fuzzing** — sending unexpected or malformed input to endpoints to
see how the application handles it and whether it reveals errors
or vulnerabilities
- **Parameter Tampering** — automatically testing how an application
responds to different values in a specific parameter

**How it works:**
You capture a request, mark the part you want to modify as a payload
position, load a wordlist of values, and let Intruder fire them all
automatically. You then analyze the responses to identify which
payloads produced interesting results.

Note: In the Community Edition of Burp Suite, Intruder is rate-limited
intentionally by PortSwigger. For serious brute forcing, tools like
Hydra or ffuf are faster alternatives.

---

## What I Learned

The biggest takeaway from this room was the difference between knowing
what a tool does and actually knowing how to use it.

Before this room, I understood conceptually that Burp Suite intercepts
traffic. After completing it, I understand how to set up the proxy,
route browser traffic through it, capture live requests, modify them
in the Repeater, and automate attacks with the Intruder.

That practical understanding is what matters in real penetration testing.
Tools are only useful if you can operate them under pressure, in a real
engagement, against a real target. This room built that foundation.

**Specific skills gained:**
- Setting up FoxyProxy to route browser traffic through Burp
- Intercepting and reading raw HTTP requests and responses
- Modifying live requests in the Proxy and Repeater
- Understanding how the Intruder automates payload injection
- Navigating the Burp Suite interface confidently

---

## Why Burp Suite Matters For My Path

As someone working toward penetration testing and bug bounty hunting,
Burp Suite is non-negotiable. Almost every bug bounty target is a
web application. Almost every web application vulnerability — SQL
Injection, XSS, IDOR, SSRF, authentication bypasses — is found and
exploited through HTTP traffic manipulation.

Burp Suite is the tool that makes all of that possible. Mastering it
is not optional. The next step for me is completing the
**PortSwigger Web Security Academy** — a free, world-class platform
built by the same people who made Burp Suite, covering every major
web vulnerability in depth.

---

## Resources

- [TryHackMe — Burp Suite: The Basics](https://tryhackme.com/room/burpsuitebasics)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [Burp Suite Documentation](https://portswigger.net/burp/documentation)

---

*Written by Ibrahim Babarinde | [GitHub](https://github.com/ibrahim-babarinde) | [TryHackMe](https://tryhackme.com)*
