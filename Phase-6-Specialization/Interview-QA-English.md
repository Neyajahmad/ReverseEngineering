# Phase 6 — Interview Questions & Answers (English)

> The same Specialization and advanced-topics interview questions, answered in clear English.

---

### Q1. Why is specialization important in Reverse Engineering?
**Ans:** RE is a very broad field — malware, exploits, mobile, crypto, and firmware are all distinct areas with their own tools and techniques. Becoming expert in everything is impractical, so you pick 1-2 areas and go deep. This builds mastery and gives your career focus.

---

### Q2. What does a Malware Analyst do?
**Ans:** A malware analyst examines malicious software like viruses, ransomware, and trojans — understanding how it works, spreads, and what it steals. They then create detection signatures (e.g., YARA rules) and IOCs to protect systems. The role is in high demand at security companies.

---

### Q3. What do vulnerability researchers and exploit developers do?
**Ans:** They find bugs in software — such as buffer overflows and use-after-free — and build exploits to demonstrate their severity, so companies can fix them. It requires deep assembly, understanding of memory corruption, and exploit development (ROP, shellcode). These skills are used in bug bounties and red teaming.

---

### Q4. What is a packer and what is unpacking?
**Ans:** A packer is a tool that compresses or encrypts a program's code (e.g., UPX) to make analysis harder and reduce size. Unpacking means restoring the original code — usually by running the program until it unpacks itself in memory, then dumping the memory at the OEP (Original Entry Point).

---

### Q5. What is obfuscation?
**Ans:** Obfuscation is deliberately making code complex/confusing to hinder reversing, without changing its behavior. Techniques include inserting junk code, flattening control flow, encrypting strings, and mangling names. Deobfuscation removes that confusion to recover the real logic.

---

### Q6. In exploit development, what are ASLR, DEP, and stack canaries?
**Ans:** These are modern protections that hinder exploits:
- **ASLR** — randomizes memory addresses each run, so attackers can't predict them.
- **DEP/NX** — marks data memory as non-executable, preventing shellcode from running.
- **Stack canary** — places a secret value on the stack; if an overflow changes it, the program detects the tampering.

Exploit developers learn techniques (like ROP) to bypass these.

---

### Q7. Why is scripting/automation important in RE?
**Ans:** Much RE work is repetitive (finding patterns, decrypting, solving inputs). Scripting with Python automates it. Ghidra scripting automates analysis tasks, angr automates input solving (symbolic execution), and Frida automates runtime hooking. This saves time and scales to large programs.

---

### Q8. What is the angr framework?
**Ans:** angr is a Python-based binary analysis framework that performs "symbolic execution" — running a program symbolically to automatically find the input that reaches a specific path (like "access granted"). In RE it's used to automatically solve passwords/keys.

---

### Q9. How is firmware RE different from ordinary software RE?
**Ans:** Firmware is the software of embedded devices (routers, cameras, IoT). It's often on ARM/MIPS architectures (not x86), and you must first extract the firmware from the device (with tools like binwalk). Sometimes you extract code directly from the chip via hardware interfaces (UART, JTAG). The core RE concepts remain the same.

---

### Q10. What are YARA rules?
**Ans:** YARA rules are patterns used to identify/detect malware. In a rule you specify strings, bytes, or conditions characteristic of a malware family. Analysts write these rules so antivirus/scanners can catch similar malware.

---

### Q11. What career options exist in RE?
**Ans:** Malware Analyst, Reverse Engineer, Vulnerability Researcher, Security Researcher, Penetration Tester (with a binary/mobile specialty), and Threat Intelligence Analyst. These roles exist at security companies, antivirus vendors, product security teams, and in bug bounty (freelance).

---

### Q12. What matters most when choosing a specialization?
**Ans:** Job demand and pay matter (Malware Analysis and Vulnerability Research are top), but what matters most is what you genuinely enjoy. RE demands sustained effort, and consistency comes from interest. The best choice combines interest with demand.
