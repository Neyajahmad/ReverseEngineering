# Phase 1 — Interview Questions & Answers (English)

> The same Assembly Language interview questions, answered in clear English for formal interviews.

---

### Q1. What is assembly language?
**Ans:** Assembly is the human-readable form of machine code — small instructions given to the CPU, such as `mov eax, 5`. Each assembly instruction maps directly to machine code. When we open an `.exe` in RE tools, they present it as assembly.

---

### Q2. What are registers?
**Ans:** Registers are very small, very fast storage locations inside the CPU where actual computation happens. The CPU loads data from RAM into a register, operates on it, and stores the result. Examples: `eax`, `ebx`, `ecx`, `esp`, `eip`.

---

### Q3. What is the role of the EAX, ESP, and EIP registers?
**Ans:**
- **EAX** — used for general calculations and holds a function's return value.
- **ESP** (Stack Pointer) — points to the top of the stack.
- **EIP** (Instruction Pointer) — indicates the instruction the CPU is currently executing. It's crucial: controlling EIP is what lets an attacker hijack program flow.

---

### Q4. What is the difference between Intel and AT&T syntax?
**Ans:** They are two styles of writing assembly.
- **Intel:** `mov eax, 5` — destination first, then source. Common on Windows/IDA/Ghidra.
- **AT&T:** `mov $5, %eax` — source first, with `%` and `$` symbols. Default on Linux/GDB.

Intel syntax is more common in RE.

---

### Q5. What is the difference between `mov eax, [0x1000]` and `mov eax, 0x1000`?
**Ans:** Brackets `[ ]` mean memory access.
- `mov eax, 0x1000` — put the literal value 0x1000 into eax.
- `mov eax, [0x1000]` — load the value stored at memory address 0x1000 into eax (like dereferencing a pointer).

---

### Q6. How do `cmp` and conditional jumps form if-else logic?
**Ans:** `cmp` compares two values and sets CPU flags based on the result. A following conditional jump (`je` = jump if equal, `jne` = jump if not equal, `jg` = greater, `jl` = less) then decides where to go. This combination implements if-else statements and loops in assembly.

---

### Q7. What is the stack and in which direction does it grow?
**Ans:** The stack is a region of memory that works in LIFO (Last In, First Out) order, like a stack of plates. It stores a function's local variables, arguments, return address, and saved base pointer. Importantly, the stack grows **downward** (from higher to lower addresses): `push` decreases ESP, `pop` increases it.

---

### Q8. What do the `call` and `ret` instructions do internally?
**Ans:** `call func` first **pushes** the return address onto the stack, then jumps to `func`. `ret` **pops** that return address off the stack and jumps back to it, so execution resumes right after the original call.

---

### Q9. What is a stack frame?
**Ans:** Each function call has its own region on the stack called a stack frame. It contains that function's arguments, the return address, the saved old EBP, and its local variables. EBP (base pointer) points to the frame's base, and locals are typically accessed as `[ebp-4]`, `[ebp-8]`, etc.

---

### Q10. What is the difference between x86 and x64?
**Ans:**
- **x86 (32-bit):** registers `eax, ebx...`, 4-byte addresses, and function arguments mostly passed on the stack.
- **x64 (64-bit):** registers `rax, rbx...` plus extra `r8`-`r15`, 8-byte addresses, and the first few arguments passed in registers (Windows: `rcx, rdx, r8, r9`).

x86 is easier to learn first, then move to x64.

---

### Q11. What is a calling convention?
**Ans:** A calling convention is the set of rules defining how arguments are passed to a function and where the return value is placed. For example, x86 cdecl pushes arguments on the stack and returns the value in `eax`; x64 Windows passes the first four arguments in `rcx, rdx, r8, r9`.

---

### Q12. What does `xor eax, eax` do and why is it common?
**Ans:** XOR-ing a register with itself sets it to **0**, so `xor eax, eax` means `eax = 0`. Compilers use it instead of `mov eax, 0` because it's slightly faster and smaller in machine code. It's a very common pattern in RE — recognize it as "zeroing the register."

---

### Q13. What does the `lea` instruction do?
**Ans:** `lea` (Load Effective Address) computes an address and stores it in a register — the address, not the value at it. For example, `lea eax, [ebx+4]` puts the address `ebx+4` into eax. Compilers use it for pointer arithmetic and sometimes for fast arithmetic.

---

### Q14. Where is a function's return value stored?
**Ans:** By convention, the return value is placed in the accumulator register — `eax` on x86, `rax` on x64. In RE, if `eax` is used right after a `call`, that value is the function's result.

---

### Q15. What are the smaller sub-registers like AL and AX?
**Ans:** A large register contains smaller sub-registers. `RAX` (64-bit) contains `EAX` (32-bit), which contains `AX` (16-bit), which contains `AH`/`AL` (8-bit). `AL` is the lowest byte — used for small values like a single character.
