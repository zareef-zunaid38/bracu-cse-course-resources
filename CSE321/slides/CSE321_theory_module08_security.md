---
source: CSE321_theory_module08_security.pptx (the original, included in this repo)
fidelity: the summary above the divider was rewritten from the deck on 2026-09-20 after an audit found errors in the earlier version; it is still a condensed paraphrase and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 8 embedded image(s); informational images are described in the summary)
source-mtime: 2026-01-03T06:37:30
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE321 Module 08: Security

Lecture slides (18) on operating-system security: the security problem, violation categories and methods, the levels at which security must be applied, program threats (Trojan horses, trap doors, malware, buffer overflows and code injection, viruses, worms), the four-layered model, defences (defence in depth, intrusion detection, virus protection) and firewalls.
Pptx last modified: 03-01-2026.
Rewritten 2026-09-20 directly from the deck (an audit found the earlier summary unreliable). Figures below were read from renders of slides 7, 8, 10, 11, 13, 15 and 18.

## Content

### Slides 2–5: The security problem
- **Slide 2:** a system is secure if resources are used and accessed as intended under all circumstances — unachievable. Intruders (crackers) attempt to breach security; a *threat* is a potential violation, an *attack* is an attempt to breach security; attacks can be accidental or malicious; accidental misuse is easier to protect against.
- **Slide 3, violation categories:** breach of confidentiality (unauthorized reading of data); of integrity (unauthorized modification); of availability (unauthorized destruction); theft of service (unauthorized use of resources); denial of service (DOS — preventing legitimate use).
- **Slide 4, violation methods:** *masquerading* (breach authentication — pretending to be an authorized user to escalate privileges); *replay attack* (as is or with message modification); *man-in-the-middle* (the intruder sits in the data flow, masquerading as sender to receiver and vice versa); *session hijacking* (intercepting an established session to bypass authentication); *privilege escalation* (access beyond what a user or resource should have; very common).
- **Slide 5, security measure levels:** absolute security is impossible, so make the cost to a perpetrator high enough to deter most intruders. Four levels are needed: *physical* (data centres, servers, terminals), *application* (malicious apps), *operating system* (protection mechanisms, debugging), *network* (intercepted communications, interruption, DOS). Security is as weak as the weakest link; humans are a risk too (phishing, social engineering).

### Slides 6–14: Program threats
- **Slide 6:** *Trojan horse* — a code segment that misuses its environment by exploiting the mechanisms that let programs written by users be executed by other users; includes spyware, pop-up windows, covert channels; up to 80% of spam is delivered by spyware-infected systems. *Trap door* — a specific user identifier or password that circumvents normal security procedures; could even be included in a compiler.
- **Slide 7:** *malware* is software designed to exploit, disable or damage a computer; a Trojan horse acts clandestinely; *spyware* is frequently installed with legitimate software to display ads and capture user data; *ransomware* locks data by encryption and demands payment; others include trap doors and logic bombs. All try to violate the principle of least privilege, and the goal is frequently to leave behind a Remote Access Tool (RAT) for repeated access. The slide quotes Jerome H. Saltzer (Multics, 1974) on the principle of least privilege: every program and every privileged user should operate using the least amount of privilege necessary to complete the job, to reduce the potential interactions among privileged programs and develop confidence that unintentional, unwanted or improper uses of privilege do not occur.
- **Slide 8, buffer-overflow example (C):**
  ```c
  #include <stdio.h>
  #define BUFFER_SIZE 256
  int main(int argc, char *argv[])
  {
      char buffer[BUFFER_SIZE];
      if (argc < 2)
          return -1;
      else {
          strcpy(buffer, argv[1]);
          return 0;
      }
  }
  ```
  Code review — programmers reviewing each other's code for logic flaws and programming flaws — can help.
- **Slides 9–10, code injection:** an attack where system code is not malicious but has bugs that let executable code be added or modified; it results from poor or insecure programming paradigms, commonly in low-level languages such as C or C++ with direct memory access through pointers; the goal is a buffer overflow in which code is placed in a buffer and its execution triggered; can be run by "script kiddies" using tools written by others that exploit known identifiers. Figure: stack layout (buffer with padding, other variables, return address) and three outcomes of an overflow — unnoticed, logic subversion/DOS (program crash) when variables or the return address are overwritten, and code injection when the injected code and a new return address are written. Frequently a *trampoline* is used to steer execution: dummy opcodes, shellcode, and the address of the shellcode.
- **Slide 11, viruses:** a code fragment embedded in a legitimate program; self-replicating, designed to infect other computers; very specific to CPU architecture, operating system and applications; usually borne by e-mail or as a macro. Example shown — a Visual Basic macro that reformats the hard drive: `Sub AutoOpen() Dim oFS; Set oFS = CreateObject("Scripting.FileSystemObject"); vs = Shell("c:command.com /k format c:", vbHide) End Sub`.
- **Slide 12:** a *virus dropper* inserts the virus onto the system; categories (thousands of viruses): file/parasitic, boot/memory, macro, source code, polymorphic (to avoid a virus signature), encrypted, stealth, tunneling, multipartite, armored.
- **Slide 13, a boot-sector virus (flow diagram):** the virus copies the boot sector to an unused location X → replaces the original boot block with itself → at system boot it decreases physical memory and hides above the new limit → attaches to the disk read-write interrupt and monitors all disk activity → then: whenever a new removable R/W disk is installed it infects that as well; it blocks attempts by other programs to write the boot sector; it has a logic bomb to wreak havoc at a certain date.
- **Slide 14, worms:** standalone programs using a spawn mechanism. The Internet worm exploited UNIX networking features (remote access) and bugs in the `finger` and `sendmail` programs, and the trust relationship used by `rsh` to reach friendly systems without a password; a *grappling hook* program uploaded the main worm program (99 lines of C); the hooked system then uploaded the main code and attacked connected systems, also tried to break into other users' accounts on the local system by password guessing; if the target was already infected it aborted, except every 7th time.

### Slides 15–18: Defences
- **Slide 15, four-layered model** (attacks → layer ← prevention): application — logic bugs, design flaws, code injections ← sandboxing, software restrictions; operating system — insecure defaults, platform vulnerabilities ← patches, reconfiguration, hardening; network — sniffing, spoofing, masquerading ← encryption, authentication, filtering; physical — console access, hardware-based attacks ← guards, vaults, device data encryption.
- **Slide 16, implementing defences:** *defence in depth* (multiple layers) is the most common theory; a *security policy* describes what is being secured; *vulnerability assessment* compares the real state of the system/network with the policy; *intrusion detection* tries to detect attempted or successful intrusions — signature-based (known bad patterns) or anomaly detection (differences from normal behaviour, can catch zero-day attacks; false positives and false negatives are a problem); *virus protection* searches programs (all, or at execution) for known virus patterns, or runs them in a sandbox; *auditing, accounting and logging* of system or network activity; practise safe computing (avoid sources of infection, download only from "good" sites).
- **Slide 17, firewalls:** a network firewall sits between trusted and untrusted hosts and limits access between the two security domains. It can be tunneled or spoofed — tunneling lets a disallowed protocol travel inside an allowed one (e.g. telnet inside HTTP), and rules based on hostname or IP address can be spoofed. A *personal firewall* is a software layer on a host that monitors/limits its traffic; an *application proxy firewall* understands an application protocol and can control it (e.g. SMTP); a *system-call firewall* monitors important system calls and applies rules to them (e.g. this program may execute that call).
- **Slide 18, domain separation via firewall:** a firewall separates the Internet, the company's computers and a DMZ; allowed flows shown: Internet access from the company's computers, DMZ access from the Internet, and access between the DMZ and the company's computers.

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets; list numbering, formatting and images are not reproduced -->

<!-- slide 1 -->
### OPERATING SYSTEMS Security

<!-- slide 2 -->
### The Security Problem
System secure if resources used and accessed as intended under all circumstances.
  Unachievable.
Intruders (crackers) attempt to breach security.
Threat is potential security violation.
Attack is attempt to breach security.
Attack can be accidental or malicious.
Easier to protect against accidental than malicious misuse.

<!-- slide 3 -->
### Security Violation Categories
Breach of confidentiality: Unauthorized reading of data.
Breach of integrity: Unauthorized modification of data.
Breach of availability: Unauthorized destruction of data.
Theft of service: Unauthorized use of resources.
Denial of service (DOS): Prevention of legitimate use.

<!-- slide 4 -->
### Security Violation Methods
Masquerading (breach authentication): Pretending to be an authorized user to escalate privileges.
Replay attack: As is or with message modification.
Man-in-the-middle attack: Intruder sits in data flow, masquerading as sender to receiver and vice versa.
Session hijacking: Intercept an already-established session to bypass authentication.
Privilege escalation: Common attack type with access beyond what a user or resource is supposed to have.

<!-- slide 5 -->
### Security Measure Levels
Impossible to have absolute security, but make cost to perpetrator sufficiently high to deter most intruders.
Security must occur at four levels to be effective:
  Physical: Data centers, servers, connected terminals.
  Application: Malicious apps can cause security problems.
  Operating System: Protection mechanisms, debugging.
  Network: Intercepted communications, interruption, DOS.
Security is as weak as the weakest link in the chain.
Humans a risk too via phishing and social-engineering attacks.

<!-- slide 6 -->
### Program Threats
Many variations, many names.
Trojan Horse:
  Code segment that misuses its environment.
  Exploits mechanisms for allowing programs written by users to be executed by other users.
  Spyware, pop-up browser windows, covert channels.
  Up to 80% of spam delivered by spyware-infected systems.
Trap Door:
  Specific user identifier or password that circumvents normal security procedures.
  Could be included in a compiler.

<!-- slide 7 -->
### Program Threats
Malware - Software designed to exploit, disable, or damage computer.
Trojan Horse – Program that acts in a clandestine manner.
  Spyware – Program frequently installed with legitimate software to display ads, capture user data.
  Ransomware – locks up data via encryption, demanding payment to unlock it
Others include trap doors, logic bombs.
All try to violate the Principle of Least Privilege.
Goal frequently is to leave behind Remote Access Tool (RAT) for repeated access.
[image]

<!-- slide 8 -->
### Program Threats
C Program with Buffer-overflow Condition:
Code review can help – programmers review each other’s code, looking for logic flows, programming flaws.
[image]

<!-- slide 9 -->
### Program Threats
Code-injection attack occurs when system code is not malicious but has bugs allowing executable code to be added or modified.
  Results from poor or insecure programming paradigms, commonly in low level languages like C or C++ which allow for direct memory access through pointers.
  Goal is a buffer overflow in which code is placed in a buffer and execution caused by the attack.
  Can be run by script kiddies – use tools written but exploit identifiers.

<!-- slide 10 -->
### Program Threats
Outcomes from code injection
Frequently use trampoline to code execution to exploit buffer overflow:
[image]
[image]

<!-- slide 11 -->
### Program Threats
Viruses:
  Code fragment embedded in legitimate program.
  Self-replicating, designed to infect other computers.
  Very specific to CPU architecture, operating system, applications.
  Usually borne via email or as a macro.
  Visual Basic Macro to reformat hard drive.
[image]

<!-- slide 12 -->
### Program Threats
Virus dropper inserts virus onto the system.
Many categories of viruses, literally many thousands of viruses:
  File / parasitic
  Boot / memory
  Macro
  Source code
  Polymorphic to avoid having a virus signature
  Encrypted
  Stealth
  Tunneling
  Multipartite
  Armored

<!-- slide 13 -->
### Program Threats
A Boot-sector Computer Virus
[image]

<!-- slide 14 -->
### Program Threats
Worms – use spawn mechanism; standalone program.
Internet worm:
  Exploited UNIX networking features (remote access) and bugs in finger and sendmail programs.
  Exploited trust-relationship mechanism used by rsh to access friendly systems without use of password.
  Grappling hook program uploaded main worm program.
  99 lines of C code .
  Hooked system then uploaded main code, tried to attack connected systems.
  Also tried to break into other users accounts on local system via password guessing.
  If target system already infected, abort, except for every 7th time.

<!-- slide 15 -->
### Four-layered Model of Security
[image]

<!-- slide 16 -->
### Implementing Security Defences
Defence in depth is most common security theory – multiple layers of security.
Security policy describes what is being secured.
Vulnerability assessment compares real state of system / network compared to security policy.
Intrusion detection endeavors to detect attempted or successful intrusions.
  Signature-based detection spots known bad patterns.
  Anomaly detection spots differences from normal behavior.
  Can detect zero-day attacks.
  False-positives and false-negatives a problem.
Virus protection:
  Searching all programs or programs at execution for known virus patterns.
  Or run in sandbox so can’t damage system.
Auditing, accounting, and logging of all or specific system or network activities.
Practice safe computing – avoid sources of infection, download from only “good” sites, etc.

<!-- slide 17 -->
### Firewall to Protect Systems and Networks
A network firewall is placed between trusted and untrusted hosts.
  The firewall limits network access between these two security domains.
Can be tunneled or spoofed.
  Tunneling allows disallowed protocol to travel within allowed protocol (i.e., telnet inside of HTTP).
  Firewall rules typically based on hostname or IP address which can be spoofed.
Personal firewall is software layer on given host.
  Can monitor / limit traffic to and from the host.
Application proxy firewall understands application protocol and can control them (i.e., SMTP).
System-call firewall monitors all important system calls and apply rules to them (i.e., this program can execute that system call).

<!-- slide 18 -->
### Network Security Through Domain Separation Via Firewall
[image]
