# Phase 0 — Interview Questions & Answers (English)

> The same important theory questions from Phase 0 (Foundation), answered in clear English. Use this to phrase your exact answer in a formal interview.

---

### Q1. What is Reverse Engineering?
**Ans:** Reverse Engineering is the process of understanding how something works by taking it apart — without having its source code. In software, we take a compiled program (like an `.exe`) and analyze it to figure out its internal logic. It's like opening a closed watch to understand how the gears drive it, but applied to software.

---

### Q2. Why does a computer only understand binary (0s and 1s)?
**Ans:** A computer's electronic circuits have only two stable states — current on (**1**) or off (**0**). Because hardware can reliably represent only these two states, everything is stored and processed in binary.

---

### Q3. Why do we use hexadecimal instead of binary?
**Ans:** Binary is long and hard to read (e.g., `11111111`). Hexadecimal is compact and readable — the same value becomes `0xFF`. Each hex digit maps exactly to 4 binary bits, so conversion is straightforward. RE tools display memory and values in hex.

---

### Q4. How many values can a single byte hold?
**Ans:** One byte = 8 bits. With 8 bits you get 2^8 = **256 distinct values**, i.e. **0 to 255** (hex `0x00` to `0xFF`).

---

### Q5. What is a memory address?
**Ans:** RAM is like a large building where every "flat" has a unique number — that number is the memory address. The computer stores every piece of data (variables, code) at some address so it can locate it later. In RE we frequently inspect what value sits at a given address.

---

### Q6. What is endianness? Difference between little and big endian?
**Ans:** Endianness defines the byte order in which a multi-byte number is stored in memory.
- **Little Endian:** the least significant byte is stored first. Used by Intel/AMD (x86/x64).
- **Big Endian:** the most significant byte is stored first. Common in network protocols.

Example: `0x12345678` is stored as `78 56 34 12` in little endian, and `12 34 56 78` in big endian.

---

### Q7. What is a pointer? Why is it important in RE?
**Ans:** A pointer is a variable that holds the **address** of another variable rather than its value. `&x` gives the address of `x`, and `*p` gives the value stored at that address. Pointers are crucial in RE because at the assembly level most data access happens through addresses.

---

### Q8. What is ASCII? What is the value of 'A'?
**Ans:** ASCII is a standard that assigns a number to each character so computers can store text. For example, `'A'` = 65 (hex `0x41`), `'a'` = 97 (`0x61`). In RE, bytes like `41 42 43` in memory actually represent the string `"ABC"`.

---

### Q9. What is the compilation process — how does source code become an .exe?
**Ans:** The steps are:
1. **Preprocessor** — handles directives like `#include`.
2. **Compiler** — translates C code into assembly.
3. **Assembler** — converts assembly into machine code (object files).
4. **Linker** — links libraries and produces the final `.exe`.

Reverse Engineering is the reverse of this — going from `.exe` back to understanding the logic.

---

### Q10. What information is lost during compilation?
**Ans:** Variable names (like `age`, `price`), original function names, comments, and clear type information are lost. Only raw instructions remain in the machine code. This is why RE is hard — we must reconstruct that lost information by analysis.

---

### Q11. What is the difference between machine code and assembly?
**Ans:** **Machine code** is pure numbers (bytes) that the CPU executes directly. **Assembly** is a human-readable representation of that machine code, e.g., `mov eax, 5`. Each assembly instruction maps to machine code, but assembly is far easier for humans to read.

---

### Q12. What is the basic difference between the stack and the heap?
**Ans:** Both are regions of memory a program uses.
- **Stack:** automatic and fast; stores local variables and function call information in LIFO (Last In, First Out) order.
- **Heap:** dynamic memory requested at runtime (e.g., `malloc` in C). More flexible but must be managed manually.

---

### Q13. What is the difference between a bit and a byte?
**Ans:** A **bit** is the smallest unit — a single `0` or `1`. A **byte** is a group of 8 bits and can represent values from 0 to 255. Data sizes are usually measured in bytes (KB, MB, GB).

---

### Q14. Why is knowing C important for learning RE?
**Ans:** Most software is written in C/C++, and decompiler output (e.g., from Ghidra) resembles C. If you understand C — especially pointers, arrays, and functions — reading decompiled code becomes much easier.
