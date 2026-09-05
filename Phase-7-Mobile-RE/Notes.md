# Phase 7 — Mobile Reverse Engineering (Android + iOS)

> **Goal:** Mobile apps (Android APK, iOS IPA) ko reverse karna seekhna. Ye Phase 0-6 ke concepts (assembly, static/dynamic, debugging) use karta hai, bas platform alag hai. Time: **6-8 hafte** (dono platforms). Base strong hoga toh aasan lagega.

---

## 7.0 — Mobile RE Kyun Alag Hai?

Desktop `.exe` aur mobile apps mein bada farak hai:
- **Android apps** Java/Kotlin mein banti hain aur ek special "bytecode" (DEX) mein chalti hain — jo reverse karna **aasan** hai (bytecode → aasani se wapas Java jaisa).
- **iOS apps** Swift/Objective-C mein banti hain aur seedha **native ARM machine code** mein — jo desktop RE jaisa **mushkil** hai.

Isliye tools aur approach dono alag hain. Ye phase do parts mein hai:
- **Part A — Android RE** (pehle karo — free, aasan)
- **Part B — iOS RE** (baad mein — Mac + jailbroken device chahiye)

---

# 🟢 PART A — ANDROID REVERSE ENGINEERING
**⏱️ Time: 4-5 hafte**

Android se shuru karo — free hai, tools easily milte hain, normal Windows/Linux PC pe ho jaata hai.

---

## 7A.1 — Android Basics (Neev)

### APK file kya hai?
**APK = Android Package.** Ye asal mein ek **ZIP file** hai! `app.apk` ko `app.zip` rename karke khol lo — andar sab files dikh jaayengi.

### APK ke andar kya hota hai
| File/Folder | Kya Hai |
|-------------|---------|
| `AndroidManifest.xml` | App ka "identity card" — permissions, components, entry point |
| `classes.dex` | ⭐ Asli code (Dalvik bytecode) — yahin logic chhupa hai |
| `resources.arsc` | Compiled resources (strings, layouts) |
| `res/` | Images, layouts, icons |
| `lib/` | Native code (`.so` files — C/C++ compiled, ARM assembly) |
| `assets/` | Extra files (fonts, DBs, config) |
| `META-INF/` | Signature (app kis ne sign kiya) |

### App kaise banti hai (samajhna zaroori)
```
Java/Kotlin Code
   ↓ (compile)
.class (Java bytecode)
   ↓ (dx / d8 tool)
.dex (Dalvik bytecode)   <- RE mein hum ISKO wapas padhte hain
   ↓ (package + sign)
APK
```

### DVM vs ART
Android apps ek Virtual Machine pe chalti hain — pehle **Dalvik VM (DVM)**, ab **ART (Android Runtime)**. Isliye code seedha "machine code" nahi, balki **bytecode** hota hai — jo desktop se reverse karna aasan hai!

> **Key point:** DEX bytecode high-level info (class names, method names) rakhta hai, isliye reverse karke wapas Java jaisa code aasani se milta hai.

---

## 7A.2 — Smali Language (Android Ki "Assembly")

Ye Android RE ka **dil** hai. Jaise desktop mein Assembly thi, waise Android mein **Smali** hai.

### Smali kya hai?
Smali, Dalvik bytecode ka human-readable version hai. `.dex` ko todoge toh smali dikhega. Ye Java se low-level hai but desktop assembly se aasan (registers aur methods clearly dikhte hain).

### Example — Ek simple smali
```smali
.method public checkPassword(Ljava/lang/String;)Z
    .locals 2
    const-string v0, "admin123"        # v0 register mein "admin123" daalo
    invoke-virtual {p1, v0}, Ljava/lang/String;->equals(Ljava/lang/Object;)Z
    move-result v1                     # equals() ka result v1 mein
    return v1                          # true/false return
.end method
```
**Line by line:**
- `.method public checkPassword(...)Z` — method ka naam, `Z` matlab boolean (true/false) return karega.
- `const-string v0, "admin123"` — v0 register mein string "admin123" daali.
- `invoke-virtual {p1, v0}, ...equals` — `p1` (user input) pe `.equals(v0)` call kiya — matlab input ko "admin123" se compare.
- `move-result v1` — comparison ka result v1 mein.
- `return v1` — result wapas.

> **Password mil gaya: `admin123`** — smali padhna aa gaya toh aadha kaam ho gaya!

### Smali seekhne ke key points
- **Registers:** `v0, v1...` (local) aur `p0, p1...` (parameters; `p0` = `this` object).
- **Common opcodes:** `const-string`, `invoke-virtual`, `invoke-static`, `if-eq`, `if-ne`, `move-result`, `return`, `goto`.
- **Types:** `Z`=boolean, `I`=int, `V`=void, `Ljava/lang/String;`=String object.

---

## 7A.3 — Android RE Tools

| Tool | Kaam | Level |
|------|------|-------|
| **jadx / jadx-gui** ⭐ | APK ko seedha Java code mein dikhaata hai (best beginner tool) | Free |
| **apktool** ⭐ | APK ko smali mein todta hai aur wapas build karta hai (patching) | Free |
| **dex2jar** | `.dex` ko `.jar` mein badalta hai | Free |
| **JD-GUI / Bytecode Viewer** | Java code dekhna | Free |
| **Ghidra / IDA** | `lib/*.so` (native C/C++) reverse karna | Free |
| **Android Studio + Emulator** | App chalana/debug karna | Free |
| **Frida** ⭐⭐ | Runtime hooking (dynamic) — sabse powerful | Free |

> **Beginner recommendation:** **jadx-gui** se shuru karo. APK drag-drop karo, turant Java-jaisa code dikh jaayega.

---

## 7A.4 — Android Static Analysis

Bina app chalaye samajhna:

1. **APK ko jadx-gui mein kholo** → Java code padho.
2. **AndroidManifest.xml padho** → kaunsi Activity pehle chalti hai (entry point, usually `MainActivity`), kya permissions maangi.
3. **Strings dhoondo** → URLs, API keys, passwords aksar hardcoded milte hain.
4. **Interesting functions** → login, password check, encryption wale functions dhoondo.
5. **Native `.so` files** → agar logic native code mein hai toh Ghidra mein kholo (yahan Phase 1 ki ARM assembly kaam aayegi!).

### Real example flow
```
Ek "Login" app mila
  → jadx-gui mein khola
  → MainActivity dhoondi
  → checkLogin() function mila
  → dekha: if(password.equals("Sup3rS3cr3t"))
  → Password mil gaya bina app chalaye! 🎯
```

---

## 7A.5 — Android Dynamic Analysis

App ko **chalate hue** dekhna:
- **Emulator setup** — Android Studio ka emulator (safe, computer pe hi Android chalega). Rooted emulator best hai.
- **Logcat** — app ke logs dekhna (developers debug info chhod dete hain).
- **Frida** ⭐⭐ — Sabse powerful tool. Chalti hui app mein "hook" laga ke functions ko rok/badal sakte ho, runtime values dekh sakte ho.
  - Example: `equals()` function hook karke dekho asli value kya compare ho rahi.
- **Objection** — Frida ke upar bana, aasan commands (SSL pinning bypass, root detection bypass).
- **Burp Suite / mitmproxy** — app ka network traffic dekhna (kaunse server se baat kar rahi).

### Frida hooking example (concept)
```javascript
// login function ko hook karke uske arguments print karna
Java.perform(function() {
    var Login = Java.use("com.app.LoginActivity");
    Login.checkPassword.implementation = function(pass) {
        console.log("Password argument: " + pass);   // asli input print
        return true;   // hamesha true return — bypass!
    };
});
```
**Samjho:**
- `Java.use(...)` — target class pakdi.
- `.implementation = function(...)` — us method ko replace kiya.
- `console.log(pass)` — jo bhi password aaya, print kar diya.
- `return true` — function ko hamesha "sahi" bana diya (login bypass).

---

## 7A.6 — APK Patching (Modify & Rebuild)

App ka behaviour badalna:
```bash
apktool d app.apk          # 1. APK ko smali mein todo (decompile)
# 2. smali file edit karo (jaise "if-eqz" ko badlo ya return value change karo)
apktool b app_folder -o new.apk   # 3. wapas APK banao (build)
# 4. APK ko sign karo (warna install nahi hoga)
apksigner sign --ks my.keystore new.apk
# 5. install & test (emulator/phone pe)
adb install new.apk
```
> Example: Ek "premium locked" feature ko smali mein `return true` bana ke unlock karna (learning ke liye). Ya login check ko hamesha pass karwana.

> ⚠️ Sirf apni ya practice apps pe. Kisi ki paid app ki protection todna illegal hai.

---

## 7A.7 — Android Advanced Topics
- **Obfuscation** — ProGuard/R8/DexGuard se naam ulte (`a.a.a()`) — inhe trace karna.
- **Root detection bypass** — apps jo rooted device pe chalne se mana karti hain.
- **SSL Pinning bypass** — network traffic dekhne ke liye (Objection/Frida se).
- **Anti-tampering bypass** — apps jo modify hona detect karti hain.
- **Native code RE** — `.so` files (ARM assembly) — desktop RE skills seedha kaam aati hain.
- **Frida scripting** — apne custom hooks likhna.

**✅ Android Part Complete Jab:** Tum ek APK ko jadx mein khol ke logic samajh sako, smali padh sako, Frida se ek function hook kar sako, aur ek simple patch karke APK rebuild kar sako.

---

# 🍎 PART B — iOS REVERSE ENGINEERING
**⏱️ Time: 3-4 hafte**

> ⚠️ **Note:** iOS RE thoda mushkil aur mehanga hai. Ideally chahiye: ek **Mac** (kuch tools sirf Mac pe) aur ek **jailbroken iPhone** (dynamic analysis ke liye). Bina inke bhi static analysis ho sakta hai, but limited. Isliye Android pehle solid karo.

iOS apps **Swift/Objective-C** mein banti hain aur seedha **ARM machine code** (native) mein compile hoti hain — Android ke bytecode jaisa aasan nahi. Yahan desktop RE skills (assembly, Ghidra) zyada kaam aati hain.

---

## 7B.1 — iOS App Basics

### IPA file kya hai?
Android ke APK ki tarah, iOS ka app package. Ye bhi ek **ZIP** hai! Andar `Payload/AppName.app/` folder hota hai.

### IPA ke andar kya hota hai
| File | Kya Hai |
|------|---------|
| `Payload/AppName.app/` | Asli app folder |
| `AppName` (binary) | ⭐ Main executable — Mach-O format (ARM machine code) |
| `Info.plist` | App ki config/identity (Android ke Manifest jaisa) |
| `Frameworks/` | Extra libraries |
| `_CodeSignature/` | Apple ka digital signature |
| Assets (`.car`) | Images, resources |

### Mach-O format
iOS/Mac binaries ka format (jaise Windows mein PE, Linux mein ELF). Ghidra/IDA ise samajhte hain.

### Encryption problem (⭐ important)
App Store se aane wali apps **encrypted** hoti hain (FairPlay DRM). Reverse karne ke liye pehle **decrypt** karni padti hai — jailbroken device pe, jab app memory mein decrypt ho jaati hai tab use dump karke (jaise `frida-ios-dump` se).

---

## 7B.2 — Objective-C & Swift Samjho

- **Objective-C** — purani but common. Ismein method calls ek special function `objc_msgSend` ke through hote hain — ye RE mein bahut dikhega.
- **Swift** — modern, but reverse karna thoda mushkil (name mangling — naam encode ho jaate hain).
- Basic samajh honi chahiye ki inka code kaisa dikhta hai — classes, methods, message passing.

---

## 7B.3 — iOS RE Tools

| Tool | Kaam | Note |
|------|------|------|
| **Ghidra / IDA Pro** ⭐ | Mach-O binary disassemble/decompile | Free/Paid |
| **Hopper Disassembler** | iOS/Mac ke liye popular, achha UI | Paid (trial) |
| **class-dump** | Objective-C classes/methods ki list nikaalna | Free |
| **Frida** ⭐ | Dynamic analysis, hooking (jailbroken device) | Free |
| **Objection** | Frida-based, aasan commands | Free |
| **frida-ios-dump** | Encrypted app ko decrypt/dump karna | Free |
| **otool / nm** | Binary info, symbols dekhna (Mac tools) | Free |

---

## 7B.4 — iOS Static Analysis
1. **IPA ko unzip karo** → binary aur `Info.plist` nikaalo.
2. **Info.plist padho** → permissions, URL schemes, config.
3. **App decrypt karo** (agar App Store se) → `frida-ios-dump` se.
4. **class-dump** → saari classes aur methods ki list (kya-kya functions hain).
5. **Ghidra/Hopper mein binary kholo** → interesting functions (login, crypto) dhoondo, ARM assembly padho.
6. **Strings** → hardcoded secrets, URLs.

---

## 7B.5 — iOS Dynamic Analysis
> Iske liye **jailbroken iPhone** chahiye (ya kuch cases mein simulator, but limited).
- **Frida hooking** — chalti hui app ke functions rok/badal ke dekhna (Android jaisa hi, but Objective-C/Swift methods).
- **Objection** — SSL pinning bypass, jailbreak detection bypass, method hooking aasan commands se.
- **Debugserver / LLDB** — Apple ka debugger, breakpoints lagana.
- **Network analysis** — Burp Suite se traffic dekhna.
- **Cycript / Frida REPL** — live app mein objects explore karna.

---

## 7B.6 — iOS Advanced Topics
- **FairPlay decryption** deeply samajhna.
- **Anti-jailbreak detection bypass** — apps jo jailbroken device pe chalne se mana karti hain.
- **SSL pinning bypass** — network dekhne ke liye.
- **Swift-specific RE** — name demangling, metadata.
- **Kernel-level / system RE** — bahut advanced.

**✅ iOS Part Complete Jab:** Tum ek IPA ko unzip/analyze kar sako, class-dump se structure samajh sako, Ghidra/Hopper mein binary explore kar sako, aur (jailbroken device ho toh) Frida se hook laga sako.

---

## 🔑 Android vs iOS — Quick Comparison

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

> **Rule of thumb:** Android pehle poora seekho (aasan + free), phir iOS. **Frida dono mein common hai** — isko achhe se seekhna dono jagah kaam aayega.

---

## 📱 Mobile RE — Practice & Resources
- 🌐 **OWASP MASTG (Mobile App Security Testing Guide)** — mobile RE/security ki bible, free.
- 🌐 **MASVS** — mobile security standards.
- 🧪 **DIVA / InsecureBankv2 / OWASP UnCrackable** — jaan-boojh ke kamzor banaayi gayi practice apps (Android + iOS dono).
- 📱 Apni khud ki banaayi hui simple app reverse karke shuru karo (safe + legal).
- 🎥 YouTube: LiveOverflow (mobile), 8ksec, TheGrayArea.

---

## 🎯 Key Takeaways (Ek Line Mein)

1. **APK aur IPA dono ZIP files hain** — rename karke khol sakte ho.
2. Android code = **bytecode (DEX)** — aasan; iOS code = **native ARM** — mushkil.
3. **Smali** = Android ki assembly; padhna aa gaya toh aadha kaam.
4. Android tools: **jadx** (Java dekho), **apktool** (patch), **Frida** (hook).
5. iOS apps **encrypted** (FairPlay) — pehle decrypt/dump karni padti (frida-ios-dump).
6. iOS tools: **Ghidra/Hopper**, **class-dump**, **Frida**, **Objection**.
7. **Frida dono platforms mein** sabse powerful — zaroor seekho.
8. **Android pehle** (free), phir iOS (Mac + jailbroken device).

---

> ✅ **Phase 7 Complete Jab:** Tum Android APK aur (resources ho toh) iOS IPA dono ko static + dynamic dono tarike se analyze kar sako, aur ek practice app ka logic/secret nikaal sako. Congratulations — ab tum ek Mobile RE bhi ho! 🎉🚀
