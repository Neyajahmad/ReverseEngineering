# Phase 3 — Interview Questions & Answers (Hinglish)

> Static Analysis se related important interview questions, simple Hinglish jawaab ke saath.

---

### Q1. Static analysis kya hai?
**Ans:** Static analysis matlab program ko **chalaye bina** analyze karna — sirf uska code (assembly ya decompiled C) padhkar samajhna ki wo kya karega. Ye safe hota hai (malware bhi chalta nahi) aur poora code ek saath dikh jaata hai.

---

### Q2. Static analysis ke fayde aur nuksan kya hain?
**Ans:**
- **Fayde:** Safe hai (kuch execute nahi hota), poora code aur saare code paths dikhte hain (chhupe hue bhi).
- **Nuksan:** Encrypted/packed code samajhna mushkil, aur runtime pe banne wali values (jaise decrypt hua password) nahi dikhtin.

Isliye ise dynamic analysis ke saath milaake use karte hain.

---

### Q3. Kisi program ka analysis shuru karte waqt pehla step kya hota hai?
**Ans:** File ka **recon** — file type (PE/ELF, 32/64-bit), packed hai ya nahi (DIE tool se), aur kaunse compiler/language se bani. Uske baad **strings** nikaalna, jahan aksar passwords, messages, aur URLs jaise clues seedha mil jaate hain.

---

### Q4. Strings analysis se kya milta hai?
**Ans:** File ke andar chhupe readable text — jaise hardcoded passwords/keys, URLs ya IP addresses (malware ke C2 servers), success/error messages ("Access granted!", "Wrong password!"), file paths, aur registry keys. Ye pehla aur sabse aasan clue source hai.

---

### Q5. Entry point aur main() mein kya farak hai?
**Ans:** **Entry point** wo jagah hai jahan se program technically chalu hota hai — ismein runtime setup code hota hai. **main()** wo function hai jahan program ka asli logic likha hota hai. RE mein hum usually main() se analysis shuru karte hain.

---

### Q6. Program ka control flow static analysis mein kaise pehchante hain?
**Ans:** If-else `cmp` + conditional jump (`je`/`jne`) se bante hain, aur loops ek backward jump (upar wali line pe wapas kudna) se. Decompiler ismein `if`, `for`, `while` jaisa dikha deta hai. Ghidra ka Control Flow Graph (CFG) boxes aur arrows se ye flow visually dikhata hai.

---

### Q7. `strcmp` function dikhe toh kya samajhna chahiye?
**Ans:** `strcmp` do strings ko compare karta hai aur barabar hone pe **0** return karta hai. RE mein jab `strcmp(input, "kuch") == 0` dikhe, to samajh jao wahan koi string comparison ho raha hai — aksar password ya key check. Ye function seedha logic tak le jaata hai.

---

### Q8. Decompiler output padhte waqt kaunsi baatein dhyaan mein rakhni chahiye?
**Ans:** Variable naam ajeeb honge (`local_8`, `iVar1`, `param_1`) — unhe samajhte hue rename karo. Types galat ho sakte hain (int vs pointer) — sahi type set karo. Aur decompiler kabhi galti karta hai — doubt ho to assembly (Listing) dekho.

---

### Q9. Rename aur comment karna static analysis mein kyun zaroori hai?
**Ans:** Decompiler generic naam deta hai jo samajhna mushkil hai. Jaise-jaise logic samajhte ho, functions/variables ko meaningful naam do aur notes (comments) likho. Ye ek "map banane" jaisa hai — jitna map banega, poora program utna clear hota jaayega.

---

### Q10. Agar password seedha strcmp mein na ho, encoded ho, tab kya karoge?
**Ans:** Aksar password XOR ya kisi simple encoding se chhupaya hota hai. Tab code se encoding logic samajhkar (jaise har byte ko `0x10` se XOR) usko ulta karke asli password nikaalna padta hai. Ya dynamic analysis mein breakpoint lagaakar decrypt hui value memory mein dekh lete hain.

---

### Q11. Cross-references (XREF) static analysis mein kaise madad karte hain?
**Ans:** XREF batata hai koi string ya function kahan use hua. Jaise agar "Wrong password!" string mili, uske XREF se seedha wo function mil jaata hai jahan password check ho raha hai. Isse important logic tak jaldi pahunch jaate hain bina poora code padhe.

---

### Q12. Packed binary static analysis mein problem kyun banta hai?
**Ans:** Packed binary ka asli code compress/encrypt hota hai, isliye static mein sirf packer ka code aur garbage dikhta hai — asli logic aur strings chhupe hote hain. Ise pehle unpack karna padta hai (ya dynamic analysis mein memory se dump karna padta hai) tab hi asli code analyze ho paata hai.
