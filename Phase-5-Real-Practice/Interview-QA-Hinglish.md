# Phase 5 — Interview Questions & Answers (Hinglish)

> Real Practice, Lab Setup aur Malware Analysis se related important interview questions, simple Hinglish jawaab ke saath.

---

### Q1. RE seekhne ke liye practice itni zaroori kyun hai?
**Ans:** RE swimming jaisi hai — kitni bhi theory padho, jab tak khud challenges solve na karo, skill nahi aati. Practice se tumhe pattern pehchanana, tools ka fast use, aur real problems solve karna aata hai. Isliye har hafte crackmes/CTF solve karte rehna zaroori hai.

---

### Q2. Analysis ke liye Virtual Machine (VM) kyun use karte hain?
**Ans:** VM ek isolated (alag) computer hota hai tumhare PC ke andar. Malware ya unknown files VM mein chalane se tumhara asli system surakshit rehta hai. Agar malware kuch kharaab kare, toh sirf VM affect hota hai, main PC nahi.

---

### Q3. Snapshot kya hota hai aur kyun useful hai?
**Ans:** Snapshot VM ka "save point" hota hai — us waqt ki poori state. Malware analyze karne ke baad ya kuch bigadne pe, ek click mein us clean snapshot pe wapas aa sakte ho. Isse har baar fresh, clean environment mil jaata hai bina VM dubara banaye.

---

### Q4. Malware analyze karte waqt network isolation kyun zaroori hai?
**Ans:** Malware often internet se apne C2 (Command & Control) server se baat karta hai, ya network pe phailta hai. Network band/isolate karne se malware baahar nuksan nahi kar sakta, aur na hi tumhara IP/data leak hota hai. Analysis ke liye "host-only" ya no-network rakhte hain.

---

### Q5. Crackme kya hota hai?
**Ans:** Crackme ek program hota hai jo specially RE practice ke liye banaya jaata hai — legal aur safe. Ismein usually password/serial nikaalna ya koi protection bypass karna hota hai. crackmes.one jaisi sites pe difficulty-wise sorted milte hain, beginners easy se shuru karte hain.

---

### Q6. CTF kya hota hai?
**Ans:** CTF (Capture The Flag) ek hacking competition hai jismein challenges solve karke chhupe "flags" (secret strings) capture karne hote hain. Reversing category mein binaries ko reverse karke flag nikalte hain. picoCTF beginners ke liye best hai.

---

### Q7. Malware ka static aur dynamic analysis mein kya farak hai?
**Ans:** **Static malware analysis** malware ko chalaye bina hoti hai — strings, imports, PE structure dekhkar (safe). **Dynamic malware analysis** malware ko VM mein chala ke uska behaviour dekhna hai — kaunse files banata, registry/network kaise use karta. Dono milaake poori picture milti hai.

---

### Q8. IOC (Indicators of Compromise) kya hote hain?
**Ans:** IOCs wo nishaan hain jinse pata chalta hai ki system malware se infected hai — jaise malware ki file hashes, wo kaunse IP/domain se baat karta, kaunsi registry keys ya files banata. Malware analysis mein IOCs nikaalte hain taaki detection aur defense ban sake.

---

### Q9. C2 (Command and Control) server kya hota hai?
**Ans:** C2 server wo remote server hai jise attacker control karta hai, aur malware usse instructions leta ya chura hua data bhejta hai. Malware analysis mein hum dekhte hain malware kaunse C2 se, kaise baat karta hai — ye important IOC hota hai.

---

### Q10. Writeup likhna kyun important hai?
**Ans:** Writeup matlab kisi challenge ko kaise solve kiya, wo likhna. Isse concept pakka hota hai (revise ho jaata hai), tum apni thinking clear karte ho, aur ek portfolio banta hai jo jobs/community mein kaam aata hai. Doosron ke writeups padhna bhi nayi tricks sikhata hai.

---

### Q11. Malware analysis se pehle basics solid hona kyun zaroori hai?
**Ans:** Malware often obfuscated, packed, aur anti-analysis tricks se bhara hota hai. Agar assembly, static/dynamic analysis, aur tools ki solid samajh na ho, toh na sirf analysis mushkil hoga, balki galti se malware execute karke apne system ko khatre mein daal sakte ho. Isliye pehle crackmes/CTF pe practice karke basics pakke karte hain.

---

### Q12. RE mein consistency intensity se zyada important kyun hai?
**Ans:** RE ek skill hai jo dheere-dheere, regular practice se banti hai. Ek din 10 ghante karke fir mahine bhar chhodne se kam faayda hota hai, bajaye rozana thoda-thoda karne ke. Regular practice se patterns dimaag mein baith jaate hain aur skill permanent ho jaati hai.
