# Sums
**Category:** Programming  
**Points:** Dynamic  
**CTF:** ScriptCTF 2025  
**Author:** ChickenJockey64

---

## 🧠 Challenge Description

> Find the sum of nums[i] for i in [l, r] (if there are any issues with input/output format, plz open a ticket)

---

## 🗂️ Files Provided

- `server.py`

---

## 🧠 Solution Strategy

We're given a `server.py` and an instance to connect to a Netcat connection. The first step is to understand the code fully:

1. The server sends one huge line of $n=123456$ numbers between 0 and 696969.
2. After it sends $n$ inclusive ranges `[l, r]`, where `l` is a random number between 0 and $n-2$ and `r` is between `l` and $n-1$. 
3. Our task is then to find the sum of all the `nums[i]` printed, where `nums[i]` is between the corresponding `l` and `r`.
4. Important to note that there is 10 second timeout, so we have to do our solution in linear time, and if we must get the correct answer (duh)

Then by explaining this to ChatGPT we get the following code that dirrectly hooks up to Netcat connection and prints out whatever the flag sends. The code works by 


`enc.txt` that seems to be encrypted with enchantment table language (or also known as Standard Galactic Alphabet):

```
ᒲ╎リᒷᓵ∷ᔑ⎓ℸ ̣ ╎ᓭ⎓⚍リ
```

We then wrap this in 'scriptCTF{}' and our flag is:

```bash
scriptCTF{MINECRAFTISFUN}
```

