# Plastic Shield
**Category:** Programming  
**Points:** Dynamic  
**CTF:** ScriptCTF 2025  
**Author:** ChickenJockey64

---

## 🧠 Challenge Description

> OPSec is useless unless you do it correctly.

>Hint 1: The algorithm implementation itself is not the problem, I would look elsewhere.

---

## 🗂️ Files Provided

- `plastic shield`

---

## 🧠 Solution Strategy

First off we're given a file called `plastic shield`. I start by checking what filetype it is: 

![](Images/plastic - image.png)

We have a ELF 64-bit LSB executable file and I quickly do `chmod +x` on the file to make it executable. I try running it and we get the following:

![](Images/plastic - image-1.png)

So we have to find some password from the file. Since it's an ELF executable I open it up in GHidra and start looking at the main function:  

![](Images/plastic - image-2.png)

After looking at the code it becomes apparent that on line 27 we choose one position from the input (the 7th character). This is then used to feed the hash and the rest of the input doesn't change the key. So as long as our input is 10 characters long we can brute force the 7th character until we get the correct one. I asked ChatGPT to make this script for me, all though it can probably be done with much cleaner code. When running the script I got this flag:

```bash
scriptCTF{20_cau541i71e5_d3f3n5es_d0wn}
```

