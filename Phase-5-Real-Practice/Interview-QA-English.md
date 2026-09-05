# Phase 5 — Interview Questions & Answers (English)

> The same Real Practice, Lab Setup, and Malware Analysis interview questions, answered in clear English.

---

### Q1. Why is practice so important for learning RE?
**Ans:** RE is like swimming — no matter how much theory you read, the skill comes only from solving challenges yourself. Practice teaches pattern recognition, fast tool usage, and real problem-solving. That's why regularly solving crackmes/CTFs is essential.

---

### Q2. Why do we use a Virtual Machine (VM) for analysis?
**Ans:** A VM is an isolated computer running inside your PC. Running malware or unknown files in a VM keeps your real system safe — if the malware causes damage, only the VM is affected, not your main machine.

---

### Q3. What is a snapshot and why is it useful?
**Ans:** A snapshot is a "save point" of the VM — its complete state at a moment. After analyzing malware or if something breaks, you can revert to that clean snapshot in one click. This gives you a fresh, clean environment each time without rebuilding the VM.

---

### Q4. Why is network isolation important when analyzing malware?
**Ans:** Malware often talks to its C2 (Command & Control) server over the internet or spreads across the network. Isolating the network prevents the malware from causing external harm or leaking your IP/data. For analysis we use "host-only" or no network.

---

### Q5. What is a crackme?
**Ans:** A crackme is a program made specifically for RE practice — legal and safe. It typically requires recovering a password/serial or bypassing some protection. Sites like crackmes.one sort them by difficulty, and beginners start with the easy ones.

---

### Q6. What is a CTF?
**Ans:** A CTF (Capture The Flag) is a hacking competition where you solve challenges to capture hidden "flags" (secret strings). In the reversing category you reverse binaries to extract the flag. picoCTF is best for beginners.

---

### Q7. What is the difference between static and dynamic malware analysis?
**Ans:** **Static malware analysis** examines malware without running it — looking at strings, imports, and PE structure (safe). **Dynamic malware analysis** runs the malware in a VM to observe its behavior — what files it creates, how it uses the registry/network. Together they give the full picture.

---

### Q8. What are IOCs (Indicators of Compromise)?
**Ans:** IOCs are signs that a system is infected with malware — such as the malware's file hashes, the IPs/domains it contacts, and the registry keys or files it creates. Malware analysis extracts IOCs so defenders can build detection and response.

---

### Q9. What is a C2 (Command and Control) server?
**Ans:** A C2 server is a remote server controlled by the attacker; the malware receives instructions from it or sends stolen data to it. In malware analysis we examine which C2 the malware contacts and how — an important IOC.

---

### Q10. Why is writing writeups important?
**Ans:** A writeup documents how you solved a challenge. It cements the concept (acts as revision), clarifies your thinking, and builds a portfolio useful for jobs and community. Reading others' writeups also teaches new tricks.

---

### Q11. Why must fundamentals be solid before doing malware analysis?
**Ans:** Malware is often obfuscated, packed, and full of anti-analysis tricks. Without a solid grasp of assembly, static/dynamic analysis, and tools, analysis is hard — and you might accidentally execute malware and endanger your system. So we build fundamentals first via crackmes/CTFs.

---

### Q12. Why is consistency more important than intensity in RE?
**Ans:** RE is a skill built gradually through regular practice. Doing 10 hours one day then nothing for a month helps less than practicing a little every day. Regular practice ingrains patterns in your mind and makes the skill permanent.
