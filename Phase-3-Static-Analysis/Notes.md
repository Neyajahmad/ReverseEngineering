# Phase 3 — Static Analysis (Bina Chalaaye Samajhna)

> **Goal:** Program ko **chalaye bina**, sirf code padh ke samajhna. Jaise book padhna bina act kiye. Ye RE ka core skill hai. Time: **3-4 hafte**.

---

## 3.0 — Static Analysis Kya Hai?

**Static analysis** matlab program ko run kiye bina analyze karna — bas uska code (assembly / decompiled C) padhkar samajhna ki wo kya karega.

**Fayde:**
- ✅ Safe hai — malware bhi chalta nahi, isliye PC surakshit.
- ✅ Poora code ek saath dikh jaata hai.
- ✅ Chhupe paths bhi dikhte hain (jo shayad kabhi run na hon).

**Nuksan:**
- ❌ Encrypted/packed code samajhna mushkil.
- ❌ Runtime values (jaise decrypt hua password) nahi dikhte.

> Isiliye static + dynamic dono seekhte hain. Static se "kya karega" samajho, dynamic (Phase 4) se "kya kar raha hai" confirm karo.

---

## 3.1 — Step 1: File Ko Pehchano (Recon)

Analysis se pehle file ke baare mein basic info nikaalo:

1. **File type?** — Windows PE? Linux ELF? 32-bit ya 64-bit? (DIE tool se)
2. **Packed/encrypted?** — Agar packed hai toh pehle unpack karna padega (DIE batata hai).
3. **Compiler/language?** — C, C++, Go, Rust, .NET? (har ka reverse alag).
4. **Strings nikaalo** — sabse pehla clue yahan milta hai.

### Strings — Sabse Powerful Pehla Kadam
```bash
strings program.exe            # sab readable text
strings program.exe | grep -i "pass"   # "password" jaisa dhoondo
```
Aksar milte hain:
- Passwords, secret keys (hardcoded)
- URLs, IP addresses (malware C2 servers)
- Error/success messages ("Wrong password!", "Access granted!")
- File paths, registry keys

> **Trick:** "Success" ya "Wrong" message dhoondo. Uske XREF se seedha password-check function mil jaata hai!

---

## 3.2 — Step 2: Entry Point Aur main() Dhoondo

Program kahan se shuru hota hai?
- **Entry point** — technically program yahan se chalu hota hai (runtime setup code).
- **main()** — asli logic yahan hota hai. Ghidra usually `main` dhoond ke label kar deta hai.

Ghidra mein: Symbol Tree → Functions → `main` (ya `entry`, `WinMain`). Wahan se logic follow karo.

---

## 3.3 — Step 3: Functions Ka Flow Follow Karo

Ek program bahut se functions ka jaal hota hai. Unhe samajhne ka tarika:

1. `main` se shuru karo.
2. Dekho `main` kaunse functions ko `call` karta hai.
3. Har important function ko rename karo (jaise `FUN_00401000` → `check_password`).
4. Chhote helper functions pehle samjho, phir bade.

### Call Graph
Ghidra ek "call graph" bana sakta hai — dikhata hai kaunsa function kise call karta hai. Isse poore program ka nakshaa mil jaata hai.

---

## 3.4 — Step 4: Control Flow Samajhna (If, Loops)

Decompiled/assembly code mein logic ko pehchano:

### If-else pehchano
```c
// Decompiled Ghidra output aisa dikhta hai:
if (iVar1 == 0x2a) {          // agar iVar1 == 42
    puts("Correct!");
} else {
    puts("Wrong!");
}
```
Assembly mein yahi `cmp` + `je`/`jne` hota hai (Phase 1 wala).

### Loops pehchano
```c
for (i = 0; i < 10; i = i + 1) {
    sum = sum + i;
}
```
Assembly mein loop ek "backward jump" se banta hai (upar wali line pe wapas kudna).

### Control Flow Graph (CFG)
Ghidra CFG banata hai — boxes (basic blocks) aur arrows (jumps). Isse dekho code kaise flow karta hai — kaunsi condition kahan le jaati hai.

---

## 3.5 — Step 5: Decompiler Ka Sahi Use

Ghidra ka decompiler assembly ko C-jaisa dikhata hai. Ye padhna aasan hai, but kuch baatein yaad rakho:

- Variable naam ajeeb honge: `local_8`, `iVar1`, `uVar2`, `param_1`. Inhe **rename** karo jaise samajhte jao.
- Types galat ho sakte hain (int vs pointer). Ghidra ko sahi type batao (`Ctrl+L`).
- Kabhi decompiler galti karta hai — doubt ho toh assembly (Listing window) dekho.

### Common function names pehchano
Ye standard library functions dikhein toh turant matlab samjho:
| Function | Matlab |
|----------|--------|
| `strcmp` / `strncmp` | do strings compare (aksar password check!) |
| `strcpy` / `memcpy` | data copy |
| `printf` / `puts` | screen pe print |
| `scanf` / `gets` / `fgets` | user se input lena |
| `malloc` / `free` | memory allocate/release |
| `fopen` / `fread` | file operations |

> `strcmp` dikha? 90% chance wahan koi string comparison ho raha hai — password ya key check.

---

## 3.6 — Real Example: Ek Password Checker Crack Karna

Chalo ek typical "crackme" static analysis se todte hain.

### Program ka behaviour
```
$ ./crackme
Enter password: hello
Wrong password!
```

### Ghidra decompiler output (typical)
```c
int main(void) {
    char input[32];              // user ka input yahan aayega
    printf("Enter password: ");
    scanf("%s", input);          // user se password liya
    if (strcmp(input, "Sup3rS3cr3t") == 0) {   // compare!
        puts("Access granted!");
        return 0;
    }
    puts("Wrong password!");
    return 1;
}
```

**Line by line samjho:**
- `char input[32];` — 32 characters ka box, user input ke liye.
- `scanf("%s", input);` — user se password padha, `input` mein daala.
- `strcmp(input, "Sup3rS3cr3t")` — user ka input "Sup3rS3cr3t" se compare kiya. `== 0` matlab dono barabar (strcmp equal pe 0 deta hai).
- Agar barabar → "Access granted!", warna "Wrong password!".

> **Password mil gaya: `Sup3rS3cr3t`** — bina program chalaye, sirf code padh ke! Ye static analysis ki power hai.

### Thoda mushkil version (obfuscated)
Kabhi password seedha nahi hota — thoda "encode" hota hai:
```c
// har character ko XOR kiya gaya 0x10 se
char secret[] = {0x63, 0x75, 0x64, ...};   // encoded bytes
for (i = 0; i < len; i++) {
    if ((input[i] ^ 0x10) != secret[i]) {   // input ko bhi XOR karke check
        // wrong
    }
}
```
Yahan tumhe khud `secret` bytes ko `0x10` se XOR karke asli password nikaalna padega. (Ya dynamic analysis se dekh lo — Phase 4.)

---

## 3.7 — Static Analysis Workflow (Summary)

```
1. Recon    → DIE se file type, packed check
2. Strings  → clues (passwords, messages, URLs)
3. Entry    → main() dhoondo
4. Flow     → functions follow karo, rename karo
5. Logic    → if/loops/strcmp pehchano
6. Solve    → password/key/logic nikaalo
7. Notes    → comments aur renames karte raho
```

---

## 🧪 Phase 3 — Hands-On Practice
- crackmes.one se **easy** password-based crackmes lo.
- Sirf static analysis (Ghidra) se solve karne ki koshish karo.
- Har function ko rename + comment karo.
- Kam se kam 3-4 crackmes static se solve karo.

---

## 🎯 Key Takeaways (Ek Line Mein)

1. Static analysis = program **run kiye bina** code padh ke samajhna (safe + complete view).
2. **Strings pehla step** — passwords, messages, URLs aksar seedha mil jaate hain.
3. `main()` se shuru karo, functions ko follow aur rename karte jao.
4. `cmp`+jump = if-else; backward jump = loop.
5. `strcmp` dikha = string comparison (aksar password check).
6. Decompiler aasan hai but approximate — doubt pe assembly dekho.
7. Encrypted password ko XOR/decode karke nikaalna pad sakta hai.

---

> ✅ **Phase 3 Complete Jab:** Tum ek simple program ka decompiled code padh ke samajh sako ki wo kya logic use kar raha, aur ek easy crackme static analysis se solve kar sako. Ab dynamic analysis (Phase 4)! 🚀
