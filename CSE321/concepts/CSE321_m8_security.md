---
type: module-notes
course: CSE321
module: 8
topics: "31–32"
title: Security
status: draft
date: 2026-07-31
exam: final
---

# CSE321 — Module 8: Security
**Topics 31–32 | Exam: Final | Silberschatz Ch.16 (16.1, 16.2, 16.6.1–16.6.4, 16.6.6) | No practice sheet — conceptual only.**

---

## Topic 31 — Security Problem, Violation Categories, Program Threats
**Ref: Silberschatz 16.1, 16.2**

### The Security Problem
A system is **secure** if its resources are used and accessed as intended under all circumstances — an ideal that's practically unachievable in full, so security is really about reducing the probability of violations to an acceptable level, at acceptable cost.

**Core security goals** (commonly abbreviated CIA, plus authenticity):

| Goal | Meaning |
|---|---|
| **Confidentiality** | Information is disclosed only to authorized parties |
| **Integrity** | Information (and system resources) can only be modified by authorized parties, in authorized ways |
| **Availability** | Authorized parties can access resources/information when needed — not disrupted by attack |
| **Authenticity** | The origin of data/communication can be verified — you know who you're actually talking to |

A related vocabulary distinction from the slides: a **threat** is a *potential* security violation; an **attack** is an actual *attempt* to breach security. Attacks can be accidental or malicious — accidental misuse is generally easier to guard against than deliberate, malicious misuse.

### Security Violation Categories

| Category | Description |
|---|---|
| **Breach of confidentiality** | Unauthorized reading of data |
| **Breach of integrity** | Unauthorized modification of data |
| **Breach of availability** | Unauthorized destruction of data, or denying legitimate access |
| **Theft of service** | Unauthorized use of resources (e.g., an attacker running their own processes on a compromised system) |
| **Denial of service (DoS)** | Preventing legitimate users from using the system, typically by overwhelming it rather than destroying data |

### Security Violation Methods (slide-only — a distinct list from the categories above; categories describe *what* is violated, methods describe *how*)

| Method | Mechanism |
|---|---|
| **Masquerading** | Breach of authentication — pretending to be an authorized user to escalate privileges |
| **Replay attack** | Capturing and re-sending a legitimate message, as-is or with modification |
| **Man-in-the-middle attack** | Intruder sits inside the data flow, masquerading as the sender to the receiver and as the receiver to the sender simultaneously |
| **Session hijacking** | Intercepting an already-established (already-authenticated) session, bypassing the need to authenticate at all |
| **Privilege escalation** | A common attack pattern generally — gaining access beyond what a user or resource is actually supposed to have |

> **Exam note:** Violation *categories* (confidentiality/integrity/availability/theft-of-service/DoS) answer "what got violated." Violation *methods* (masquerading/replay/MITM/session-hijacking/privilege-escalation) answer "how was it done." Don't conflate the two lists if a question asks for one specifically.

### Levels of Security Concern (corrected — 4 official levels per the slide deck)
A system's overall security depends on the weakest of several layers. **The slides name exactly four formal levels** — note this replaces an earlier draft of this note that listed "Human" as a 4th formal level; the slides instead fold human/social-engineering risk in as a cross-cutting concern, not a named level:

| Level | Concern |
|---|---|
| **Physical** | Data centers, servers, and connected terminals must be physically secured |
| **Application** | Malicious applications themselves can cause security problems |
| **Operating System** | The OS must protect itself and its resources from accidental/malicious violations (debugging access is one specific concern named in the slides) |
| **Network** | Data in transit can be intercepted, communications interrupted, or the network used as a DoS vector |

> **Exam note:** "Security is as weak as the weakest link in the chain" (the slide's own phrasing). **Humans remain a real risk** via phishing and social-engineering — the slides state this explicitly, just as a cross-cutting risk factor rather than as a formal 5th level. If a question asks to *name the four levels*, answer Physical/Application/OS/Network; if it asks about the weakest link generally, human/social-engineering is still fair game to raise.

### Program Threats
Threats where a legitimate program is subverted to perform unauthorized actions, often exploiting the fact that it runs with the privileges of the user who invoked it.

| Threat | Mechanism |
|---|---|
| **Trojan Horse** | A program that appears to do something legitimate/useful but secretly performs a malicious action as well. Often exploits a user's own broad access rights — the malicious code simply runs with whatever privileges the tricked user already has |
| **Trap Door (Backdoor)** | A hole deliberately left in a program's design (often by its original developer) allowing unauthorized access, bypassing normal security checks — hard to detect via ordinary code review since it's an intentional, hidden inclusion |
| **Logic Bomb** | Code that lies dormant until triggered by a specific condition (a date, an event, an action) — at which point it executes its malicious payload |
| **Stack / Buffer Overflow** | Exploits a program that fails to bounds-check input, writing past the end of an allocated buffer to overwrite adjacent memory (e.g., a function's return address on the stack) — used to redirect execution into attacker-supplied code. One of the most common exploitation techniques historically. Often uses a **trampoline** technique to redirect execution into the injected code. Root cause: languages like C/C++ allow direct memory access via pointers with no automatic bounds checking; **code review** (programmers checking each other's code for logic flaws) is a named mitigation |

**Malware umbrella terms (slide-specific, likely testable definitions):**
- **Malware** — general term for software designed to exploit, disable, or damage a computer.
- **Spyware** — frequently installed alongside legitimate software; displays ads and/or captures user data without consent. Slide-specific stat: **up to 80% of spam is delivered by spyware-infected systems**.
- **Ransomware** — encrypts the victim's data, demanding payment to unlock it.
- **RAT (Remote Access Tool)** — many attacks' end goal is leaving one of these behind for repeated future access, not just a single one-time compromise.
- **Code-injection attack** — occurs when system code isn't itself malicious but has bugs that let executable code be added/modified; goal is typically a buffer overflow that plants and executes attacker code. Can be carried out by **"script kiddies"** using pre-built tools that exploit already-known vulnerabilities, without the attacker needing to discover the flaw themselves.
- All of the above ultimately work by violating the **Principle of Least Privilege**.

### Viruses
Code **embedded within a legitimate program**, designed to self-replicate and spread — usually **requires human action** to propagate (e.g., running an infected file, opening an infected document) unlike a worm.

| Virus type | Characteristic |
|---|---|
| **File** | Attaches to an executable file |
| **Boot** | Infects the boot sector, executes at every system startup |
| **Macro** | Written in a macro language (e.g., embedded in Office documents), platform-independent |
| **Source code** | Modifies source code to include the virus, so it compiles into every build |
| **Polymorphic** | Changes its own signature/form on each infection to evade signature-based detection |
| **Encrypted** | Carries itself in encrypted form plus a decryption routine, to hide its signature from scanners |
| **Stealth** | Actively modifies system information (e.g., file sizes reported to the OS) to conceal its own presence |
| **Tunneling** | Attempts to install itself underneath (before) antivirus software, so the antivirus can't intercept it |
| **Multipartite** | Infects multiple targets — e.g., both files and boot sectors — via multiple methods |
| **Armored** | Wraps itself in defenses specifically designed to make disassembly/analysis by researchers harder — a distinct evasion goal from Polymorphic/Encrypted/Stealth above (those hide the signature at scan-time; Armored resists *analysis* of the virus itself) |

A virus is typically introduced onto a system by a **virus dropper** — the delivery mechanism, distinct from the virus payload itself. Viruses are highly specific to a given CPU architecture, OS, and application (unlike worms, which are more general-purpose) and are commonly borne via email or as a document macro.

### Worms
Unlike a virus, a **worm is a standalone program** — it does **not** need to attach to a host program, and does **not** need human action to spread. It self-replicates via a **spawn mechanism** and propagates autonomously, typically across a network, consuming resources (bandwidth, CPU, memory) as it spreads — often causing denial-of-service effects purely as a side effect of its replication rate.

> **The Internet/Morris Worm (1988) — slide covers this in specific, quotable detail, not just as a passing example:**
> - Exploited UNIX networking features and specific bugs in the `finger` and `sendmail` programs.
> - Exploited the **trust-relationship mechanism used by `rsh`** to reach "friendly" systems without needing a password.
> - Used a small **"grappling hook" program** to bootstrap-upload the actual (larger) main worm program onto each newly-infected host.
> - The grappling hook itself was only **99 lines of C code**.
> - After hooking a system, it uploaded the main code and then attempted to attack systems connected to that one, propagating further.
> - It also tried to break into other local user accounts on an already-infected system via password guessing.
> - **Reinfection-avoidance logic with a twist:** if a target system was already infected, the worm would normally abort — **except every 7th time**, where it would proceed to infect anyway. (This quirk is exactly the kind of specific, quotable fact this exam format tends to draw questions from.)

> **Key distinction (frequently tested):** Virus = needs a host program + human action to spread. Worm = standalone, self-propagating, no human action needed.

---

## Topic 32 — Security Defenses: Cryptography, Firewalls, Intrusion Detection
**Ref: Silberschatz 16.6.1–16.6.4, 16.6.6**

### Defense in Depth (slide's framing for this whole topic)
The slides introduce Topic 32 under this umbrella term: **defense in depth** — the most common security theory, relying on multiple overlapping layers of security rather than any single perfect defense. Two terms worth distinguishing precisely:
- **Security policy** — describes what is actually being secured (the target state).
- **Vulnerability assessment** — compares the system/network's *real* current state against that policy, to find gaps.

Everything below (cryptography, firewalls, intrusion detection) is one layer within that defense-in-depth model, not a standalone complete solution on its own.

> **Slide-verification note:** the theory slide deck's "Implementing Security Defences" section does **not** cover cryptography (no symmetric/asymmetric, no DES/AES/RSA anywhere in the deck) — the entire Cryptography subsection below is Silberschatz-textbook-only, same status as Peterson's Solution in Module 4 and Contiguous Allocation in Module 6. Still in the confirmed Final syllabus range, but lower slide-emphasis than firewalls/IDS/virus-protection, which the slides do cover directly.

### Cryptography
The foundational tool for securing communication over an insecure medium — used for confidentiality, integrity verification, and authentication.

**Symmetric-key encryption:**
- The **same key** is used to both encrypt and decrypt.
- Fast — suitable for encrypting large volumes of data.
- **Key distribution problem:** both parties need the same secret key beforehand, and that key itself must be exchanged over some channel — which is itself a security risk if that channel is insecure.
- Examples: **DES**, **AES**.

**Asymmetric-key (public-key) encryption:**
- Each party has a **key pair**: a **public key** (freely shared) and a **private key** (kept secret).
- Data encrypted with the public key can only be decrypted with the matching private key (and vice versa, used for digital signatures).
- Solves the key-distribution problem — the public key can be shared openly, since only the private key can decrypt.
- Significantly **slower** than symmetric encryption — in practice, often used just to securely exchange a symmetric session key, after which the bulk of the communication uses fast symmetric encryption (a hybrid approach).
- Example: **RSA**.

| | Symmetric | Asymmetric |
|---|---|---|
| Keys | One shared secret key | Public/private key pair |
| Speed | Fast | Slow |
| Key distribution | Hard (needs secure channel) | Easy (public key shared openly) |
| Typical use | Bulk data encryption | Key exchange, digital signatures, authentication |

### Authentication
Verifying that a user/entity is who they claim to be, before granting access.

| Method | Basis |
|---|---|
| **Passwords** | Something you *know* |
| **Biometrics** | Something you *are* (fingerprint, retina, etc.) |
| **Security tokens / two-factor authentication (2FA)** | Something you *have*, combined with something you know — significantly stronger than a password alone, since compromising both factors simultaneously is harder |

### Firewalls
A system (hardware, software, or both) placed between a trusted internal network/host and an untrusted external network (typically the internet), limiting network access between the two security domains according to a security policy.

| Type | Mechanism |
|---|---|
| **Network (packet-filtering) firewall** | Inspects individual packets (source/destination IP, port, protocol) against a rule set; allows or drops each packet independently. Sits between trusted/untrusted hosts |
| **Application-level (proxy) firewall** | Understands the application-layer protocol itself (e.g., SMTP) and can control it directly — more thorough but more overhead |
| **Personal firewall** | Software layer running on an individual host (rather than at the network perimeter); can monitor/limit traffic to and from just that host |
| **System-call firewall** | Monitors important system calls and applies rules to them directly (e.g., "this program may execute that system call") — a firewall concept applied *inside* a single machine, not at a network boundary |

**Firewall weaknesses (slide-specific — attacks against the firewall itself, not just what it defends against):**
- **Tunneling:** a disallowed protocol is smuggled inside an allowed one (e.g., telnet traffic carried inside HTTP) to bypass firewall rules that only inspect the outer protocol.
- **Spoofing:** firewall rules are commonly based on hostname or IP address, both of which **can be spoofed** — a rule that trusts "traffic from IP X" is only as strong as the assumption that the IP can't be forged.

### Intrusion Detection (and Prevention)
Presented in the slides as one layer of the broader "Implementing Security Defences" picture, alongside virus protection and auditing/logging.

| System | Role |
|---|---|
| **IDS (Intrusion Detection System)** | Monitors system/network activity and **alerts** on suspected intrusions, but does not itself block them |
| **IPS (Intrusion Prevention System)** | Monitors activity and can **actively block/prevent** the detected threat, not just alert |

**Detection approaches:**

| Approach | Mechanism | Trade-off |
|---|---|---|
| **Signature-based detection** | Matches activity against a database of known attack patterns/signatures | Low false-positive rate for known attacks, but **cannot detect novel (zero-day) attacks** not yet in the signature database |
| **Anomaly-based detection** | Builds a baseline of "normal" behavior, flags significant deviations from it | **Can detect zero-day attacks** (slide-explicit advantage), but both false-positives and false-negatives remain a real problem |

> **Exam note:** The signature-vs-anomaly trade-off mirrors the general detection dilemma seen elsewhere in security: known-pattern matching is precise but blind to new threats; behavioral/statistical detection generalizes (including to zero-days) but is noisier.

**Other defense-in-depth layers named in the slides (brief, but worth recognizing):**
- **Virus protection:** scanning all programs (or programs at execution time) for known virus patterns, OR running suspect code in a **sandbox** so it can't damage the real system even if malicious.
- **Auditing, accounting, and logging** of system/network activity — a detective (after-the-fact) control, not a preventive one.
- **Practicing safe computing** — avoiding known-bad sources of infection, downloading only from trusted sites, etc. — a human-layer defense, tying back to the "humans are a risk too" point under security levels above.

---

## Final PYQ Priority (see [CSE321_final_probable_questions](../pyq/CSE321_final_probable_questions.md))
Based on 8 sampled Final sittings: **LOW-MEDIUM** — appeared in only 1 of the 4 most recent sittings (#12 on the ranked list), always small (5 marks total, split across 2 small scenarios). When it does appear, it's specifically: a buffer-overflow-vulnerability code-spotting question, and a security-violation-**method** identification (masquerading/replay/MITM/session-hijacking/privilege-escalation — see the Topic 31 subsection added in this session's audit). **Cryptography, firewalls, and IDS/IPS have never appeared in any sitting sampled**, old or new, consistent with the slide deck itself omitting cryptography entirely (see the slide-verification note under Topic 32) — this is the lowest-yield material in the entire post-Midterm syllabus. If time is short, prioritize the buffer-overflow/violation-method content over the rest of this module.

## Module 8 — Quick Review

| # | Topic | Must Know |
|---|---|---|
| 31 | Security problem & threats | CIA(+Authenticity) goals; threat vs attack; 5 violation **categories** (what) vs 5 violation **methods** (how — masquerading/replay/MITM/session-hijacking/privilege-escalation); **4 official security levels: Physical/Application/OS/Network** (human risk is cross-cutting, not a formal 5th level); program threats incl. malware/spyware/ransomware/RAT/code-injection; virus types (**10** kinds incl. Armored); worm vs virus distinction; Morris Worm specifics (finger/sendmail, rsh trust, grappling hook, 99 lines, every-7th-time reinfection) |
| 32 | Security defenses | Defense in depth; security policy vs vulnerability assessment; **cryptography is textbook-only, not in slides**; authentication factors (know/are/have, 2FA); 4 firewall types incl. system-call firewall; tunneling/spoofing as firewall weaknesses; IDS (alert-only) vs IPS (blocks); signature- vs anomaly-based detection (anomaly catches zero-days); virus protection = scanning + sandboxing; auditing/logging |

### Quick Reference — Virus vs Worm
```
Virus: needs a host program + human action to spread
Worm:  standalone, self-propagating, no human action needed (uses a spawn mechanism)
```

### Quick Reference — Violation Categories vs Methods
```
Categories (WHAT was violated): confidentiality, integrity, availability, theft of service, DoS
Methods    (HOW it was done):   masquerading, replay, man-in-the-middle, session hijacking, privilege escalation
```

### Quick Reference — Encryption Trade-off
```
Symmetric:  1 shared key   | fast   | key-distribution problem | DES, AES
Asymmetric: public/private | slow   | solves key distribution  | RSA
Common practice: asymmetric to exchange a session key, then symmetric for bulk data
```

### Quick Reference — Detection Trade-off
```
Signature-based: precise on known attacks, blind to new ones
Anomaly-based:   catches novel attacks, higher false-positive rate
```
