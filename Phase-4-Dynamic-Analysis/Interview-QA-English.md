# Phase 4 — Interview Questions & Answers (English)

> The same Dynamic Analysis interview questions, answered in clear English for formal interviews.

---

### Q1. What is dynamic analysis?
**Ans:** Dynamic analysis means understanding a program by **actually running it**, pausing it, and observing live values. If static analysis is "reading a book," dynamic is "watching a movie" — you see things happen, such as the actual values in registers and memory.

---

### Q2. When does dynamic analysis become necessary?
**Ans:** When the code is encrypted or packed (unclear statically), when a password/key is decrypted or generated at runtime, or when the logic is so complex that running it is easier than reading it. In these cases static analysis alone isn't enough.

---

### Q3. What is a breakpoint?
**Ans:** A breakpoint is a marker where the program automatically pauses, so we can inspect registers, memory, and the stack at that point. It's the debugger's most important tool. Types include software, conditional (pause only when a condition is true), and hardware (pause on memory read/write).

---

### Q4. What is the difference between Step Into, Step Over, and Step Out?
**Ans:**
- **Step Into (F7):** if the current line is a function call, enter that function.
- **Step Over (F8):** execute the function fully without entering it.
- **Step Out (Ctrl+F9):** run until you exit the current function (back to its caller).

---

### Q5. What is the biggest advantage of dynamic analysis?
**Ans:** You see the **actual values** at runtime. For example, if a password is compared in encrypted form, static analysis shows only encrypted bytes; but by placing a breakpoint at the comparison function, you can read the decrypted real value directly from memory — without understanding the encryption.

---

### Q6. Where should you place a breakpoint to recover a password?
**Ans:** At comparison functions — `strcmp`, `strncmp`, `memcmp`. There the program compares your input against the expected value, and at that moment both values (your input and the real password) are visible in memory/registers. Right after input functions (`scanf`/`fgets`) is also useful.

---

### Q7. What is patching?
**Ans:** Patching means changing a program's instructions/bytes to alter its behavior. A classic example: changing a password check's `jne` (jump if not equal) to `je` or `nop`, so that even a wrong password leads to "access granted."

---

### Q8. What does the NOP instruction do and how is it used in patching?
**Ans:** `nop` (No Operation, byte `0x90`) means "do nothing, move to the next instruction." In patching we replace an unwanted instruction (like a check or a jump) with `nop`, effectively removing it so the program continues past it.

---

### Q9. How do you use static and dynamic analysis together?
**Ans:** A real workflow: first use static (Ghidra) to find the overall structure and interesting functions, then set breakpoints on those functions in dynamic (x64dbg) to observe live values/decrypted data, then re-read the static code with new understanding. This loop repeats — one gives clues, the other confirms them.

---

### Q10. What is anti-debugging?
**Ans:** These are techniques by which a program detects that a debugger is attached and then behaves differently or exits — to make analysis harder. Common tricks: the `IsDebuggerPresent()` API, timing checks (code runs slower under a debugger), and inspecting PEB flags.

---

### Q11. How do you bypass anti-debugging (basics)?
**Ans:** Patch the return value of a check like `IsDebuggerPresent` to 0 (no debugger), or `nop` out the anti-debug check. Plugins for Ghidra/x64dbg (like ScyllaHide) automatically hide many anti-debug tricks.

---

### Q12. What precautions are essential when dynamically analyzing malware?
**Ans:** Always run malware in an isolated **Virtual Machine (VM)**, never on your main PC. Keep the VM's network isolated and use snapshots, so you can revert to a clean state if something goes wrong. This keeps your real system and data safe.
