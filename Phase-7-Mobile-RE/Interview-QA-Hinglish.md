# Phase 7 — Interview Questions & Answers (Hinglish)

> Mobile RE (Android + iOS) se related important interview questions, simple Hinglish jawaab ke saath.

---

### Q1. APK file kya hoti hai?
**Ans:** APK (Android Package) ek Android app ka installation package hai. Ye asal mein ek **ZIP file** hoti hai — rename karke khol sakte ho. Andar `AndroidManifest.xml`, `classes.dex` (asli code), resources, `lib/` (native code), aur signature hoti hai.

---

### Q2. `classes.dex` file mein kya hota hai?
**Ans:** `classes.dex` mein app ka asli code hota hai — Dalvik bytecode ke form mein. Ye wahi code hai jo Android ki virtual machine (ART/Dalvik) chalati hai. RE mein isi ko reverse karke wapas Java/smali mein padhte hain.

---

### Q3. Smali kya hai?
**Ans:** Smali, Dalvik bytecode ka human-readable version hai — Android ki "assembly language". Jab `.dex` ko decompile karte hain toh smali milta hai. Ye Java se low-level hai but desktop assembly se aasan, kyunki ismein registers (`v0`, `p1`) aur method names clearly dikhte hain.

---

### Q4. DVM/ART kya hai aur Android code reverse karna aasan kyun hota hai?
**Ans:** Android apps ek virtual machine pe chalti hain — pehle Dalvik VM (DVM), ab ART (Android Runtime). Isliye app ka code seedha machine code nahi, balki **bytecode** hota hai jo high-level info (class/method names) rakhta hai. Isliye reverse karke wapas Java-jaisa readable code aasani se mil jaata hai — desktop native code se kaafi aasan.

---

### Q5. jadx aur apktool mein kya farak hai?
**Ans:** **jadx** APK ko seedha readable Java code mein dikhata hai — code samajhne (static analysis) ke liye best. **apktool** APK ko smali mein todta hai aur usse wapas APK build bhi kar sakta hai — matlab patching (code badalkar rebuild) ke liye use hota hai.

---

### Q6. AndroidManifest.xml kyun important hai?
**Ans:** Ye app ka "identity card" hai. Ismein app ke saare components (Activities, Services), permissions (kya-kya access maangti hai), aur entry point (kaunsi Activity pehle chalti hai, usually MainActivity) hote hain. RE shuru karte waqt yahan se pata chalta hai app kahan se shuru hoti hai aur kya karti hai.

---

### Q7. Frida kya hai aur mobile RE mein kyun important hai?
**Ans:** Frida ek dynamic instrumentation tool hai jo chalti hui app mein functions ko "hook" karne deta hai — matlab unhe rok sakte ho, arguments/return values dekh/badal sakte ho, ya poora replace kar sakte ho. Ye Android aur iOS **dono** mein kaam karta hai, isliye mobile RE ka sabse powerful aur zaroori tool hai (jaise login bypass, SSL pinning bypass).

---

### Q8. APK patching ka process kya hai?
**Ans:** Steps: (1) `apktool d app.apk` se APK ko smali mein decompile karo, (2) smali code edit karo (jaise koi check ka result badal do), (3) `apktool b` se wapas APK build karo, (4) APK ko sign karo (`apksigner`), warna install nahi hoga, (5) `adb install` se emulator/phone pe install karke test karo.

---

### Q9. IPA file kya hoti hai aur APK se kaise alag hai?
**Ans:** IPA iOS app ka package hai (APK ka iOS version), aur ye bhi ek ZIP file hai jisme `Payload/AppName.app/` folder hota hai. Bada farak: IPA ke andar binary **native ARM machine code** (Mach-O format) mein hoti hai — Android ke aasan bytecode se mushkil. Aur App Store apps **encrypted** (FairPlay) hoti hain.

---

### Q10. iOS apps reverse karna Android se mushkil kyun hai?
**Ans:** Do wajah: (1) iOS apps native ARM machine code mein compile hoti hain (Android ki tarah high-level bytecode nahi), isliye readable code wapas nahi milta — assembly padhni padti hai. (2) App Store apps FairPlay DRM se encrypted hoti hain, jinhe pehle jailbroken device pe decrypt/dump karna padta hai. Plus setup mehnga hai (Mac + jailbroken iPhone).

---

### Q11. FairPlay encryption kya hai aur ise kaise handle karte hain?
**Ans:** FairPlay Apple ka DRM hai jo App Store apps ki binary ko encrypt kar deta hai, taaki koi seedha reverse na kar sake. Handle karne ke liye jailbroken device pe app ko chalate hain — jab app memory mein khud ko decrypt kar leti hai, tab `frida-ios-dump` jaise tool se decrypted binary memory se dump kar lete hain, jise phir Ghidra/Hopper mein analyze karte hain.

---

### Q12. class-dump tool kya karta hai?
**Ans:** class-dump iOS/Mac binary se Objective-C ki saari classes, methods, aur properties ki list nikaal deta hai (header files jaise). Isse ek hi jhalak mein pata chal jaata hai ki app mein kaunse functions hain — jaise `checkLogin`, `isPremium` — aur kahan analysis focus karni hai.

---

### Q13. SSL pinning kya hai aur use bypass kyun karte hain?
**Ans:** SSL pinning ek security technique hai jismein app sirf ek specific certificate ko trust karti hai, taaki koi beech mein (proxy) traffic na dekh sake. RE/testing mein hum app ka network traffic dekhna chahte hain (Burp Suite se), isliye SSL pinning ko bypass karte hain (Objection/Frida se), taaki traffic proxy se guzre aur dikh sake.

---

### Q14. Android aur iOS RE mein kya common cheez seekhna best hai?
**Ans:** **Frida** — kyunki ye dono platforms mein kaam karta hai. Ek baar Frida hooking achhe se seekh li toh Android aur iOS dono ki dynamic analysis (function hooking, bypass, runtime inspection) cover ho jaati hai. Isliye ye mobile RE ka sabse high-value skill hai.

---

### Q15. Mobile app ki native `.so` files kya hoti hain aur unhe kaise reverse karte hain?
**Ans:** `.so` files Android APK ke `lib/` folder mein hoti hain — ye C/C++ mein likha, ARM assembly mein compiled native code hai (jaise performance ya security-sensitive logic). Inhe reverse karne ke liye Ghidra/IDA use karte hain — yahan desktop RE aur ARM assembly ki skills seedha kaam aati hain, kyunki ye bytecode nahi native code hai.
