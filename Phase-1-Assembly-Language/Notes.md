# Phase 1 — Assembly Language (Machine Ki Bhasha)

> **Goal:** Assembly padhna aur samajhna seekhna. Jab bhi tum `.exe` khologe, yahi dikhegi. Ye **sabse important phase** hai — isko achhe se karo. Time: **3-4 hafte**. Jaldbaazi bilkul nahi.

---

## 1.0 — Assembly Kyun Itni Important?

Jab ek program compile hota hai, wo machine code ban jaata hai. Ghidra/IDA jaise tools us machine code ko **Assembly** mein dikhate hain. Matlab jo bhi program tum reverse karoge, uski asli bhasha Assembly hi hogi.

> Ek line yaad rakho: **"RE aati hai jab Assembly aati hai."** Baaki sab tools aur tricks bekaar hain agar tum assembly nahi padh sakte.

Ghabrao mat — assembly padhna seekhna hai, likhna nahi (abhi). Aur ye utni mushkil nahi jitni lagti hai, kyunki har instruction bahut chhota kaam karti hai.

---

## 1.1 — Assembly Hai Kya?

Assembly, machine code ka human-readable version hai. CPU ko diye jaane wale chhote-chhote commands (instructions).

**Example:**
```asm
mov eax, 5      ; eax naam ke dabbe mein 5 daalo
add eax, 3      ; eax mein 3 aur jodo (ab eax = 8)
```

- C ki ek line = kai assembly lines. Assembly bahut "low-level" hai — matlab CPU ke bahut kareeb.
- Har line ek chhota specific kaam karti hai: value move karo, jodo, compare karo, jump karo.

### Syntax ke do styles (dhyan rakho)
- **Intel syntax:** `mov eax, 5` (destination pehle) — Windows/IDA/Ghidra mein common. **Hum yahi use karenge.**
- **AT&T syntax:** `mov $5, %eax` (source pehle, `%` aur `$` ke saath) — Linux/GDB default.

> Confusion se bachne ke liye ek hi style pe focus karo. Intel syntax RE mein zyada common hai.

---

## 1.2 — CPU Registers (Chhote Dabbe)

Registers = CPU ke andar bahut chhote, bahut fast storage boxes jahan calculation hoti hai. CPU pehle data ko register mein laata hai, phir kaam karta hai.

### General Purpose Registers (32-bit / 64-bit)

| 32-bit | 64-bit | Aam kaam |
|--------|--------|----------|
| `eax` | `rax` | Calculation, function ka return value yahan aata hai |
| `ebx` | `rbx` | General data |
| `ecx` | `rcx` | Loop counter (counting) |
| `edx` | `rdx` | Data, multiplication/division mein help |
| `esi` | `rsi` | Source (copy karte waqt "kahan se") |
| `edi` | `rdi` | Destination (copy karte waqt "kahan") |

### Special Registers

| Register | Kaam |
|----------|------|
| `esp` / `rsp` | **Stack Pointer** — stack ke top (upar) ko point karta hai |
| `ebp` / `rbp` | **Base Pointer** — current function ke stack frame ka base |
| `eip` / `rip` | **Instruction Pointer** — abhi kaunsi instruction chal rahi hai (⭐ bahut important) |

> **EIP/RIP:** Ye batata hai CPU abhi kahan hai. Agar hum ise control kar lein (exploit mein), toh program hijack ho jaata hai. Isliye ye register RE aur hacking dono mein star hai.

### Register ke chhote hisse

Ek 64-bit register ke andar chhote versions bhi hote hain:
```
RAX (64-bit) ── poora 8 bytes
 └─ EAX (32-bit) ── niche ke 4 bytes
     └─ AX (16-bit) ── niche ke 2 bytes
         └─ AH | AL (8-bit) ── AL sabse niche ka 1 byte
```
> Jab code mein `al` dikhe, wo `eax` ka sabse chhota (1 byte) hissa hai. Ye choti values (jaise ek character) ke liye use hota hai.

---

## 1.3 — Common Instructions (Ratne nahi, samajhne hain)

Ye lists dekh ke ghabrana nahi — practice se sab yaad ho jaayega.

### Data Move
```asm
mov eax, 5        ; eax = 5 (value copy)
mov ebx, eax      ; ebx = eax (ek register se doosre mein)
mov [0x1000], eax ; memory address 0x1000 pe eax ki value likho
mov eax, [0x1000] ; address 0x1000 se value eax mein laao
lea eax, [ebx+4]  ; eax = ebx+4 ka ADDRESS (value nahi) — "load effective address"
```
> `[ ]` brackets ka matlab: "is address pe jaake dekho/likho" (pointer jaisa). Bina bracket = seedha value ya register.

### Arithmetic (ganit)
```asm
add eax, 3        ; eax = eax + 3
sub eax, 1        ; eax = eax - 1
inc eax           ; eax = eax + 1 (increment)
dec eax           ; eax = eax - 1 (decrement)
imul eax, 2       ; eax = eax * 2 (multiply)
xor eax, eax      ; eax = 0 (khud ke saath XOR = 0, ye common trick hai!)
```
> `xor eax, eax` register ko 0 karne ka fast tarika hai. RE mein baar-baar dikhega — ise "eax = 0" samajhna.

### Compare & Jump (⭐ decisions yahan hote hain)
```asm
cmp eax, 10       ; eax aur 10 compare karo (result flags mein)
je  label         ; Jump if Equal (agar barabar the)
jne label         ; Jump if Not Equal
jg  label         ; Jump if Greater
jl  label         ; Jump if Less
jmp label         ; hamesha jump (bina condition)
```

**Ye if-else banate hain!** `cmp` do cheezein compare karta hai, phir `je`/`jne`/etc. decide karte hain kahan jaana hai.

### Function Calls
```asm
call func         ; 'func' function ko bulao (aur wapas aane ka address yaad rakho)
ret               ; function se wapas jao (return)
push eax          ; eax ki value stack pe daalo (upar)
pop  eax          ; stack se upar wali value eax mein nikaalo
```

### Cheat Sheet Table

| Instruction | Kaam | Yaad rakhne ka tarika |
|-------------|------|----------------------|
| `mov`  | copy data | "move" |
| `add`/`sub` | jodna/ghatana | + / - |
| `cmp`  | compare | do cheez check |
| `jmp`  | jump (always) | seedha kudo |
| `je`/`jne` | jump if equal/not | condition pe kudo |
| `call` | function bulao | function call |
| `ret`  | wapas | return |
| `push`/`pop` | stack pe daalo/nikaalo | stack |
| `xor r,r` | register = 0 | zero karne ka trick |
| `lea`  | address nikaalo | pointer math |

---

## 1.4 — Ek Real Example: C ↔ Assembly

Chalo ek chhota C program lete hain aur uski assembly samajhte hain.

### C Code
```c
int main() {
    int a = 5;
    int b = 3;
    int c = a + b;    // c = 8
    return c;
}
```

### Assembly (approx, Intel syntax)
```asm
main:
    push ebp            ; purana base pointer bachao (stack pe)
    mov  ebp, esp       ; naya stack frame set karo
    mov  dword [ebp-4], 5   ; a = 5 (stack pe rakha)
    mov  dword [ebp-8], 3   ; b = 3
    mov  eax, [ebp-4]   ; eax = a (5)
    add  eax, [ebp-8]   ; eax = eax + b = 5+3 = 8
    mov  [ebp-12], eax  ; c = eax = 8
    mov  eax, [ebp-12]  ; return value eax mein daalo (8)
    pop  ebp            ; base pointer wapas
    ret                 ; return
```

**Line by line samjho:**
- `push ebp` / `mov ebp, esp` — har function shuru mein ye karta hai ("prologue"). Purana frame bachata hai, naya banata hai.
- `mov dword [ebp-4], 5` — variable `a` stack pe `ebp-4` address pe hai, usme 5 daala. (`dword` = 4 bytes ka int)
- `mov eax, [ebp-4]` — `a` ki value eax mein laayi.
- `add eax, [ebp-8]` — usme `b` jod diya. Ab eax = 8.
- `mov eax, [ebp-12]` — return karne ke liye value eax mein rakhi (return value hamesha eax/rax mein).
- `pop ebp` / `ret` — frame wapas, function khatam ("epilogue").

> Dekha? C ki 4 lines = assembly ki ~10 lines. Har local variable stack pe `[ebp-kuch]` pe rehta hai. Ye pattern har jagah dikhega — ise pehchano.

### If-else assembly mein kaisa dikhta hai

```c
if (x == 10) { y = 1; }
else         { y = 2; }
```
```asm
    cmp dword [ebp-4], 10   ; x ko 10 se compare
    jne else_part           ; agar equal NAHI, else pe jao
    mov dword [ebp-8], 1    ; y = 1  (if wala part)
    jmp end
else_part:
    mov dword [ebp-8], 2    ; y = 2  (else wala part)
end:
```
> Pattern: `cmp` + conditional jump = if-else. Ye pehchan gaye toh RE bahut aasan.

---

## 1.5 — Stack Ko Deeply Samjho (⭐⭐)

Stack samajhna RE ka bada milestone hai. Stack ek "plate ka dher" jaisa hai — LIFO (Last In, First Out). Jo aakhri mein rakha, wahi pehle nikalta hai.

### Stack kya rakhta hai?
- Function ke **local variables**
- Function ke **arguments** (parameters)
- **Return address** (function khatam hone pe kahan wapas jaana hai)
- Purana **base pointer** (ebp)

### Stack Frame ka structure (ek function ka)

```
Higher address (upar)
  ┌────────────────────┐
  │  function arguments │
  ├────────────────────┤
  │  return address     │  <- call ke waqt push hua
  ├────────────────────┤
  │  old ebp (saved)    │  <- ebp yahan point karta hai
  ├────────────────────┤
  │  local variable a   │  [ebp-4]
  │  local variable b   │  [ebp-8]
  └────────────────────┘
Lower address (esp yahan, stack niche badhta hai)
```

> **Important:** Stack **niche ki taraf** badhta hai (higher se lower address). `push` karne pe esp chhota hota hai, `pop` pe bada. Ye ulta lagta hai but yaad rakho.

### call aur ret internally kya karte hain
- `call func` = pehle wapas aane ka address stack pe **push** karta hai, phir func pe jump.
- `ret` = stack se wo address **pop** karta hai aur wahan wapas jump.

> Isiliye **buffer overflow** attacks kaam karte hain — agar hum stack pe return address ko overwrite kar dein, toh program kahin aur "wapas" jaayega jahan hum chahein. (Ye advanced topic hai, abhi bas idea rakho.)

---

## 1.6 — x86 vs x64 (32-bit vs 64-bit)

| | x86 (32-bit) | x64 (64-bit) |
|---|---|---|
| Registers | `eax, ebx...` | `rax, rbx...` (aur `r8`-`r15` extra) |
| Address size | 4 bytes | 8 bytes |
| Function args | zyada tar stack pe | pehle registers mein (`rcx, rdx, r8, r9` Windows) |
| Seekhna | aasan (pehle karo) | modern (baad mein) |

> **Advice:** Pehle x86 (32-bit) achhe se samajho — simple hai. Phir x64 pe jaao. Concepts same hain, bas thoda badla hua.

---

## 1.7 — Calling Conventions (Functions ko args kaise milte hain)

Jab ek function ko call karte hain, arguments kaise pass hote hain — iske rules ko "calling convention" kehte hain.

- **x86 cdecl:** arguments stack pe push hote hain (ulte order mein). Return value `eax` mein.
- **x64 Windows:** pehle 4 args registers mein — `rcx, rdx, r8, r9`. Baaki stack pe. Return `rax` mein.
- **x64 Linux (System V):** pehle 6 args — `rdi, rsi, rdx, rcx, r8, r9`.

> RE mein jab `call` dikhe, uske pehle wale `mov rcx, ...` / `push ...` dekho — wahi function ke arguments hain. Return value hamesha `eax`/`rax` mein aati hai.

---

## 🧪 Phase 1 — Hands-On Practice

1. **godbolt.org** kholo. Chhote C programs likho, unki assembly dekho. C aur assembly ko side-by-side compare karo.
2. Ye programs try karo: simple addition, if-else, ek loop, ek function call.
3. Har assembly line ko English/Hinglish mein "translate" karo — ye best exercise hai.
4. Register kis line pe kya value rakhega — dimaag mein simulate karo.

---

## 🎯 Key Takeaways (Ek Line Mein)

1. Assembly = machine code ka human-readable version. **RE ki asli bhasha.**
2. **Registers** = CPU ke chhote fast dabbe. `eax`=calculation/return, `esp`=stack top, `eip`=abhi kahan.
3. `[ ]` brackets = memory access (pointer jaisa).
4. `cmp` + conditional jump (`je`/`jne`) = **if-else**. Loops mein bhi yahi.
5. **Stack niche badhta hai**, LIFO hai, aur function ke local vars + return address rakhta hai.
6. `call` return address push karta hai, `ret` use pop karke wapas jaata hai.
7. Return value hamesha `eax`/`rax` mein.
8. Pehle **x86** (aasan), phir **x64**.

---

> ✅ **Phase 1 Complete Jab:** Tum ek simple assembly code padh ke samajh sako ki wo kya kar raha hai — jaise loop, if-else, addition, function call. Ye aa gaya toh RE ka sabse bada hurdle paar. Ab tools seekhne ki baari (Phase 2)! 🚀
