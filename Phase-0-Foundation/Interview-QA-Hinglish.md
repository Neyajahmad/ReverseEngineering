# Phase 0 — Interview Questions & Answers (Hinglish)

> Ye important theory questions hain jo interview mein Phase 0 (Foundation) se puche ja sakte hain. Har jawaab simple Hinglish mein — jaise tum interviewer ko bologe.

---

### Q1. Reverse Engineering kya hai?
**Ans:** Reverse Engineering ka matlab hai kisi cheez ko "ulta" samajhna — jab humein source code nahi pata, tab hum ek program (jaise `.exe`) ko todkar, andar dekhkar samajhte hain ki wo kaise kaam karta hai. Jaise ek band ghadi ko kholkar samajhna ki gears kaise chalte hain, waise hi software pe.

---

### Q2. Computer sirf binary (0 aur 1) kyun samajhta hai?
**Ans:** Computer ke andar electronic circuits hote hain jinme sirf do states hoti hain — current on (**1**) ya off (**0**). Isiliye computer sab kuch binary mein store aur process karta hai. Hardware level pe sirf yahi do cheezein possible hain.

---

### Q3. Hexadecimal kyun use karte hain, binary kyun nahi?
**Ans:** Binary bahut lambi hoti hai — jaise `11111111`. Hex chhoti aur padhne mein aasan hai — wahi cheez `0xFF` ho jaati hai. Ek hex digit exactly 4 binary digits (bits) ko represent karta hai, isliye conversion bhi seedha hai. RE tools memory aur values hex mein hi dikhate hain.

---

### Q4. 1 byte mein kitni values aa sakti hain?
**Ans:** 1 byte = 8 bits. 8 bits se 2^8 = **256 alag values** ban sakti hain, matlab **0 se 255** tak. Hex mein ye `0x00` se `0xFF` tak hota hai.

---

### Q5. Memory address kya hota hai?
**Ans:** RAM ek badi building jaisi hai jismein har "flat" ka ek unique number hota hai — usi ko memory address kehte hain. Computer har data (variable, code) ko kisi address pe rakhta hai, taaki baad mein use dhoond sake. RE mein hum baar-baar dekhte hain ki kisi address pe kya value hai.

---

### Q6. Endianness kya hoti hai? Little aur Big endian mein kya farak hai?
**Ans:** Endianness batati hai ki ek multi-byte number memory mein kis order mein store hoga.
- **Little Endian:** sabse chhota (least significant) byte pehle store hota hai. Intel/AMD (x86/x64) yahi use karte hain.
- **Big Endian:** sabse bada byte pehle. Network protocols mein common.

Example: `0x12345678` little endian mein memory mein `78 56 34 12` (ulta) dikhega, big endian mein `12 34 56 78`.

---

### Q7. Pointer kya hota hai? Isse RE mein kya importance hai?
**Ans:** Pointer ek variable hai jo kisi doosre variable ki **value nahi, balki address** rakhta hai. `&x` se x ka address milta hai, aur `*p` se us address pe rakhi value milti hai. RE mein pointer bahut important hai kyunki assembly level pe zyada tar kaam addresses ke through hota hai — data ko address se access kiya jaata hai.

---

### Q8. ASCII kya hai? 'A' ki value kya hoti hai?
**Ans:** ASCII ek standard hai jo har character ko ek number deta hai, taaki computer text ko store kar sake. Jaise `'A'` = 65 (hex `0x41`), `'a'` = 97 (`0x61`), `'0'` = 48. RE mein jab memory mein `41 42 43` dikhta hai, wo actually `"ABC"` hota hai.

---

### Q9. Compilation process kya hai? Source code se .exe kaise banta hai?
**Ans:** Steps hote hain:
1. **Preprocessor** — `#include` jaise kaam handle karta hai.
2. **Compiler** — C code ko assembly mein badalta hai.
3. **Assembler** — assembly ko machine code (object file) mein.
4. **Linker** — libraries jodkar final `.exe` banata hai.

RE isi ka **ulta** hai — hum `.exe` se wapas samajhne ki koshish karte hain.

---

### Q10. Compile karne pe kaunsi information kho jaati hai?
**Ans:** Compile hone pe variable ke naam (jaise `age`, `price`), function ke asli naam, comments, aur data types ki clear info kho jaati hai. Machine code mein sirf raw instructions bachti hain. Isliye RE mushkil hota hai — hume ye khoyi hui information wapas guess karni padti hai.

---

### Q11. Machine code aur Assembly mein kya farak hai?
**Ans:** **Machine code** pure numbers (0s aur 1s / bytes) hote hain jo CPU seedha chalata hai. **Assembly** usi machine code ka human-readable version hai — jaise `mov eax, 5`. Har assembly instruction ek machine code se map hoti hai. Assembly humein padhne mein aasan hai.

---

### Q12. Stack aur Heap mein basic farak kya hai?
**Ans:** Dono memory ke hisse hain jahan program data rakhta hai.
- **Stack:** automatic, fast, function ke local variables aur call info yahan aati hai. Order fixed hota hai (LIFO — Last In First Out).
- **Heap:** manual/dynamic memory, jise program chalte-chalte maangta hai (jaise C mein `malloc`). Zyada flexible but manage karna padta hai.

---

### Q13. Bit aur Byte mein farak?
**Ans:** **Bit** sabse chhoti unit hai — ek `0` ya `1`. **Byte** = 8 bits ka group. Ek byte se 0-255 tak number ban sakta hai. Data usually bytes mein measure hota hai (KB, MB, GB).

---

### Q14. RE seekhne ke liye C language kyun zaroori hai?
**Ans:** Kyunki zyada tar software C/C++ mein bane hote hain, aur decompiler (jaise Ghidra) ka output C jaisa dikhta hai. Agar tum C samajhte ho — especially pointers, arrays, functions — toh decompiled code padhna bahut aasan ho jaata hai.
