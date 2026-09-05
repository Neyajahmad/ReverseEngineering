# 🔍 Reverse Engineering Seekhne Ka Full Plan (Hinglish)

> Ye ek complete roadmap hai — bilkul zero se lekar advanced tak. Har cheez simple language mein, examples ke saath. Jaldbaazi nahi karni, ek-ek step samajhte hue aage badhna hai.

---

## 📌 Sabse Pehle: Reverse Engineering Hai Kya?

**Reverse Engineering (RE)** ka matlab hai kisi cheez ko "ulta" samajhna. Matlab jab humein pata nahi hai ki koi software/program **andar se kaise kaam karta hai**, tab hum use kholkar, tod-tod ke samajhte hain.

**Real life example:**
Socho tumhare paas ek band ghadi (watch) hai. Tumhe nahi pata ye kaise chalti hai. Toh tum use kholte ho, gears dekhte ho, samajhte ho ki kaunsa part kya karta hai — **yahi reverse engineering hai**, bas software pe.

Software RE mein hum ek `.exe` file (jiska source code nahi hai) ko lekar samajhte hain ki wo andar kya kar rahi hai.

**RE kyun seekhein? (Kaam kahan aata hai)**
- 🛡️ **Malware Analysis** — Virus kaise kaam karta hai samajhna (cyber security)
- 🐛 **Bug/Vulnerability Hunting** — Software ki kamzoriyan dhoondhna
- 🎮 **Game Hacking / Modding** — (seekhne ke liye, legal boundaries mein)
- 🔓 **CTF Competitions** — Hacking ki competitions jeetna
- 🔧 **Legacy Software Fix** — Purana software jiska code kho gaya, use theek karna
- 💼 **Career** — Security researcher, malware analyst ki achhi demand hai

> ⚠️ **Legal Note:** RE tabhi karna jab tumhare paas permission ho, ya khud ka software ho, ya CTF/practice ke liye bane hue programs ho. Kisi ki cheez bina permission ke todna galat hai.

---

## 🗺️ Learning Roadmap — Overview

Ye plan **6 phases** mein divided hai. Har phase pichle ke upar bana hai:

```
Phase 0  →  Foundation (Computer andar se kaise chalta hai)
Phase 1  →  Assembly Language (Machine ki bhasha)
Phase 2  →  Tools se Dosti (Ghadi kholne wale aujaar)
Phase 3  →  Static Analysis (Bina chalaaye samajhna)
Phase 4  →  Dynamic Analysis (Chalate hue samajhna)
Phase 5  →  Real Practice (CTF, Crackmes, Malware)
Phase 6  →  Specialization (Apna raasta chunna)
Phase 7  →  Mobile RE (Android APK/Smali + iOS IPA)  📱
```

Har phase ke saath **estimated time** diya hai — but ye flexible hai, apni speed se chalo.

---

## 🧱 PHASE 0 — Foundation (Neev Majboot Karo)
**⏱️ Time: 2-3 hafte**

RE seekhne se pehle computer ki basic working samajhni zaroori hai. Warna aage sab upar se nikal jaayega.

### 0.1 — Computer Number Systems
- **Binary (0,1)** — Computer sirf 0 aur 1 samajhta hai
- **Hexadecimal (Hex)** — RE mein sab kuch hex mein dikhta hai (`0x41` = 65 = 'A')
- **Decimal ↔ Binary ↔ Hex** conversion practice

**Example:**
```
Decimal 255  =  Binary 11111111  =  Hex 0xFF
Letter 'A'   =  Decimal 65       =  Hex 0x41
```
> RE mein tumhe baar-baar hex dikhega, isliye ye comfortable hona chahiye.

### 0.2 — Memory (RAM) Kaise Kaam Karti Hai
- **Bits aur Bytes** — 8 bits = 1 byte
- **Memory Addresses** — Har cheez RAM mein ek "address" pe rehti hai (jaise ghar ka address)
- **Stack aur Heap** — Program data ko kahan rakhta hai
- **Endianness** (Little vs Big Endian) — Bytes kis order mein store hote hain

**Example (Little Endian):**
```
Number 0x12345678 memory mein aise store hoga:  78 56 34 12
(ulta! — ye confuse karta hai, isliye pehle samajh lo)
```

### 0.3 — C Programming Basics (Zaroori!)
RE mein zyada tar programs C/C++ mein bane hote hain. C samajhna = RE ka aadha kaam.
- Variables, data types, pointers (⭐ pointers bahut important)
- Functions, loops, if-else
- Arrays aur strings
- Ek chhota program likho, compile karo, dekho `.exe` kaise banta hai

**Practice:** Ek `hello.c` likho → compile karo → `.exe` banao. Ye samajho ki source code → machine code kaise banta hai.

### 0.4 — Program Kaise Banta Hai (Compilation)
```
Source Code (.c)  →  Compiler  →  Assembly  →  Machine Code (.exe)
        ↑                                              ↓
   Ye hum likhte hain                    Ye computer chalata hai

RE mein hum ULTA chalte hain: .exe se wapas samajhne ki koshish
```

**✅ Phase 0 Complete Jab:** Tum hex-decimal convert kar sako, ek chhota C program likh-compile kar sako, aur stack/memory ka basic idea ho.

---

## ⚙️ PHASE 1 — Assembly Language (Machine Ki Bhasha)
**⏱️ Time: 3-4 hafte**

Ye **sabse important** phase hai. Jab `.exe` khologe toh Assembly hi dikhegi. Isko samajhna hi asli RE hai.

### 1.1 — Assembly Hai Kya?
Assembly, machine code ka human-readable version hai. Computer ko diye jaane wale chhote-chhote commands (instructions).

**Example:**
```asm
mov eax, 5      ; eax naam ke box mein 5 daalo
add eax, 3      ; eax mein 3 aur jodo (ab eax = 8)
```
> Har line ek chhota kaam karti hai. C ki ek line = kai assembly lines.

### 1.2 — CPU Registers (Chhote Dabbe)
Registers = CPU ke andar chhote-chhote storage boxes jahan calculation hoti hai.
- **General purpose:** `eax`, `ebx`, `ecx`, `edx` (32-bit) / `rax`, `rbx`... (64-bit)
- **Special:** `esp`/`rsp` (stack pointer), `ebp`/`rbp` (base pointer), `eip`/`rip` (instruction pointer — abhi kaunsi line chal rahi)

### 1.3 — Common Instructions (Ratne nahi, samajhne hain)
| Instruction | Kaam |
|-------------|------|
| `mov`  | Data ek jagah se doosri jagah copy |
| `add` / `sub` | Jodna / Ghatana |
| `cmp`  | Do cheezein compare karna |
| `jmp`  | Kisi aur line pe jump |
| `je` / `jne` | Jump if equal / not equal (conditions) |
| `call` | Function bulana |
| `ret`  | Function se wapas aana |
| `push` / `pop` | Stack pe daalna / nikalna |

### 1.4 — x86 vs x64
- **x86** = 32-bit (purana, seekhne mein aasan)
- **x64** = 64-bit (modern, aaj kal sab yahi)
- Pehle x86 se shuru karo, phir x64.

### 1.5 — Stack Ko Deeply Samjho
- Function call hone pe stack pe kya hota hai
- Local variables, function arguments, return address
- **Stack frame** ka concept
> Stack samajh gaye toh RE ka bada hissa clear ho jaata hai.

**Practice:**
- Chhote C programs likho, unhe assembly mein compile karo (`gcc -S`), dono compare karo
- Online: [godbolt.org](https://godbolt.org) — yahan C likho aur turant assembly dekho (BAHUT useful tool)

**✅ Phase 1 Complete Jab:** Tum ek simple assembly code padh ke samajh sako ki wo kya kar raha hai (jaise loop, if-else, addition).

---

## 🛠️ PHASE 2 — Tools Se Dosti (Aujaar Seekho)
**⏱️ Time: 2-3 hafte**

Ab hum wo tools seekhenge jinse ".exe ki ghadi" kholte hain.

### 2.1 — Disassemblers & Decompilers
Ye tools `.exe` ko wapas assembly (ya kuch had tak C jaise) code mein badalte hain.

| Tool | Kya Hai | Cost |
|------|---------|------|
| **Ghidra** ⭐ | NSA ka banaaya, free, powerful. Beginners ke liye best | Free |
| **IDA Free** | Industry standard, free version | Free |
| **x64dbg** | Windows debugging ke liye | Free |
| **Cutter/Rizin** | Open source, modern UI | Free |
| **Binary Ninja** | Achha UI, paid | Paid |

> **Beginners ke liye recommendation: Ghidra se shuru karo.** Free hai aur decompiler bhi deta hai (assembly ko C jaisa dikhata hai).

### 2.2 — Debuggers
Program ko **chalate hue** rok-rok ke dekhne ke liye:
- **x64dbg** (Windows) ⭐
- **GDB** (Linux)
- Breakpoints lagana, step-by-step chalana, registers/memory dekhna

### 2.3 — Helper Tools
- **PE-bear / PEview** — `.exe` file ki structure dekhne ke liye
- **Strings** — File mein chhupe text dhoondhne ke liye
- **HxD (Hex Editor)** — File ko raw bytes mein dekhna/edit karna
- **Detect It Easy (DIE)** — File kis cheez se bani, packed hai ya nahi

**Practice:** Ghidra install karo, ek simple program (jo tumne khud banaya) usme kholo, ghumo, dekho kya-kya dikhta hai. Darr mat, explore karo!

**✅ Phase 2 Complete Jab:** Tum Ghidra/x64dbg mein ek program khol sako, functions dhoond sako, aur basic navigate kar sako.

---

## 📖 PHASE 3 — Static Analysis (Bina Chalaaye Samajhna)
**⏱️ Time: 3-4 hafte**

**Static Analysis** = Program ko chalaye bina, sirf code padh ke samajhna. Jaise book padhna bina act kiye.

### 3.1 — File Ko Pehchano
- File type kya hai? (PE for Windows, ELF for Linux)
- 32-bit ya 64-bit?
- Packed/encrypted toh nahi? (DIE tool se check)
- Strings nikaalo — often passwords, URLs, hints milte hain

### 3.2 — Functions Ko Samjho
- `main` function dhoondo (program yahan se shuru hota hai)
- Function calls ka flow follow karo
- Important functions rename karo (Ghidra mein `analyze_input` jaisa naam do)

### 3.3 — Control Flow Samajhna
- If-else, loops assembly/decompiled code mein kaise dikhte hain
- **Control Flow Graph (CFG)** — Ghidra ye graph banata hai, dekho program kaise flow karta hai

### 3.4 — Decompiler Ka Use
Ghidra ka decompiler assembly ko C-jaise code mein badal deta hai. Ye padhna aasan hai, but 100% sahi nahi hota — variable naam ajeeb hote hain (`local_8`, `iVar1`).

**Example — Ek simple "password checker":**
```c
// Decompiled code aisa dikh sakta hai:
if (strcmp(user_input, "s3cr3t") == 0) {
    printf("Access Granted!");
} else {
    printf("Wrong Password!");
}
```
> Yahan static analysis se hi password `s3cr3t` mil gaya! Program chalaya bhi nahi.

**Practice:** Simple "crackme" challenges (password-based) static analysis se solve karo.

**✅ Phase 3 Complete Jab:** Tum ek simple program ka decompiled code padh ke samajh sako ki wo kya logic use kar raha hai.

---

## 🏃 PHASE 4 — Dynamic Analysis (Chalate Hue Samajhna)
**⏱️ Time: 3-4 hafte**

**Dynamic Analysis** = Program ko **chalate hue** dekhna. Jab static se samajh na aaye (ya code chhupa ho), tab ye kaam aata hai.

### 4.1 — Debugger Ki Power
- **Breakpoint** — Program ko kisi specific line pe rokna
- **Step Into / Step Over / Step Out** — Ek-ek line chalana
- **Registers dekhna** — Runtime pe values kya hain
- **Memory dekhna** — RAM mein kya store ho raha

### 4.2 — Runtime Values Pakadna
Jab program chal raha ho, tum real values dekh sakte ho:
- User input kahan store hua
- Comparison ke waqt kya value thi
- Password memory mein plain dikh sakta hai!

**Example:** Ek program jo password ko encrypt karke compare karta hai — static analysis mushkil. But debugger mein breakpoint lagao `strcmp` pe, aur decrypt hua password memory mein dikhega. 🎯

### 4.3 — Patching (Code Badalna)
- Ek instruction ko badal ke program ka behaviour change karna
- Example: `je` (jump if equal) ko `jne` mein badal ke "wrong password" ko "access granted" bana dena
- Ye samajhna zaroori — malware/protection kaise bypass hoti hai

### 4.4 — Anti-Debugging Se Ladna
- Kuch programs detect kar lete hain ki debugger laga hai aur band ho jaate hain
- Basic anti-debug tricks aur unhe bypass karna (advanced topic, baad mein)

**Practice:** Wahi crackmes ab dynamic analysis se solve karo. Static + dynamic dono ka combo use karo.

**✅ Phase 4 Complete Jab:** Tum x64dbg/GDB mein breakpoint laga sako, values dekh sako, aur ek simple patch kar sako.

---

## 🎯 PHASE 5 — Real Practice (Asli Maidan)
**⏱️ Time: Ongoing — jitna zyada, utna behtar**

Ab theory kaafi ho gayi. RE **practice se** aati hai. Jitne challenges solve karoge, utne acche banoge.

### 5.1 — Crackmes (Beginner Friendly)
Ye specially RE practice ke liye bane programs hain (legal, safe).
- 🌐 **crackmes.one** — Difficulty-wise sorted, easy se shuru karo
- Password find karna, serial key nikalna, etc.

### 5.2 — CTF Challenges (Capture The Flag)
Hacking competitions. "Reversing" category pe focus karo.
- 🌐 **picoCTF** — Beginners ke liye best, free
- 🌐 **CTFtime.org** — Live competitions ki list
- 🌐 **HackTheBox / TryHackMe** — Guided learning

### 5.3 — Malware Analysis (Advanced, Careful!)
- ⚠️ **HAMESHA isolated VM (Virtual Machine) mein** — kabhi apne main PC pe nahi!
- Real virus kaise kaam karta hai samajhna
- 🌐 **MalwareBazaar**, **theZoo** (samples — extreme care ke saath)

### 5.4 — Apna Setup Banao (Lab)
- **Virtual Machine** (VirtualBox/VMware) — Safe environment
- Windows VM + Linux VM
- Snapshots — Kuch bigad jaaye toh wapas theek
- Internet isolated rakhna malware ke liye

**Practice Routine:** Har hafte kam se kam 2-3 crackmes solve karo. Writeup padho (dusron ne kaise solve kiya). Apna writeup likho.

---

## 🚀 PHASE 6 — Specialization (Apna Raasta Chuno)
**⏱️ Time: Long term**

Ab tum RE ki basics jaante ho. Ab decide karo kis direction mein jaana hai:

### Options:
1. **🦠 Malware Analyst** — Virus, ransomware analyze karna. Cyber security jobs.
2. **🐛 Vulnerability Researcher** — Software bugs dhoondna, exploits banana (bug bounty)
3. **📱 Mobile RE** — Android (APK, smali) / iOS apps reverse karna
4. **🎮 Game Hacking** — Memory editing, cheats (learning purpose)
5. **🔐 Cryptography RE** — Encryption algorithms samajhna
6. **⚡ Firmware/IoT RE** — Hardware devices ka software

### Advanced Topics (Jab ready ho):
- Packers & Unpacking (compressed/encrypted binaries)
- Obfuscation techniques aur unhe todna
- Exploit development (buffer overflow, ROP chains)
- Kernel-level RE
- Automation (scripting with Python — Ghidra scripting, angr framework)

---

## 📱 PHASE 7 — Mobile Reverse Engineering (Android + iOS)
**⏱️ Time: 6-8 hafte (dono platforms ke liye)**

> ⚠️ **Zaroori:** Phase 0-6 pehle complete karo. Mobile RE mein wahi concepts (assembly, static/dynamic analysis, debugging) use hote hain, bas platform alag hai. Base strong hoga toh ye aasan lagega.

Mobile apps desktop `.exe` se alag kaam karti hain. Android apps **Java/Kotlin** mein banti hain (ek special "bytecode" mein chalti hain), aur iOS apps **Swift/Objective-C** mein (native ARM machine code). Isliye tools aur approach dono alag hain.

Ye phase **do parts** mein hai:
- **Part A — Android RE** (pehle seekho, aasan aur free)
- **Part B — iOS RE** (baad mein, Mac aur jailbroken device chahiye hota hai)

---

### 🟢 PART A — ANDROID REVERSE ENGINEERING
**⏱️ Time: 4-5 hafte**

Android se shuru karo — free hai, tools easily milte hain, aur ek normal Windows/Linux PC pe ho jaata hai.

#### 7A.1 — Android Basics Samjho (Neev)
Pehle samajho Android app hoti kya hai.
- **APK file kya hai?** — APK = Android Package. Ye asal mein ek **ZIP file** hai! Rename karke `.zip` karo aur khol lo — andar sab files dikh jaayengi.
- **APK ke andar kya hota hai:**
  | File/Folder | Kya Hai |
  |-------------|---------|
  | `AndroidManifest.xml` | App ka "identity card" — permissions, components, entry point |
  | `classes.dex` | ⭐ Asli code (Dalvik bytecode) — yahin logic chhupa hota hai |
  | `resources.arsc` | Compiled resources (strings, layouts) |
  | `res/` | Images, layouts, icons |
  | `lib/` | Native code (`.so` files — C/C++ compiled, ARM assembly) |
  | `assets/` | Extra files (fonts, DBs, config) |
  | `META-INF/` | Signature (app kis ne sign kiya) |

- **App kaise banti hai (samajhna zaroori):**
```
Java/Kotlin Code  →  .class (Java bytecode)  →  .dex (Dalvik bytecode)  →  APK
                                                        ↑
                                          RE mein hum ISKO wapas padhte hain
```
- **DVM vs ART** — Android apps ek Virtual Machine pe chalti hain (pehle Dalvik VM, ab ART - Android Runtime). Isliye code "machine code" nahi, "bytecode" hota hai — jo reverse karna aasan hai desktop se!

#### 7A.2 — Smali Language (Android Ki "Assembly")
Ye Android RE ka **dil** hai. Jaise desktop mein Assembly thi, waise Android mein **Smali** hai.
- **Smali kya hai?** — Dalvik bytecode ka human-readable version. `.dex` ko khologe toh smali dikhega.
- Ye Java se low-level hai but assembly se aasan (registers, methods clearly dikhte hain).

**Example — Ek simple smali:**
```smali
.method public checkPassword(Ljava/lang/String;)Z
    const-string v0, "admin123"        # v0 register mein "admin123" daalo
    invoke-virtual {p1, v0}, ...equals # user input ko "admin123" se compare
    move-result v1                     # result v1 mein
    return v1
.end method
```
> Dekha? Password `admin123` yahin dikh gaya! Smali padhna aa gaya toh aadha kaam ho gaya.

- **Smali seekhne ke points:** registers (`v0, v1, p0, p1`), method syntax, common opcodes (`const-string`, `invoke-virtual`, `if-eq`, `move-result`, `return`)

#### 7A.3 — Android RE Tools
| Tool | Kaam | Level |
|------|------|-------|
| **jadx / jadx-gui** ⭐ | APK ko seedha Java code mein dikhaata hai (best beginner tool) | Free |
| **apktool** ⭐ | APK ko smali mein todta hai aur wapas build karta hai (patching ke liye) | Free |
| **dex2jar** | `.dex` ko `.jar` mein badalta hai | Free |
| **JD-GUI / Bytecode Viewer** | Java code dekhne ke liye | Free |
| **Ghidra / IDA** | `lib/*.so` (native C/C++ code) reverse karne ke liye | Free |
| **Android Studio + Emulator** | App chalane/debug karne ke liye | Free |
| **APKLab (VS Code extension)** | Sab kuch ek jagah, aasan workflow | Free |

> **Beginner recommendation: jadx-gui se shuru karo.** APK drag-drop karo, turant Java-jaisa code dikh jaayega.

#### 7A.4 — Android Static Analysis
Bina app chalaye samajhna:
1. **APK ko jadx-gui mein kholo** → Java code padho
2. **AndroidManifest.xml padho** → Kaunsi Activity pehle chalti hai (entry point), kya permissions maangi
3. **Strings dhoondo** → URLs, API keys, passwords aksar hardcoded milte hain
4. **Interesting functions** → login, password check, encryption wale functions dhoondo
5. **Native `.so` files** → agar logic native code mein hai toh Ghidra mein kholo (yahan Phase 1 ki assembly kaam aayegi!)

**Real example flow:**
```
Ek "Login" app mila
  → jadx-gui mein khola
  → MainActivity dhoondi
  → checkLogin() function mila
  → dekha: if(password.equals("Sup3rS3cr3t"))
  → Password mil gaya bina app chalaye! 🎯
```

#### 7A.5 — Android Dynamic Analysis
App ko **chalate hue** dekhna:
- **Emulator setup** — Android Studio ka emulator (safe, computer pe hi Android chalega)
- **Logcat** — App ke logs dekhna (developers debug info chhod dete hain)
- **Frida** ⭐⭐ — Sabse powerful tool. Chalti hui app mein "hook" laga ke functions ko rok/badal sakte ho, runtime values dekh sakte ho
  - Example: `equals()` function hook karke dekho asli password kya compare ho raha
- **Objection** — Frida ke upar bana, aasan commands (SSL pinning bypass, etc.)
- **Burp Suite / mitmproxy** — App ka network traffic dekhna (kaunse server se baat kar rahi)

#### 7A.6 — APK Patching (Modify & Rebuild)
App ka behaviour badalna:
```
apktool d app.apk        # APK ko smali mein todo (decompile)
   ↓
smali file edit karo     # jaise "if-eq" ko "if-ne" mein badlo
   ↓
apktool b app_folder     # wapas APK banao (build)
   ↓
sign the APK             # naya APK sign karo (warna install nahi hoga)
   ↓
install & test           # emulator/phone pe chalao
```
> Example: Ek "premium locked" feature ko `return true` bana ke unlock karna (learning ke liye).

#### 7A.7 — Android Advanced Topics
- **Obfuscation** — ProGuard/R8/DexGuard se code ke naam ulte-pulte (`a.a.a()`) — inhe samajhna
- **Anti-tampering / Root detection** — Apps jo detect karti hain ki modify/root hua, aur unhe bypass karna
- **SSL Pinning bypass** — Network traffic dekhne ke liye
- **Native code RE** — `.so` files (ARM assembly) — yahan desktop RE skills seedha kaam aati hain
- **Frida scripting** — Apne custom hooks likhna (Python/JavaScript)

**✅ Android Part Complete Jab:** Tum ek APK ko jadx mein khol ke logic samajh sako, smali padh sako, Frida se ek function hook kar sako, aur ek simple patch karke APK rebuild kar sako.

---

### 🍎 PART B — iOS REVERSE ENGINEERING
**⏱️ Time: 3-4 hafte**

> ⚠️ **Note:** iOS RE thoda mushkil aur mehanga hai. Ideally chahiye: ek **Mac** (kuch tools sirf Mac pe) aur ek **jailbroken iPhone** (dynamic analysis ke liye). Bina inke bhi kuch static analysis ho sakta hai, but limited. Isliye Android pehle solid karo.

iOS apps **Swift/Objective-C** mein banti hain aur seedha **ARM machine code** (native) mein compile hoti hain — Android ke bytecode jaisa aasan nahi. Isliye yahan desktop RE skills (assembly, Ghidra) zyada kaam aati hain.

#### 7B.1 — iOS App Basics
- **IPA file kya hai?** — Android ke APK ki tarah, iOS ka app package. Ye bhi ek **ZIP** hai! Andar `.app` folder hota hai.
- **IPA ke andar:**
  | File | Kya Hai |
  |------|---------|
  | `Payload/AppName.app/` | Asli app folder |
  | `AppName` (binary) | ⭐ Main executable — Mach-O format (ARM machine code) |
  | `Info.plist` | App ki config/identity (Android ke Manifest jaisa) |
  | `Frameworks/` | Extra libraries |
  | `_CodeSignature/` | Apple ka digital signature |
  | `.car` / assets | Images, resources |

- **Mach-O format** — iOS/Mac binaries ka format (jaise Windows mein PE, Linux mein ELF)
- **Encryption problem** — App Store apps **encrypted** hoti hain (FairPlay DRM). Reverse karne ke liye pehle **decrypt** karni padti hai (jailbroken device pe, memory se dump karke).

#### 7B.2 — Objective-C & Swift Samjho
- **Objective-C** — Purani but common. Ismein "method calls" ek special tarike se hote hain (`objc_msgSend`) — ye RE mein bahut dikhega
- **Swift** — Modern, but reverse karna thoda mushkil (name mangling, etc.)
- Basic samajh honi chahiye ki inka code kaisa dikhta hai

#### 7B.3 — iOS RE Tools
| Tool | Kaam | Note |
|------|------|------|
| **Ghidra / IDA Pro** ⭐ | Mach-O binary disassemble/decompile | Free/Paid |
| **Hopper Disassembler** | iOS/Mac ke liye popular, achha UI | Paid (trial available) |
| **class-dump** | Objective-C classes/methods ki list nikaalna | Free |
| **Frida** ⭐ | Dynamic analysis, hooking (jailbroken device pe) | Free |
| **Objection** | Frida-based, aasan commands | Free |
| **frida-ios-dump** | Encrypted app ko decrypt/dump karna | Free |
| **Cycript** | Runtime exploration (purana) | Free |
| **otool / nm** | Binary info, symbols dekhna (Mac tools) | Free |

#### 7B.4 — iOS Static Analysis
1. **IPA ko unzip karo** → binary aur `Info.plist` nikaalo
2. **Info.plist padho** → permissions, URL schemes, config
3. **App decrypt karo** (agar App Store se) → frida-ios-dump se
4. **class-dump** → saari classes aur methods ki list (kya-kya functions hain)
5. **Ghidra/Hopper mein binary kholo** → interesting functions (login, crypto) dhoondo, ARM assembly padho
6. **Strings** → hardcoded secrets, URLs

#### 7B.5 — iOS Dynamic Analysis
> Iske liye **jailbroken iPhone** chahiye (ya kuch cases mein emulator/simulator, but limited).
- **Frida hooking** — Chalti hui app ke functions rok/badal ke dekhna
- **Objection** — SSL pinning bypass, method hooking aasan commands se
- **Debugserver / LLDB** — Apple ka debugger, breakpoints lagana
- **Network analysis** — Burp Suite se traffic dekhna
- **Cycript/Frida REPL** — Live app mein objects explore karna

#### 7B.6 — iOS Advanced Topics
- **FairPlay decryption** deeply samajhna
- **Anti-jailbreak detection** bypass (apps jo jailbroken device pe chalne se mana karti hain)
- **SSL pinning bypass** (network dekhne ke liye)
- **Swift-specific RE** (name demangling, metadata)
- **Kernel-level / system RE** (bahut advanced)

**✅ iOS Part Complete Jab:** Tum ek IPA ko unzip/analyze kar sako, class-dump se structure samajh sako, Ghidra/Hopper mein binary explore kar sako, aur (jailbroken device ho toh) Frida se hook laga sako.

---

### 🔑 Android vs iOS — Quick Comparison

| Cheez | Android | iOS |
|-------|---------|-----|
| Package | `.apk` (ZIP) | `.ipa` (ZIP) |
| Code type | Bytecode (Dalvik/DEX) — aasan | Native ARM machine code — mushkil |
| Language | Java / Kotlin | Swift / Objective-C |
| "Assembly" | Smali | ARM Assembly |
| Main tool | jadx, apktool, Frida | Ghidra/Hopper, Frida |
| Encryption | Usually nahi | Haan (FairPlay DRM) |
| Setup cost | Free (koi PC) | Mehnga (Mac + jailbroken iPhone) |
| Difficulty | 🟢 Aasan (shuru yahin se) | 🔴 Mushkil (baad mein) |

> **Rule of thumb:** Android pehle poora seekho (aasan + free), phir iOS. Frida dono mein common hai — isko achhe se seekhna dono jagah kaam aayega.

---

### 📱 Mobile RE — Practice & Resources
- 🌐 **OWASP MASTG (Mobile Security Testing Guide)** — Mobile RE/security ki bible, free
- 🌐 **MASVS** — Mobile security standards
- 🧪 **DIVA / InsecureBankv2 / OWASP UnCrackable** — Practice ke liye jaan-boojh ke kamzor banaayi gayi apps (Android + iOS dono)
- 📱 Apni khud ki banaayi hui simple app reverse karke shuru karo (safe + legal)
- 🎥 YouTube: LiveOverflow (mobile playlists), 8ksec, TheGrayArea

**✅ PHASE 7 Complete Jab:** Tum Android APK aur (resources ho toh) iOS IPA dono ko static + dynamic dono tarike se analyze kar sako, aur ek practice app ka logic/secret nikaal sako.

---

## 📚 Best Resources (Free & Paid)

### Books (Kitabein)
- 📕 **"Practical Malware Analysis"** — Malware RE ki bible
- 📗 **"Reverse Engineering for Beginners"** by Dennis Yurichev — **FREE PDF**, best beginner book
- 📘 **"The IDA Pro Book"** — IDA seekhne ke liye
- 📙 **"Practical Reverse Engineering"** by Bruce Dang

### Online (Free)
- 🎥 **YouTube:** LiveOverflow, stacksmashing, John Hammond, OALabs
- 🌐 **godbolt.org** — C to Assembly instantly
- 🌐 **crackmes.one** — Practice
- 🌐 **picoCTF** — CTF practice
- 📖 **Ghidra official docs & courses**

### Communities
- Reddit: r/ReverseEngineering, r/asm
- Discord servers (CTF, RE communities)

---

## ✅ Quick Checklist (Progress Track Karo)

- [ ] Phase 0: Number systems, memory, basic C samajh liya
- [ ] Phase 1: Assembly padh sakta hoon (registers, common instructions, stack)
- [ ] Phase 2: Ghidra/x64dbg install kiya aur navigate kar sakta hoon
- [ ] Phase 3: Ek program ka decompiled code padh ke samajh liya
- [ ] Phase 4: Debugger mein breakpoint laga ke values dekh li
- [ ] Phase 5: Pehla crackme solve kiya 🎉
- [ ] Phase 5: Pehla CTF reversing challenge solve kiya
- [ ] Phase 6: Apna specialization chun liya
- [ ] Phase 7A: APK ko jadx mein khol ke logic samajh liya
- [ ] Phase 7A: Smali padh liya aur apktool se patch karke rebuild kiya
- [ ] Phase 7A: Frida se ek function hook kiya 🎉
- [ ] Phase 7B: IPA analyze kiya (class-dump + Ghidra/Hopper)
- [ ] Phase 7B: Jailbroken device pe Frida se dynamic analysis kiya

---

## 💡 Important Tips (Yaad Rakho)

1. **Patience rakho** — RE dheere-dheere aati hai. Frustration normal hai.
2. **Practice > Theory** — Sirf padhne se nahi, haath gande karne se aati hai.
3. **Writeups padho** — Dusron ke solutions se bahut seekhoge.
4. **Notes banao** — Har naya instruction/trick note karo.
5. **VM use karo** — Malware ke liye zaroori, warna PC kharaab.
6. **Legal raho** — Sirf permission wale ya practice programs pe.
7. **Community join karo** — Akele mat seekho, log help karte hain.
8. **Chhota shuru karo** — Pehle "Hello World" reverse karo, phir bade programs.

---

> 🎓 **Yaad rakho:** Har expert kabhi beginner tha. Aaj jo "hacker" `.exe` ko chutki mein tod deta hai, wo bhi kabhi `mov eax, 5` pe atka tha. Bas consistent raho, roz thoda seekho. Tum bhi kar loge! 💪

---

*Ye plan ek starting point hai. Jaise-jaise seekhoge, isme apne notes aur discoveries add karte jaana. All the best! 🚀*
