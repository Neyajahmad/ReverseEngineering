# Phase 7 — Interview Questions & Answers (English)

> The same Mobile RE (Android + iOS) interview questions, answered in clear English.

---

### Q1. What is an APK file?
**Ans:** An APK (Android Package) is an Android app's installation package. It's actually a **ZIP file** — you can rename and open it. Inside are `AndroidManifest.xml`, `classes.dex` (the actual code), resources, `lib/` (native code), and the signature.

---

### Q2. What is stored in the `classes.dex` file?
**Ans:** `classes.dex` contains the app's actual code as Dalvik bytecode — the code Android's virtual machine (ART/Dalvik) runs. In RE we reverse it back into Java/smali to read it.

---

### Q3. What is Smali?
**Ans:** Smali is the human-readable form of Dalvik bytecode — Android's "assembly language." Decompiling a `.dex` yields smali. It's lower-level than Java but easier than desktop assembly, since it clearly shows registers (`v0`, `p1`) and method names.

---

### Q4. What is DVM/ART, and why is Android code easier to reverse?
**Ans:** Android apps run on a virtual machine — formerly Dalvik VM (DVM), now ART (Android Runtime). So the app's code is **bytecode**, not raw machine code, and it retains high-level info (class/method names). This makes it easy to reverse back into readable Java-like code — far easier than desktop native code.

---

### Q5. What is the difference between jadx and apktool?
**Ans:** **jadx** shows an APK directly as readable Java code — best for understanding code (static analysis). **apktool** breaks an APK into smali and can rebuild it into an APK — used for patching (modifying code and rebuilding).

---

### Q6. Why is AndroidManifest.xml important?
**Ans:** It's the app's "identity card." It lists all components (Activities, Services), permissions (what access it requests), and the entry point (which Activity launches first, usually MainActivity). When starting RE, it tells you where the app begins and what it does.

---

### Q7. What is Frida and why is it important in mobile RE?
**Ans:** Frida is a dynamic instrumentation tool that lets you "hook" functions in a running app — pause them, view/modify arguments and return values, or replace them entirely. It works on **both** Android and iOS, making it the most powerful and essential mobile RE tool (e.g., login bypass, SSL pinning bypass).

---

### Q8. What is the APK patching process?
**Ans:** Steps: (1) `apktool d app.apk` to decompile the APK into smali, (2) edit the smali code (e.g., change a check's result), (3) `apktool b` to rebuild the APK, (4) sign the APK (`apksigner`), otherwise it won't install, (5) `adb install` to install and test on an emulator/phone.

---

### Q9. What is an IPA file and how does it differ from an APK?
**Ans:** An IPA is an iOS app package (the iOS counterpart of an APK), and it's also a ZIP file containing a `Payload/AppName.app/` folder. Key difference: the IPA's binary is **native ARM machine code** (Mach-O format) — harder than Android's bytecode. Also, App Store apps are **encrypted** (FairPlay).

---

### Q10. Why is reversing iOS apps harder than Android?
**Ans:** Two reasons: (1) iOS apps compile to native ARM machine code (not high-level bytecode like Android), so you don't get readable code back — you must read assembly. (2) App Store apps are encrypted with FairPlay DRM and must first be decrypted/dumped on a jailbroken device. Plus the setup is expensive (Mac + jailbroken iPhone).

---

### Q11. What is FairPlay encryption and how do you handle it?
**Ans:** FairPlay is Apple's DRM that encrypts App Store app binaries so they can't be directly reversed. To handle it, you run the app on a jailbroken device — once the app decrypts itself in memory, you dump the decrypted binary from memory using a tool like `frida-ios-dump`, then analyze it in Ghidra/Hopper.

---

### Q12. What does the class-dump tool do?
**Ans:** class-dump extracts all Objective-C classes, methods, and properties from an iOS/Mac binary (like header files). At a glance it reveals which functions exist — such as `checkLogin`, `isPremium` — and where to focus your analysis.

---

### Q13. What is SSL pinning and why do you bypass it?
**Ans:** SSL pinning is a security technique where an app trusts only a specific certificate, so no one in the middle (a proxy) can read its traffic. In RE/testing we want to see the app's network traffic (via Burp Suite), so we bypass SSL pinning (using Objection/Frida) to let the traffic pass through the proxy and be visible.

---

### Q14. What common skill is best to learn for both Android and iOS RE?
**Ans:** **Frida** — because it works on both platforms. Once you learn Frida hooking well, you cover dynamic analysis (function hooking, bypasses, runtime inspection) for both Android and iOS. It's the highest-value skill in mobile RE.

---

### Q15. What are a mobile app's native `.so` files and how do you reverse them?
**Ans:** `.so` files live in an Android APK's `lib/` folder — they're native code written in C/C++ and compiled to ARM assembly (for performance- or security-sensitive logic). You reverse them with Ghidra/IDA — here your desktop RE and ARM assembly skills apply directly, since this is native code, not bytecode.
