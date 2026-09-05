# Phase 4 — Dynamic Analysis (Chalate Hue Samajhna)

> **Goal:** Program ko **chalate hue** rok-rok ke samajhna. Jab static se samajh na aaye (ya code chhupa ho), tab ye kaam aata hai. Time: **3-4 hafte**.

---

## 4.0 — Dynamic Analysis Kya Hai?

**Dynamic analysis** matlab program ko **actually chalakar**, beech mein rok-rok ke, live values dekhkar samajhna. Static "book padhna" tha, dynamic "movie dekhna" hai — cheezein hoti hui dikhti hain.

**Kab use karein:**
- Code encrypted/packed hai (static mein samajh nahi aata).
- Password runtime pe decrypt/generate hota hai.
- Bahut complex logic — chala ke dekhna aasan hai.

> ⚠️ **Safety:** Malware ya unknown files hamesha **Virtual Machine (VM)** mein chalao, apne main PC pe kabhi nahi. (VM setup Phase 5 mein.)

---

## 4.1 — Debugger Ki Power

Debugger program ko control mein leta hai. Tum decide karte ho kab chale, kab ruke, aur har point pe sab kuch dekh sakte ho.

### Basic Concepts

**Breakpoint** — ek nishaan jahan program apne aap ruk jaayega. Sabse important tool.
- Software breakpoint (`F2` in x64dbg) — kisi instruction pe.
- Conditional breakpoint — sirf jab koi condition sachh ho (jaise `eax == 5`).
- Hardware breakpoint — memory read/write pe ruk jaana.

**Stepping** — ek-ek karke chalana:
| Action | x64dbg | GDB | Kaam |
|--------|--------|-----|------|
| Step Into | `F7` | `stepi`/`s` | function ke andar ghuso |
| Step Over | `F8` | `nexti`/`n` | function ko chala do, andar mat jao |
| Step Out | `Ctrl+F9` | `finish` | current function se bahar niklo |
| Run/Continue | `F9` | `continue`/`c` | agle breakpoint tak chalao |

**Watch/Inspect** — chalte hue dekhna:
- **Registers** — live values (eax, eip...).
- **Memory/Dump** — kisi address pe kya rakha hai (hex + ASCII).
- **Stack** — abhi stack pe kya hai.

---

## 4.2 — Runtime Values Pakadna (Sabse Bada Fayda)

Static mein bas code dikhta hai, dynamic mein **asli values** dikhti hain.

**Example scenario:** Ek program password ko encrypt karke compare karta hai. Static analysis mein tumhe sirf encrypted bytes dikhenge — asli password nahi. Lekin:

1. `strcmp` (ya jahan comparison hota hai) pe **breakpoint** lagao.
2. Program chalao (`F9`), password wahan pe rukega.
3. Registers/stack dekho — `strcmp` ke arguments mein **decrypt hua asli password** memory mein dikhega! 🎯

> Ye dynamic analysis ka superpower hai — encryption ke baad ki asli value seedha memory mein dekh lena, bina encryption samjhe.

### Kahan breakpoint lagayein? (common spots)
- `strcmp` / `strncmp` / `memcmp` — comparisons pe.
- `scanf` / `fgets` ke baad — input padhne ke baad.
- Interesting function ke shuru mein.
- Jahan "success/wrong" message print hone se pehle decision hota hai.

---

## 4.3 — Patching (Code Badalna)

**Patching** matlab program ke bytes/instructions ko badal ke uska behaviour change karna. Ye RE ka bahut powerful (aur maze wala) hissa hai.

### Classic Example: "Wrong" ko "Right" bana dena

Password check aisa hota hai:
```asm
    cmp eax, ebx        ; password sahi hai?
    jne wrong_password  ; agar NAHI (not equal), wrong pe jao
    ; ... access granted ...
wrong_password:
    ; ... wrong message ...
```

**Patch idea:** `jne` (jump if not equal) ko `je` (jump if equal) ya `nop` (kuch mat karo) mein badal do. Ab galat password pe bhi "access granted" chalega!

```asm
    cmp eax, ebx
    je  wrong_password  ; ULTA kar diya (ya nop se hata do)
```

### Patching ke tarike
- **NOP karna** — instruction ko `nop` (No Operation, `0x90`) se replace karna — matlab "kuch mat karo, aage badho".
- **Jump ulta karna** — `je` ↔ `jne`, `jg` ↔ `jle` etc.
- **Return value badalna** — function ko forcefully `return 1` (ya 0) karwana.

### x64dbg mein patch kaise
1. Jis instruction ko badalna hai uspe right-click → "Assemble" (`Space`).
2. Nayi instruction likho (jaise `nop` ya `je`).
3. File → "Patch file" → save karke naya `.exe` banao.

> ⚠️ Patching sirf learning ya authorized testing ke liye. Kisi ki software ki protection todna illegal hai.

---

## 4.4 — Static + Dynamic Combo (Real Workflow)

Asli RE mein dono milaake use hote hain:

```
1. Static (Ghidra)  → overall structure samjho, interesting functions dhoondo
2. Breakpoints       → un functions pe x64dbg mein breakpoint lagao
3. Dynamic (x64dbg)  → chala ke live values, decrypt data dekho
4. Wapas Static      → nayi samajh ke saath code dubara padho
5. Patch (optional)  → behaviour change karke test karo
```

> Ye loop repeat hota rehta hai. Ek se clue milta hai, doosre se confirm hota hai.

---

## 4.5 — Anti-Debugging (Basic Idea)

Kuch programs (khaaskar malware) detect kar lete hain ki debugger laga hai, aur phir alag behave karte hain ya band ho jaate hain — taaki analysis mushkil ho.

**Common anti-debug tricks:**
- `IsDebuggerPresent()` — Windows API jo batata hai debugger hai ya nahi.
- Timing checks — code kitni der mein chala (debugger mein slow hota hai).
- PEB flag check — process ke internal flag dekhna.

**Bypass (basic):**
- `IsDebuggerPresent` ke return value ko patch karके 0 (no debugger) bana do.
- Anti-debug check ko `nop` se hata do.
- Plugins (jaise ScyllaHide) automatically kai anti-debug tricks chhupa dete hain.

> Ye advanced topic hai. Abhi bas idea rakho ki aisa hota hai. Deep dive baad mein.

---

## 4.6 — Dynamic Example: Encrypted Password Nikaalna

**Scenario:** Program password ko reverse + XOR karke compare karta hai. Static mushkil.

**Steps:**
1. Ghidra mein dekha — comparison `memcmp` pe ho raha hai.
2. x64dbg mein program kholo.
3. `memcmp` pe breakpoint (`bp memcmp` ya address pe `F2`).
4. Program chalao, koi bhi password daalo. Breakpoint hit hoga.
5. `memcmp` ke arguments (registers/stack) dekho — ek argument mein tumhara input, doosre mein **asli expected password** (already decrypt hokar).
6. Memory dump mein wo asli password padho. Done! 🎯

> Encryption ki logic samajhne ki zaroorat hi nahi padi — dynamic ne decrypt hui value seedha dikha di.

---

## 🧪 Phase 4 — Hands-On Practice
- Wahi crackmes (Phase 3 wale) ab **dynamic** analysis se solve karo.
- Ek crackme mein **patch** karke "wrong" ko "access granted" bana ke dekho.
- `strcmp`/`memcmp` pe breakpoint laga ke expected value pakdo.
- Static aur dynamic dono ka combo practice karo.

---

## 🎯 Key Takeaways (Ek Line Mein)

1. Dynamic analysis = program **chala ke**, rok-rok ke, live values dekhna.
2. **Breakpoint** sabse important — jahan chaho program ruk jaaye.
3. `F7` step into, `F8` step over, `Ctrl+F9` step out, `F9` run.
4. Comparison functions (`strcmp`/`memcmp`) pe breakpoint = asli password memory mein dikh jaata hai.
5. **Patching** = instructions badalna (`nop`, jump ulta) — behaviour change.
6. Static + dynamic ko **milaake** use karo — ek se clue, doosre se confirm.
7. **Anti-debugging** hoti hai — programs debugger detect karte hain; basic bypass patching/plugins se.
8. Unknown files hamesha **VM** mein.

---

> ✅ **Phase 4 Complete Jab:** Tum x64dbg/GDB mein breakpoint laga sako, live values/memory dekh sako, ek simple patch kar sako, aur ek crackme dynamic analysis se solve kar sako. Ab asli maidan — practice (Phase 5)! 🚀
