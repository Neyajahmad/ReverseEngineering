# Phase 4 — Interview Questions & Answers (Hinglish)

> Dynamic Analysis se related important interview questions, simple Hinglish jawaab ke saath.

---

### Q1. Dynamic analysis kya hai?
**Ans:** Dynamic analysis matlab program ko **actually chalakar**, beech-beech mein rok-rok ke, live values dekhkar samajhna. Static "book padhne" jaisa tha, dynamic "movie dekhne" jaisa hai — cheezein hoti hui dikhti hain, jaise register aur memory ki actual values.

---

### Q2. Dynamic analysis kab zaroori ho jaata hai?
**Ans:** Jab code encrypted ya packed ho (static mein samajh na aaye), jab password/key runtime pe decrypt ya generate hota ho, ya jab logic itna complex ho ki chala ke dekhna aasan ho. Aisi situations mein static kaafi nahi hota.

---

### Q3. Breakpoint kya hota hai?
**Ans:** Breakpoint ek nishaan hai jahan program apne aap ruk jaata hai, taaki hum us point pe registers, memory aur stack dekh sakein. Ye debugger ka sabse important tool hai. Types: software, conditional (jab koi condition sach ho), aur hardware (memory read/write pe).

---

### Q4. Step Into, Step Over aur Step Out mein kya farak hai?
**Ans:**
- **Step Into (F7):** agar current line ek function call hai, uske andar ghus jao.
- **Step Over (F8):** function ko poora chala do, but uske andar mat jao.
- **Step Out (Ctrl+F9):** current function se bahar nikal jao (jahan se call hua tha).

---

### Q5. Dynamic analysis ka sabse bada fayda kya hai?
**Ans:** Runtime pe **asli values** dikh jaati hain. Jaise agar password encrypted compare hota hai, static mein sirf encrypted bytes dikhenge, lekin comparison function pe breakpoint lagaakar hum decrypt hui asli value seedha memory mein dekh sakte hain — bina encryption samjhe.

---

### Q6. Password nikaalne ke liye breakpoint kahan lagana chahiye?
**Ans:** Comparison functions pe — `strcmp`, `strncmp`, `memcmp`. Yahan program tumhare input ko expected (asli) value se compare karta hai, aur us waqt dono values (tumhara input + asli password) memory/registers mein dikh jaati hain. Input padhne wale functions (`scanf`/`fgets`) ke baad bhi useful hota hai.

---

### Q7. Patching kya hoti hai?
**Ans:** Patching matlab program ke instructions/bytes ko badal ke uska behaviour change karna. Classic example: password check ke `jne` (jump if not equal) ko `je` ya `nop` mein badal dena — jisse galat password pe bhi "access granted" chalne lage.

---

### Q8. NOP instruction kya karti hai aur patching mein kaise use hoti hai?
**Ans:** `nop` (No Operation, byte `0x90`) ka matlab "kuch mat karo, agli instruction pe jao". Patching mein hum kisi unwanted instruction (jaise ek check ya jump) ko `nop` se replace kar dete hain, taaki wo effectively hat jaaye aur program aage badh jaaye.

---

### Q9. Static aur dynamic analysis ko saath mein kaise use karte hain?
**Ans:** Ek real workflow: pehle static (Ghidra) se overall structure aur interesting functions dhoondo, phir un functions pe dynamic (x64dbg) mein breakpoint lagaakar live values/decrypt data dekho, phir nayi samajh ke saath wapas static code padho. Ye loop repeat hota hai — ek se clue milta hai, doosre se confirm hota hai.

---

### Q10. Anti-debugging kya hota hai?
**Ans:** Ye techniques hain jinse program detect kar leta hai ki uspe debugger laga hai, aur phir alag behave karta hai ya band ho jaata hai — taaki analysis mushkil ho. Common tricks: `IsDebuggerPresent()` API, timing checks (code slow chala toh debugger hai), aur PEB flags dekhna.

---

### Q11. Anti-debugging ko bypass kaise karte hain (basic)?
**Ans:** `IsDebuggerPresent` jaise check ke return value ko patch karke 0 (no debugger) bana dete hain, ya anti-debug check ko `nop` se hata dete hain. Ghidra/x64dbg ke plugins (jaise ScyllaHide) automatically bahut si anti-debug tricks chhupa dete hain.

---

### Q12. Malware ka dynamic analysis karte waqt kya precaution zaroori hai?
**Ans:** Malware ko hamesha ek isolated **Virtual Machine (VM)** mein chalao, kabhi apne main PC pe nahi. VM ka network isolate rakho aur snapshots use karo, taaki kuch bigad jaaye toh clean state pe wapas aa sako. Isse tumhara asli system aur data surakshit rehta hai.
