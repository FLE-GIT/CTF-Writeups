# Enchant
**Category:** Misc  
**Points:** Dynamic  
**CTF:** ScriptCTF 2025  
**Author:** ChickenJockey64

---

## 🧠 Challenge Description

> I was playing minecraft, and found this strange enchantment on the enchantment table. Can you figure out what it is? Wrap the flag in scriptCTF{}

---

## 🗂️ Files Provided

- `enc.txt`

---

## 🧠 Solution Strategy

We're given a file `enc.txt` that seems to be encrypted with enchantment table language (or also known as Standard Galactic Alphabet):

```
ᒲ╎リᒷᓵ∷ᔑ⎓ℸ ̣ ╎ᓭ⎓⚍リ
```

![](Enchant%20-%20image.png)

By doing some quick google searches, we can find dCode's enchantment table decoder and we can paste the input in giving us 'MINECRAFTISFUN'. 

![](Enchant%20-%20image-1.png)

We then wrap this in 'scriptCTF{}' and our flag is:

```bash
scriptCTF{MINECRAFTISFUN}
```

