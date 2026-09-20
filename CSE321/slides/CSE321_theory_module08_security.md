# CSE321 Module 08: Security

Lecture slides covering operating system security, threat categories, security attack methods, program threats (Trojan horse, trap door, buffer overflow, code injection, viruses, worms), defense-in-depth, intrusion detection, and firewalls.
Pptx last modified: 03-01-2026.

## Content

### Slides 1–3: The Security Problem & Violation Categories
- **Security Definition:** A system is secure if its resources are used and accessed as intended under all circumstances (absolute security is unachievable in practice).
- **Threat:** A potential security violation.
- **Attack:** An attempt to breach security (accidental or malicious; easier to defend against accidental).
- **Security Violation Categories:**
  1. **Breach of Confidentiality:** Unauthorized reading of data.
  2. **Breach of Integrity:** Unauthorized modification of data.
  3. **Breach of Availability:** Unauthorized destruction or disruption of data/services.
  4. **Theft of Service:** Unauthorized use of resources (e.g., unauthorized server hosting).
  5. **Denial of Service (DoS):** Prevention of legitimate use by overwhelming system resources.

### Slide 4: Security Violation Methods
- **Masquerading:** Pretending to be an authorized user to bypass authentication and escalate privileges.
- **Replay Attack:** Capturing valid authentication or data packets and replaying them later (as-is or modified).
- **Man-in-the-Middle (MitM) Attack:** Intruder sits in data path between sender and receiver, intercepting and modifying communication.
- **Session Hijacking:** Intercepting an active, authenticated session to bypass credentials.
- **Privilege Escalation:** Exploiting a vulnerability to acquire elevated permissions beyond authorized level.

### Slide 5: Security Measure Levels (Four-Layered Model)
Security must be enforced across four interconnected layers (a system is as secure as its weakest link):
1. **Physical:** Physical security of data centers, server racks, workstations, and network cables.
2. **Human / Social:** Prevention of phishing, password sharing, and social engineering attacks.
3. **Operating System:** Protection domains, authentication, patch management, and audit logging.
4. **Network:** Encryption, VPNs, secure routing, intrusion prevention, and firewalls.

### Slides 6–10: Program Threats — Malware & Buffer Overflow
- **Trojan Horse:** A program that misuses its environment under the guise of legitimate functionality; may install a Remote Access Tool (RAT), spyware, or backdoors.
- **Trap Door (Backdoor):** Hidden mechanism in software or compiler circumventing normal authentication.
- **Spyware:** Software covertly monitoring user activity and keystrokes.
- **Ransomware:** Encrypts user files and demands payment for decryption keys.
- **Buffer Overflow & Code Injection:**
  - Occurs when a program writes data beyond allocated buffer boundary in memory (common in C/C++ without boundary checking).
  - Attackers overwrite function return address on stack with pointer to injected shellcode (trampoline).
  - *Countermeasures:* Code review, bounds-checking compilers, stack canaries, Address Space Layout Randomization (ASLR), and Non-Executable Stack (NX/DEP bits).

### Slides 11–13: Program Threats — Viruses
- **Virus:** Code fragment embedded in a legitimate host program; self-replicating and platform-specific.
- **Virus Dropper:** Program that deposits and installs the virus payload onto the target system.
- **Categories of Viruses:**
  - *File / Parasitic:* Attaches to executable files.
  - *Boot / Memory Sector:* Infects master boot record (MBR) or boot sector.
  - *Macro:* Embedded in documents/spreadsheets (e.g., Visual Basic macros).
  - *Polymorphic:* Changes byte signature with each infection to evade antivirus scanners.
  - *Stealth & Encrypted:* Conceals changes to files and encrypts payload.
  - *Multipartite:* Infects both boot records and program files.

### Slide 14: Program Threats — Worms
- **Worm:** Standalone self-replicating program that propagates across networks using system vulnerabilities without requiring a host program.
- **The Morris Internet Worm (1988):**
  - Exploited UNIX network features, `fingerd` buffer overflow, `sendmail` debug mode, and `rsh` trust relationships.
  - 99-line C grappling hook program uploaded main worm code.
  - Attempted dictionary attacks on user passwords. Re-infected hosts (1-in-7 chance) causing massive system overload.

### Slides 15–18: Security Defenses, Intrusion Detection & Firewalls
- **Defense in Depth:** Applying multiple layers of redundant defensive controls throughout the system.
- **Intrusion Detection Systems (IDS):**
  - *Signature-Based Detection:* Matches known attack patterns and virus signatures. Cannot detect zero-day attacks.
  - *Anomaly-Based Detection:* Establishes baseline profile of normal behavior and flags statistically significant deviations. Detects zero-day attacks, but prone to false positives/negatives.
- **Firewalls:**
  - Hardware/software barrier between trusted internal network and untrusted external network.
  - **Packet-Filtering Firewall:** Filters traffic based on IP addresses, ports, and protocols (vulnerable to IP spoofing).
  - **Application Proxy Firewall:** Inspects payload at application level (e.g., HTTP, SMTP).
  - **Personal / Host Firewall:** Runs on individual host machine.
  - **System-Call Firewall:** Monitors system calls made by processes and restricts disallowed calls.
