# Phase 2 — Tools Se Dosti (Aujaar Seekho)

> **Goal:** Wo tools seekhna jinse hum `.exe` ki "ghadi" kholte hain. Ab tak theory thi, ab practical tools. Time: **2-3 hafte**.

---

## 2.0 — Tools Kyun Zaroori?

Assembly padhna aa gaya (Phase 1), lekin `.exe` ko assembly mein badalta kaun hai? **Tools.** Ye tools raw machine code ko humein padhne layak (assembly / C-jaisa) code mein dikhate hain, program ko chalate-rokte hain, aur memory dikhate hain.

Tools 3 category ke hote hain:
1. **Disassembler / Decompiler** — `.exe` → assembly / C-jaisa code (static)
2. **Debugger** — program ko chalate hue rok-rok ke dekhna (dynamic)
3. **Helper tools** — file inspect karna, strings nikaalna, hex dekhna

---

## 2.1 — Disassembler vs Decompiler (Farak Samjho)

| | Disassembler | Decompiler |
|---|---|---|
| Output | Assembly code | C-jaisa high-level code |
| Padhna | thoda mushkil | aasan |
| Accuracy | 100% sahi | approximate (kabhi galat) |
| Example | IDA, objdump | Ghidra decompiler, Hex-Rays |

- **Disassembler:** machine code → assembly. Ye exact hota hai.
- **Decompiler:** assembly → C-jaisa code. Ye padhna aasan hai, but variable naam ajeeb (`local_8`, `uVar1`) aur kabhi galti bhi karta hai.

> **Best strategy:** Dono use karo. Decompiler se overall logic samjho, aur jahan doubt ho wahan assembly (disassembler) dekho.

---

## 2.2 — Main Tools (Comparison)

| Tool | Kya Hai | Cost | Beginner? |
|------|---------|------|-----------|
| **Ghidra** ⭐ | NSA ka banaya, free, powerful. Disassembler + decompiler dono | Free | ✅ Best se shuru |
| **IDA Free** | Industry standard, free version (64-bit disassembler) | Free | ✅ |
| **x64dbg** | Windows debugger (runtime analysis) | Free | ✅ |
| **Cutter / Rizin** | Open source, modern UI | Free | ✅ |
| **Binary Ninja** | Achha UI, powerful | Paid | ⚠️ baad mein |
| **GDB** | Linux ka classic debugger | Free | Linux ke liye |

> **Recommendation:** **Ghidra + x64dbg** se shuru karo. Dono free hain. Ghidra static analysis ke liye, x64dbg dynamic ke liye. Yahi combo 90% kaam kar dega.

---

## 2.3 — Ghidra (Sabse Zaroori Tool)

Ghidra free hai aur **decompiler** bhi deta hai (assembly ko C-jaisa dikhata hai) — isiliye beginners ke liye best.

### Ghidra Setup
1. Java (JDK 17+) install karo (Ghidra ko chahiye).
2. Ghidra download karo (official NSA GitHub se).
3. Run karo, ek **Project** banao.
4. Apna `.exe` import karo (File → Import File).
5. Analysis chalao ("Analyze" — Yes bolo, defaults theek hain).

### Ghidra ke main windows (parts)
- **Listing** (beech mein) — assembly code, address ke saath.
- **Decompiler** (right side) — C-jaisa code. ⭐ Sabse zyada yahi padhoge.
- **Symbol Tree** (left) — functions, imports, exports ki list.
- **Program Trees / Data Type Manager** — structure aur types.

### Ghidra mein basic kaam
- **Function dhoondo:** Symbol Tree → Functions. `main` ya `entry` dhoondo.
- **Rename karo:** Kisi variable/function pe click → `L` dabao → naya naam do (jaise `local_8` ko `user_input`). Isse code padhna aasan hota hai.
- **Cross-references (XREF):** Kisi function pe right-click → "References" → dekho use kahan-kahan call hua. ⭐ Bahut useful.
- **Comments:** `;` dabao ke apne notes likho.

> **Tip:** Ghidra mein rename aur comment karte raho jaise-jaise samajhte ho. Ye "map banane" jaisa hai — jitna map banega, utna aasan.

---

## 2.4 — x64dbg (Windows Debugger)

x64dbg program ko **chalate hue** rok-rok ke dekhne ke liye hai (dynamic analysis — Phase 4 mein detail).

### x64dbg ke main parts
- **CPU window** — assembly instructions (abhi kaunsi chal rahi).
- **Registers** (right) — live values (eax, ebx, eip...).
- **Dump** (niche) — memory ka hex view.
- **Stack** (right-niche) — stack ki live values.

### Basic controls
| Key | Kaam |
|-----|------|
| `F2` | Breakpoint lagao/hatao (yahan ruk jaao) |
| `F7` | Step Into (function ke andar jao) |
| `F8` | Step Over (function ko chala do, andar mat jao) |
| `F9` | Run (agle breakpoint tak chalao) |

> Abhi in tools ko sirf install karke ghoomo. Deep use Phase 3-4 mein.

---

## 2.5 — Helper Tools (Chhote But Kaam Ke)

Ye chhote tools RE workflow mein bahut kaam aate hain:

| Tool | Kaam |
|------|------|
| **Detect It Easy (DIE)** ⭐ | File kaunse language/compiler se bani, packed hai ya nahi — instant batata hai |
| **PE-bear / PEview / CFF Explorer** | `.exe` (PE file) ki internal structure dekhna — headers, sections, imports |
| **Strings** (Sysinternals) | File mein chhupe readable text nikaalna (URLs, passwords, messages) |
| **HxD (Hex Editor)** | File ko raw bytes mein dekhna/edit karna |
| **Dependency Walker / PE imports** | Program kaunsi DLL/functions use karta hai |
| **Process Monitor / Process Explorer** | Chalte hue program ki activity (files, registry, network) |

### PE File Structure (Windows .exe) — Basic
Windows executables "PE format" mein hote hain. Iske parts:
- **DOS Header** — purana compatibility header (`MZ` se shuru).
- **PE Header** — asli metadata (32/64 bit, entry point address).
- **Sections:**
  - `.text` — actual code (instructions)
  - `.data` — initialized data (variables jinki value pehle se hai)
  - `.rdata` — read-only data (strings, constants)
  - `.idata` — imports (kaunse DLL/functions use ho rahe)
  - `.rsrc` — resources (icons, images)

> `.text` mein code hota hai (yahan reverse karte hain), `.rdata` mein strings (yahan clues milte hain). Linux mein iska equivalent "ELF format" hota hai.

---

## 2.6 — Pehla Practical (Zaroor Karo)

1. Ek simple C program khud likho (jaise password check karne wala).
2. Compile karke `.exe` banao.
3. **DIE** mein kholo — dekho kya batata hai (compiler, bits).
4. **Ghidra** mein import karo, analyze karo.
5. `main` function dhoondo, decompiler mein padho — apna hi likha logic pehchano!
6. **x64dbg** mein kholo, `main` pe breakpoint lagao, `F8` se step karo.

> Apna hi program reverse karna best starting point hai — kyunki tumhe pehle se pata hai andar kya hai, toh tools samajhna aasan.

---

## 🎯 Key Takeaways (Ek Line Mein)

1. **Disassembler** = assembly (exact); **Decompiler** = C-jaisa code (aasan but approximate). Dono use karo.
2. **Ghidra** (free, decompiler ke saath) = best beginner tool for static analysis.
3. **x64dbg** = best free Windows debugger for dynamic analysis.
4. Ghidra mein **rename** aur **XREF** (cross-references) sabse useful features.
5. **DIE** se file identify karo (packed? kaunsa compiler?).
6. Windows `.exe` = **PE format**; code `.text` section mein, strings `.rdata` mein.
7. Debugger keys: `F2` breakpoint, `F7` step into, `F8` step over, `F9` run.

---

> ✅ **Phase 2 Complete Jab:** Tum Ghidra aur x64dbg install karke ek program khol sako, functions dhoond sako, decompiler padh sako, aur basic navigate kar sako. Ab asli analysis shuru (Phase 3)! 🚀
