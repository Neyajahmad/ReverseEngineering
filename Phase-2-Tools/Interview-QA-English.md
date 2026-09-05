# Phase 2 — Interview Questions & Answers (English)

> The same RE Tools interview questions, answered in clear English for formal interviews.

---

### Q1. What is the difference between a disassembler and a decompiler?
**Ans:** A **disassembler** converts machine code into assembly — it's 100% exact (e.g., IDA, objdump). A **decompiler** converts that code into C-like high-level code — easier to read but approximate, with odd variable names and occasional mistakes (e.g., Ghidra's decompiler, Hex-Rays). Best practice is to use both.

---

### Q2. What is Ghidra and why is it good for beginners?
**Ans:** Ghidra is a free, powerful RE tool developed by the NSA. It provides both a disassembler and a decompiler — showing assembly alongside readable C-like code. Being free and including a decompiler makes it ideal for beginners.

---

### Q3. What is a debugger and how does it differ from a disassembler?
**Ans:** A **disassembler** shows a program's code without running it (static analysis). A **debugger** lets you inspect a program while it **runs** — setting breakpoints, stepping through instructions, and viewing live register and memory values (dynamic analysis). Examples: x64dbg, GDB.

---

### Q4. What is the difference between static and dynamic analysis?
**Ans:** **Static analysis** means understanding a program by reading its code without running it (using Ghidra). **Dynamic analysis** means understanding it while running it and observing runtime values (using x64dbg). They're complementary and often used together.

---

### Q5. What is a cross-reference (XREF) and why is it useful?
**Ans:** An XREF tells you where a function or variable is **used** in the code. For example, if you find a "password" string, its XREFs reveal which function uses it — leading you straight to the important logic. It's the best way to follow clues in RE.

---

### Q6. What is the PE file format?
**Ans:** PE (Portable Executable) is the format for Windows executables (`.exe`, `.dll`). It contains a DOS header, a PE header (32/64-bit info, entry point), and sections such as `.text` (code), `.data` (variables), `.rdata` (strings/constants), `.idata` (imports), and `.rsrc` (resources). The Linux equivalent is the ELF format.

---

### Q7. What is stored in the `.text` and `.rdata` sections?
**Ans:** The `.text` section holds the program's actual code (instructions) — where reversing happens. The `.rdata` section holds read-only data such as strings and constants — often revealing clues like passwords, URLs, and error messages.

---

### Q8. What does the "Strings" tool do and why is it useful in RE?
**Ans:** The Strings tool extracts readable text (printable characters) from a file. In RE it's often the first step — hardcoded passwords, URLs, API keys, error messages, or hints frequently appear directly in the strings, without deep analysis.

---

### Q9. What is a packed binary and how do you detect it?
**Ans:** A packed binary has its code compressed or encrypted (e.g., by UPX) to make analysis harder. Such files show few readable strings and unclear code. You detect them with a tool like **Detect It Easy (DIE)**, which reports whether a file is packed and by which packer.

---

### Q10. Why is renaming important in Ghidra?
**Ans:** The decompiler gives variables generic names like `local_8`, `uVar1`. As you understand what each does, you rename them to meaningful names (like `user_input`, `password_check`). This makes the code far easier to read and reason about — it's like drawing a map as you explore.

---

### Q11. What do the basic x64dbg controls (F7, F8, F9) do?
**Ans:**
- **F2** — set/remove a breakpoint (the program pauses here).
- **F7** — Step Into (enter the called function).
- **F8** — Step Over (execute the function without entering it).
- **F9** — Run (continue to the next breakpoint).

---

### Q12. Which tools are best to start RE with and why?
**Ans:** **Ghidra + x64dbg** — both are free and together handle about 90% of the work. Ghidra for static analysis (reading code, decompiler) and x64dbg for dynamic analysis (runtime debugging). Later you can try advanced tools like IDA or Binary Ninja.
