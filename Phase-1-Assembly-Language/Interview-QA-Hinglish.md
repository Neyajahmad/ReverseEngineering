# Phase 1 — Interview Questions & Answers (Hinglish)

> Assembly Language se related important interview questions, simple Hinglish jawaab ke saath.

---

### Q1. Assembly language kya hai?
**Ans:** Assembly, machine code ka human-readable version hai. CPU ko diye jaane wale chhote-chhote commands (instructions) jaise `mov eax, 5`. Har assembly instruction seedha ek machine code se map hoti hai. RE mein jab hum `.exe` kholte hain, tools use assembly mein dikhate hain.

---

### Q2. Register kya hote hain?
**Ans:** Registers CPU ke andar bahut chhote aur bahut fast storage boxes hain jahan actual calculation hoti hai. CPU pehle data ko RAM se register mein laata hai, phir uspe kaam karta hai. Examples: `eax`, `ebx`, `ecx`, `esp`, `eip`.

---

### Q3. EAX, ESP, aur EIP registers ka kya kaam hai?
**Ans:**
- **EAX** — general calculations aur function ka return value yahan aata hai.
- **ESP** (Stack Pointer) — stack ke top (upar) ko point karta hai.
- **EIP** (Instruction Pointer) — batata hai CPU abhi kaunsi instruction chala raha hai. Ye bahut important hai — exploit mein isko control karna hi program hijack karna hota hai.

---

### Q4. Intel aur AT&T syntax mein kya farak hai?
**Ans:** Dono assembly likhne ke styles hain.
- **Intel:** `mov eax, 5` — destination pehle, source baad mein. Windows/IDA/Ghidra mein common.
- **AT&T:** `mov $5, %eax` — source pehle, `%` aur `$` symbols ke saath. Linux/GDB mein default.

RE mein Intel syntax zyada use hota hai.

---

### Q5. `mov eax, [0x1000]` aur `mov eax, 0x1000` mein kya farak hai?
**Ans:** Brackets `[ ]` ka matlab "memory access" hota hai.
- `mov eax, 0x1000` — eax mein seedha value 0x1000 daal do.
- `mov eax, [0x1000]` — address 0x1000 pe jo value rakhi hai, wo eax mein laao (pointer ki tarah).

---

### Q6. `cmp` aur conditional jumps if-else kaise banate hain?
**Ans:** `cmp` do values ko compare karta hai aur result "flags" mein rakhta hai. Uske baad conditional jump (`je` = jump if equal, `jne` = jump if not equal, `jg` = greater, `jl` = less) decide karta hai kahan jaana hai. Ye combination hi assembly mein if-else aur loops banata hai.

---

### Q7. Stack kya hai? Wo kis direction mein badhta hai?
**Ans:** Stack memory ka ek hissa hai jo LIFO (Last In First Out) tarike se kaam karta hai — jaise platon ka dher. Ye function ke local variables, arguments, return address, aur saved base pointer rakhta hai. Important baat: stack **niche ki taraf** (higher se lower address) badhta hai. `push` karne pe ESP chhota hota hai, `pop` pe bada.

---

### Q8. `call` aur `ret` instructions internally kya karte hain?
**Ans:** `call func` pehle wapas aane ka address (return address) stack pe **push** karta hai, phir `func` pe jump karta hai. `ret` stack se wahi return address **pop** karke wapas us jagah jump kar jaata hai. Isliye function khatam hone pe program sahi jagah lautta hai.

---

### Q9. Stack frame kya hota hai?
**Ans:** Har function call ka apna ek memory area stack pe hota hai — usi ko stack frame kehte hain. Ismein us function ke arguments, return address, saved old EBP, aur local variables hote hain. EBP (base pointer) frame ke base ko point karta hai, aur local variables usually `[ebp-4]`, `[ebp-8]` jaise access hote hain.

---

### Q10. x86 aur x64 mein kya farak hai?
**Ans:**
- **x86 (32-bit):** registers `eax, ebx...`, addresses 4 bytes, function arguments zyada tar stack pe pass hote hain.
- **x64 (64-bit):** registers `rax, rbx...` plus extra `r8`-`r15`, addresses 8 bytes, pehle kuch arguments registers mein pass hote hain (Windows: `rcx, rdx, r8, r9`).

Pehle x86 seekhna aasan hota hai, phir x64.

---

### Q11. Calling convention kya hoti hai?
**Ans:** Calling convention wo rules hain jo batate hain ki function ko arguments kaise diye jaayenge aur return value kahan milegi. Example: x86 cdecl mein arguments stack pe push hote hain aur return value `eax` mein aati hai; x64 Windows mein pehle 4 args `rcx, rdx, r8, r9` registers mein jaate hain.

---

### Q12. `xor eax, eax` kya karta hai aur kyun common hai?
**Ans:** Kisi register ko khud ke saath XOR karne pe wo **0** ho jaata hai. `xor eax, eax` matlab `eax = 0`. Compiler isko `mov eax, 0` ki jagah use karta hai kyunki ye thoda fast aur chhota hota hai. RE mein ye pattern bahut dikhega — ise "eax ko zero karna" samajhna.

---

### Q13. `lea` instruction kya karti hai?
**Ans:** `lea` (Load Effective Address) kisi address ko calculate karke register mein daal deti hai — value nahi, sirf address. Jaise `lea eax, [ebx+4]` eax mein `ebx+4` ka address rakhega. Compiler ise pointer math aur kabhi-kabhi fast arithmetic ke liye bhi use karta hai.

---

### Q14. Function ka return value kahan milta hai?
**Ans:** Convention ke hisaab se function ka return value hamesha accumulator register mein aata hai — x86 mein `eax`, x64 mein `rax`. RE mein jab `call` ke baad `eax` ki value use ho rahi ho, wo function ka result hai.

---

### Q15. Register ke chhote hisse (jaise AL, AX) kya hote hain?
**Ans:** Ek bade register ke andar chhote versions hote hain. Jaise `RAX` (64-bit) ke andar `EAX` (32-bit), uske andar `AX` (16-bit), aur uske andar `AH`/`AL` (8-bit) hote hain. `AL` sabse niche ka 1 byte hai — chhoti values (jaise ek character) ke liye use hota hai.
