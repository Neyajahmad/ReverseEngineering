# Phase 6 — Interview Questions & Answers (Hinglish)

> Specialization aur advanced topics se related important interview questions, simple Hinglish jawaab ke saath.

---

### Q1. Reverse Engineering mein specialization kyun zaroori hai?
**Ans:** RE bahut bada field hai — malware, exploits, mobile, crypto, firmware sab alag areas hain, har ek ke apne tools aur techniques hain. Sab mein expert banna mushkil hai, isliye 1-2 areas chunkar unme deep jaana chahiye. Isse mastery aati hai aur career focused hota hai.

---

### Q2. Malware Analyst kya karta hai?
**Ans:** Malware analyst virus, ransomware, trojans jaise malicious software ko analyze karta hai — samajhta hai wo kaise kaam karta, kaise phailta, kya chura raha hai. Fir detection signatures (jaise YARA rules) aur IOCs banata hai taaki systems ko bachaaya ja sake. Ye role security companies mein bahut demand mein hai.

---

### Q3. Vulnerability researcher aur exploit developer kya karte hain?
**Ans:** Ye software mein bugs (kamzoriyan) dhoondte hain — jaise buffer overflow, use-after-free — aur unhe exploit karke dikhate hain ki wo kitne khatarnaak hain. Isse companies bugs fix kar paati hain. Ismein deep assembly, memory corruption, aur exploit development (ROP, shellcode) ki samajh chahiye. Bug bounty aur red team mein ye skills kaam aati hain.

---

### Q4. Packer kya hota hai aur unpacking kya hoti hai?
**Ans:** Packer ek tool hai jo program ke code ko compress ya encrypt kar deta hai (jaise UPX), taaki analysis mushkil ho aur size chhota ho. Unpacking matlab us packed code ko wapas asli form mein laana — usually program ko chala ke, jab wo khud ko memory mein unpack kar leta hai, tab OEP (Original Entry Point) pe memory dump le lete hain.

---

### Q5. Obfuscation kya hoti hai?
**Ans:** Obfuscation matlab code ko jaan-boojh ke complex/confusing banana taaki reverse karna mushkil ho — bina behaviour badle. Techniques: junk (bekaar) code daalna, control flow flatten karna, strings encrypt karna, naam ulte-pulte karna. Deobfuscation ismein wo confusion hataake asli logic nikaalna hai.

---

### Q6. Exploit development mein ASLR, DEP, stack canary kya hote hain?
**Ans:** Ye modern security protections hain jo exploits ko rokte hain:
- **ASLR** — memory addresses ko har baar random kar deta hai, taaki attacker ko address pata na chale.
- **DEP/NX** — data wale memory ko execute hone se rokta hai (shellcode na chale).
- **Stack canary** — stack pe ek secret value rakhta hai; overflow se wo change ho jaaye toh program detect kar leta hai.

Exploit developers inhe bypass karne ke techniques (jaise ROP) seekhte hain.

---

### Q7. RE mein scripting/automation kyun important hai?
**Ans:** Bahut sa RE kaam repetitive hota hai (patterns dhoondna, decrypt karna, inputs solve karna). Python jaise scripting se ye automate ho jaata hai. Ghidra scripting se analysis tasks, angr se automatic input solving (symbolic execution), aur Frida se runtime hooking automate hoti hai. Isse time bachta hai aur bade programs handle hote hain.

---

### Q8. angr framework kya hai?
**Ans:** angr ek Python-based binary analysis framework hai jo "symbolic execution" karta hai — matlab program ko symbolically chalaakar automatically wo input dhoond leta hai jo kisi specific path (jaise "access granted") tak le jaaye. RE mein ise password/key automatically solve karne ke liye use karte hain.

---

### Q9. Firmware RE aam software RE se kaise alag hai?
**Ans:** Firmware embedded devices (routers, cameras, IoT) ka software hota hai. Ye often ARM/MIPS architecture pe hota hai (x86 nahi), aur pehle device se firmware extract karna padta hai (binwalk jaise tools se). Kabhi hardware interfaces (UART, JTAG) se seedha chip se code nikalna padta hai. Baaki RE concepts same rehte hain.

---

### Q10. YARA rules kya hote hain?
**Ans:** YARA rules ek tarah ke "patterns" hain jinse malware ko identify/detect kiya jaata hai. Rule mein hum specific strings, bytes, ya conditions likhte hain jo kisi malware family mein milte hain. Malware analysts ye rules banate hain taaki antivirus/scanners similar malware pakad sakein.

---

### Q11. RE mein career ke kaunse options hain?
**Ans:** Malware Analyst, Reverse Engineer, Vulnerability Researcher, Security Researcher, Penetration Tester (binary/mobile specialty), aur Threat Intelligence Analyst. Ye roles security companies, antivirus vendors, product security teams, aur bug bounty (freelance) mein hote hain.

---

### Q12. Specialization choose karte waqt kaunsi cheez sabse zyada matter karti hai?
**Ans:** Job demand aur pay important hain (Malware Analysis aur Vuln Research top pe hain), lekin sabse zyada matter karta hai ki tumhe kis cheez mein maza aata hai. Kyunki RE lambi mehnat maangti hai, aur consistency wahin aayegi jahan interest hoga. Interest + demand ka combo best hai.
