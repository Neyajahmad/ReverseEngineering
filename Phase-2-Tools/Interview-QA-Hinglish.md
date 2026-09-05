# Phase 2 — Interview Questions & Answers (Hinglish)

> RE Tools se related important interview questions, simple Hinglish jawaab ke saath.

---

### Q1. Disassembler aur Decompiler mein kya farak hai?
**Ans:** **Disassembler** machine code ko assembly mein badalta hai — ye 100% exact hota hai (jaise IDA, objdump). **Decompiler** us code ko C-jaise high-level code mein badalta hai — padhna aasan hota hai but approximate hota hai, variable naam ajeeb aur kabhi galti bhi (jaise Ghidra decompiler, Hex-Rays). Best strategy: dono use karo.

---

### Q2. Ghidra kya hai aur beginners ke liye kyun accha hai?
**Ans:** Ghidra ek free, powerful RE tool hai jise NSA ne banaya. Ye disassembler aur decompiler dono deta hai — matlab assembly ke saath C-jaisa code bhi dikhata hai, jo padhna aasan hai. Free hone aur decompiler dene ki wajah se beginners ke liye best hai.

---

### Q3. Debugger kya hota hai aur wo disassembler se kaise alag hai?
**Ans:** **Disassembler** program ko chalaye bina uska code dikhata hai (static analysis). **Debugger** program ko **chalate hue** rok-rok ke dekhne deta hai — breakpoints lagana, ek-ek line chalana, live register aur memory values dekhna (dynamic analysis). Example: x64dbg, GDB.

---

### Q4. Static analysis aur dynamic analysis mein kya farak hai?
**Ans:** **Static analysis** program ko chalaye bina, sirf code padh ke samajhna hai (Ghidra se). **Dynamic analysis** program ko chalate hue, runtime pe values dekhkar samajhna hai (x64dbg se). Dono complementary hain — often dono milaake use karte hain.

---

### Q5. Cross-reference (XREF) kya hota hai aur kyun useful hai?
**Ans:** XREF batata hai ki koi function ya variable code mein **kahan-kahan use** hua hai. Jaise agar tumhe "password" string mili, toh uske XREF se pata chalega ki wo string kaunse function mein use ho rahi — seedha important logic tak pahunch jaate ho. RE mein clues follow karne ka ye best tarika hai.

---

### Q6. PE file format kya hai?
**Ans:** PE (Portable Executable) Windows executables (`.exe`, `.dll`) ka format hai. Ismein DOS header, PE header (32/64-bit info, entry point), aur sections hote hain jaise `.text` (code), `.data` (variables), `.rdata` (strings/constants), `.idata` (imports), `.rsrc` (resources). Linux ka equivalent ELF format hai.

---

### Q7. `.text` aur `.rdata` sections mein kya hota hai?
**Ans:** `.text` section mein program ka actual code (instructions) hota hai — yahin reverse karte hain. `.rdata` section mein read-only data hota hai jaise strings aur constants — yahan often passwords, URLs, error messages jaise clues milte hain.

---

### Q8. "Strings" tool kya karta hai aur RE mein kyun kaam aata hai?
**Ans:** Strings tool file ke andar chhupe readable text (printable characters) nikaal deta hai. RE mein ye pehla step hota hai — often hardcoded passwords, URLs, API keys, error messages, ya hints seedha strings mein mil jaate hain, bina deep analysis ke.

---

### Q9. Packed binary kya hota hai aur use kaise detect karte hain?
**Ans:** Packed binary wo hai jiska code compress ya encrypt kar diya gaya hai (jaise UPX packer se), taaki analysis mushkil ho. Aisi file mein strings kam dikhte hain aur code samajh nahi aata. Ise **Detect It Easy (DIE)** jaise tool se detect karte hain — wo batata hai file packed hai ya nahi aur kaunse packer se.

---

### Q10. Ghidra mein rename karna kyun important hai?
**Ans:** Decompiler variables ko ajeeb naam deta hai jaise `local_8`, `uVar1`. Jaise-jaise hum samajhte hain ki kaunsa variable kya kar raha, unhe meaningful naam de dete hain (jaise `user_input`, `password_check`). Isse code padhna aur logic samajhna bahut aasan ho jaata hai — ye "map banane" jaisa hai.

---

### Q11. x64dbg ke basic controls (F7, F8, F9) kya karte hain?
**Ans:**
- **F2** — breakpoint lagana/hatana (program yahan ruk jaayega).
- **F7** — Step Into (function ke andar chale jaao).
- **F8** — Step Over (function ko chala do, but andar mat jaao).
- **F9** — Run (agle breakpoint tak chalao).

---

### Q12. Kaunse tools se RE shuru karna best hai aur kyun?
**Ans:** **Ghidra + x64dbg** — dono free hain aur milkar 90% kaam kar dete hain. Ghidra static analysis (code padhna, decompiler) ke liye aur x64dbg dynamic analysis (runtime debugging) ke liye. Baad mein IDA, Binary Ninja jaise advanced tools try kar sakte ho.
