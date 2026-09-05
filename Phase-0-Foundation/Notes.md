# Phase 0 — Foundation (Neev Majboot Karo)

> **Goal:** Reverse Engineering seekhne se pehle computer andar se kaise kaam karta hai — ye samajhna. Ye neev hai. Agar ye kachhi rahi, toh aage sab kuch upar se nikal jaayega. Time: **2-3 hafte**.

---

## 0.0 — Sabse Pehle: Ye Phase Kyun?

Socho tum ek doctor banna chahte ho. Kya seedha operation seekhoge? Nahi. Pehle body ke parts (anatomy) padhoge. Waise hi RE mein, pehle "computer ki anatomy" samajhni padegi — numbers kaise store hote hain, memory kaise kaam karti hai, program kaise banta hai.

Is phase mein 4 cheezein seekhenge:
1. **Number Systems** (Binary, Hex, Decimal)
2. **Memory & Data** (RAM, bits, bytes, endianness)
3. **C Programming basics** (kyunki zyada tar programs C mein bane hote hain)
4. **Compilation** (source code se `.exe` kaise banta hai)

---

## 0.1 — Number Systems (Binary, Hex, Decimal)

### Computer sirf 0 aur 1 samajhta hai

Humari duniya **decimal** (0-9, 10 digits) use karti hai. Lekin computer ke andar sirf electricity hai — ya toh current hai (**1**) ya nahi hai (**0**). Isi ko **Binary** kehte hain.

- **Binary (Base 2):** sirf `0` aur `1`
- **Decimal (Base 10):** `0` se `9` (hum log)
- **Hexadecimal / Hex (Base 16):** `0-9` phir `A,B,C,D,E,F`

### Hex kyun? (RE ka favourite)

Binary bahut lambi hoti hai. `11111111` likhna mushkil. Isliye RE mein hum **Hex** use karte hain — chhoti aur aasan. Har hex digit = 4 binary digits.

```
Binary:   1111 1111
Hex:       F    F     =  0xFF
```

> `0x` prefix ka matlab hai "ye hex number hai". Jaise `0x41`, `0xFF`. RE tools mein sab jagah `0x` dikhega.

### Conversion Table (yaad rakhne layak)

| Decimal | Binary | Hex |
|---------|--------|-----|
| 0  | 0000 | 0x0 |
| 5  | 0101 | 0x5 |
| 10 | 1010 | 0xA |
| 15 | 1111 | 0xF |
| 16 | 10000 | 0x10 |
| 65 | 1000001 | 0x41 |
| 255 | 11111111 | 0xFF |

### Real Example — Letter 'A'

Computer mein har letter ek number hota hai (ASCII code):

```
Letter 'A'  =  Decimal 65  =  Binary 01000001  =  Hex 0x41
Letter 'B'  =  Decimal 66  =  Hex 0x42
Letter 'a'  =  Decimal 97  =  Hex 0x61
```

> RE karte waqt jab tumhe memory mein `41 42 43` dikhega, tum turant samajh jaoge ki wo `"ABC"` hai. Ye skill baar-baar kaam aayegi!

### Practice

- Decimal 200 ko hex mein convert karo (answer: `0xC8`)
- Hex `0x2A` ko decimal mein (answer: `42`)
- Windows calculator ko "Programmer mode" mein rakh ke practice karo — ye instant convert karta hai.

---

## 0.2 — Memory, Bits & Bytes

### Bits aur Bytes

- **Bit** = ek `0` ya `1` (sabse chhoti unit)
- **Byte** = 8 bits (`11111111`) — ek byte se 0 se 255 tak number ban sakta hai
- Aage: 1 KB = 1024 bytes, 1 MB = 1024 KB...

```
1 byte = 8 bits = 2 hex digits
Example: 0xFF = 11111111 = 255 (ek byte ki max value)
```

### Memory (RAM) — Ghar ke address jaisa

RAM ko ek badi building socho jismein crore-o flats hain. Har flat ka ek **address** hai. Computer har data ko kisi address pe rakhta hai.

```
Address      Value (byte)
0x1000  →    0x41  ('A')
0x1001  →    0x42  ('B')
0x1002  →    0x43  ('C')
```

> RE mein hum baar-baar "is address pe kya value hai?" ye dekhte hain. Debugger memory ko exactly aise hi dikhata hai.

### Endianness (⭐ Ye confuse karta hai — dhyan se)

Jab ek number ek byte se bada ho (jaise 4-byte number), toh usko memory mein kis order mein rakhein? Do tarike hain:

- **Little Endian:** chhota (least significant) byte pehle. **Intel/AMD (x86/x64) yahi use karte hain.**
- **Big Endian:** bada byte pehle (network protocols mein common).

**Example:** Number `0x12345678` ko memory mein store karna:

```
Little Endian (Intel PC):   78 56 34 12   <- ULTA! bytes reversed
Big Endian:                 12 34 56 78   <- seedha
```

> Beginners yahin confuse hote hain. Jab debugger mein `78 56 34 12` dikhe, actual number `0x12345678` hai. Bas byte order ulta hai. Ye yaad rakho.

---

## 0.3 — C Programming Basics (RE Ki Jaan)

Zyada tar software (Windows, games, malware) **C/C++** mein bane hote hain. Agar tum C samajhte ho, toh decompiled code (Ghidra ka output) padhna aasan ho jaayega — kyunki wo C jaisa hi dikhta hai.

### Ek simple C program

```c
#include <stdio.h>          // input/output ke functions laane ke liye (printf)

int main() {                // main = program yahi se shuru hota hai
    int age = 25;           // 'age' naam ka box banaya, usme 25 rakha
    printf("Age: %d", age); // screen pe "Age: 25" chhaapo (%d = number)
    return 0;               // program theek se khatam (0 = success)
}
```

**Line by line samjho:**
- `#include <stdio.h>` — ye "library" laata hai jisme `printf` jaisa function hota hai. Isse pehle likhna padta hai.
- `int main()` — har C program `main` se chalu hota hai. `int` matlab ye ek number return karega.
- `int age = 25;` — `int` = integer (poora number). `age` variable ka naam. `= 25` uski value.
- `printf("Age: %d", age);` — screen pe print. `%d` ki jagah `age` ki value (25) aa jaayegi.
- `return 0;` — program ko batana ki sab theek raha (0 = no error).

### Zaroori Concepts

**1. Variables & Data Types**
```c
int   count = 10;      // poora number (integer)
char  letter = 'A';    // ek character (asal mein number 65)
float price = 9.99;    // decimal number
```

**2. Pointers (⭐⭐ Sabse important, RE ka dil)**

Pointer ek variable hai jo kisi doosre variable ka **address** rakhta hai (value nahi, ghar ka pata).

```c
int   x = 5;      // x mein 5 hai (maano address 0x1000 pe)
int  *p = &x;     // p mein x ka ADDRESS hai (0x1000), value nahi
printf("%d", *p); // *p matlab "us address pe jo value hai" = 5
```

**Line by line:**
- `int x = 5;` — normal variable, value 5.
- `int *p = &x;` — `*` batata hai ye pointer hai. `&x` matlab "x ka address". Toh `p` ke andar x ka address hai.
- `*p` — `*` yahan "dereference" hai = "is address pe jaake value nikaalo" = 5.

> Pointers samajh gaye toh RE ka 40% clear. Kyunki assembly mein sab kuch addresses se hota hai. `&` = address lena, `*` = us address pe jaana.

**3. Functions, Loops, If-else**
```c
int add(int a, int b) {   // 'add' function, do numbers leta hai
    return a + b;         // dono ko jodke wapas bhejta hai
}

for (int i = 0; i < 5; i++) {   // loop: i=0 se 4 tak, 5 baar chalega
    printf("%d ", i);           // print: 0 1 2 3 4
}

if (age >= 18) {                // condition check
    printf("Adult");
} else {
    printf("Minor");
}
```

**4. Arrays & Strings**
```c
int  nums[3] = {10, 20, 30};    // 3 numbers ka array
char name[] = "Amit";           // string = characters ka array (A,m,i,t,\0)
```
> String ke end mein ek chhupa `\0` (null byte) hota hai jo batata hai "string yahan khatam". RE mein ye dikhega.

### Practice
- Ek `hello.c` likho, compile karo, chalao.
- Pointer wala program likho aur samjho `&` aur `*` kaise kaam karte hain.

---

## 0.4 — Compilation: Source Code Se .exe Tak

Ye samajhna **bahut zaroori** hai, kyunki RE isi process ka **ulta** hai.

### Forward Process (normal programming)

```
  hello.c          →   Compiler   →   hello.exe
(hum likhte hain)      (gcc/msvc)     (computer chalata hai)

Detail mein:
Source Code (.c)
   ↓ (Preprocessor — #include waale kaam)
Expanded Code
   ↓ (Compiler)
Assembly Code (.asm)
   ↓ (Assembler)
Machine Code / Object (.obj)
   ↓ (Linker — libraries jodta hai)
Executable (.exe)
```

### Reverse Process (RE — hum ye karte hain)

```
hello.exe   →   Disassembler   →   Assembly   →   (hum samajhte hain logic)
                (Ghidra/IDA)                  →   Decompiler → C-jaisa code
```

> Yaad rakho: compile karte waqt bahut si information kho jaati hai — variable ke naam (`age`, `price`), comments, sab gayab. Isliye RE mushkil hai — hume ye khoyi hui info wapas guess karni padti hai.

### Khud Karke Dekho (Best Practice)

`gcc -S` command source ko assembly mein badalti hai (compile nahi karti, sirf assembly dikhati hai):

```bash
gcc -S hello.c -o hello.s     # hello.s mein assembly milegi
gcc hello.c -o hello.exe      # normal compile
```

**Sabse best tool: [godbolt.org](https://godbolt.org)**
- Left side mein C likho, right side mein turant assembly dikhegi.
- Ye Phase 1 (Assembly) ke liye bhi bahut kaam aayega. Abhi se use karna shuru karo.

---

## 🧪 Phase 0 — Hands-On Checklist

- [ ] Windows Calculator "Programmer mode" mein hex ↔ decimal convert kiya
- [ ] `'A'` = 65 = 0x41 samajh liya (ASCII)
- [ ] Little vs Big endian ka farak samajh liya (`0x12345678` → `78 56 34 12`)
- [ ] Ek `hello.c` likha, compile kiya, chalaya
- [ ] Pointer wala program likha (`&x` aur `*p`)
- [ ] godbolt.org pe C likh ke assembly dekhi

---

## 🎯 Key Takeaways (Ek Line Mein)

1. Computer sirf **binary** (0/1) samajhta hai; hum **hex** (0x..) use karte hain kyunki chhoti hoti hai.
2. **1 byte = 8 bits = 2 hex digits**; ek byte 0-255 tak.
3. Har data RAM mein ek **address** pe hota hai.
4. **Little endian** = bytes ulte store hote hain (Intel PC). `0x12345678` → `78 56 34 12`.
5. **Pointer** = address rakhne wala variable. `&` = address lo, `*` = address pe jaao. (RE ka dil)
6. Compile = source → exe (info kho jaati hai). RE = exe → samajhna (info wapas guess karna).

---

> ✅ **Phase 0 Complete Jab:** Tum hex-decimal convert kar sako, ek chhota C program (pointer ke saath) likh-compile kar sako, aur memory/endianness ka clear idea ho. Ab Phase 1 (Assembly) ke liye ready ho! 🚀
