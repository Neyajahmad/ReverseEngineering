# Phase 3 — Interview Questions & Answers (English)

> The same Static Analysis interview questions, answered in clear English for formal interviews.

---

### Q1. What is static analysis?
**Ans:** Static analysis means analyzing a program **without running it** — understanding what it will do by reading its code (assembly or decompiled C). It's safe (even malware never executes) and shows the whole code at once.

---

### Q2. What are the advantages and disadvantages of static analysis?
**Ans:**
- **Advantages:** It's safe (nothing executes), and you see the entire code and all code paths (even hidden ones).
- **Disadvantages:** Encrypted/packed code is hard to read, and runtime-generated values (like a decrypted password) aren't visible.

That's why it's combined with dynamic analysis.

---

### Q3. What is the first step when starting to analyze a program?
**Ans:** File **recon** — identify the file type (PE/ELF, 32/64-bit), whether it's packed (using a tool like DIE), and which compiler/language produced it. Then extract **strings**, which often directly reveal clues like passwords, messages, and URLs.

---

### Q4. What do you get from strings analysis?
**Ans:** Readable text hidden in the file — hardcoded passwords/keys, URLs or IP addresses (malware C2 servers), success/error messages ("Access granted!", "Wrong password!"), file paths, and registry keys. It's the first and easiest source of clues.

---

### Q5. What is the difference between the entry point and main()?
**Ans:** The **entry point** is where the program technically begins — it contains runtime setup code. **main()** is the function containing the program's actual logic. In RE we usually start analysis from main().

---

### Q6. How do you recognize control flow in static analysis?
**Ans:** If-else is formed by `cmp` plus a conditional jump (`je`/`jne`), and loops by a backward jump (jumping back to an earlier line). The decompiler shows these as `if`, `for`, `while`. Ghidra's Control Flow Graph (CFG) displays this flow visually as boxes and arrows.

---

### Q7. What should you infer when you see the `strcmp` function?
**Ans:** `strcmp` compares two strings and returns **0** when they're equal. In RE, seeing `strcmp(input, "something") == 0` signals a string comparison — often a password or key check. It leads you straight to the important logic.

---

### Q8. What should you keep in mind while reading decompiler output?
**Ans:** Variable names will be generic (`local_8`, `iVar1`, `param_1`) — rename them as you understand them. Types may be wrong (int vs pointer) — set the correct type. And the decompiler occasionally makes mistakes — when in doubt, check the assembly (Listing view).

---

### Q9. Why are renaming and commenting important in static analysis?
**Ans:** The decompiler assigns generic names that are hard to follow. As you understand the logic, give functions/variables meaningful names and add notes (comments). It's like drawing a map — the more you map, the clearer the whole program becomes.

---

### Q10. What do you do if the password isn't in a plain strcmp but is encoded?
**Ans:** Passwords are often hidden with XOR or simple encoding. You then read the encoding logic (e.g., each byte XOR-ed with `0x10`) and reverse it to recover the real password. Alternatively, in dynamic analysis, set a breakpoint and read the decrypted value directly from memory.

---

### Q11. How do cross-references (XREFs) help in static analysis?
**Ans:** XREFs show where a string or function is used. For example, if you find the "Wrong password!" string, its XREF leads directly to the function performing the password check. This gets you to the important logic quickly without reading all the code.

---

### Q12. Why is a packed binary a problem for static analysis?
**Ans:** A packed binary's real code is compressed/encrypted, so static analysis shows only the packer's stub and garbage — the real logic and strings are hidden. It must first be unpacked (or dumped from memory during dynamic analysis) before the real code can be analyzed.
