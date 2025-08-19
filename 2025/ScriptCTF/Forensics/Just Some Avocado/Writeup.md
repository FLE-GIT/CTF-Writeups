# Just Some Avocado
**Category:** Forensics  
**Points:** Dynamic  
**CTF:** ScriptCTF 2025  
**Author:** ChickenJockey64

---

## 🧠 Challenge Description

> just an innocent little avocado!

---

## 🗂️ Files Provided

- `avocado.jpg`

---

## 🧠 Solution Strategy

We're given a picture of an avocado. The first thing I did was to use Exiftool and Aperisolve on it, interestingly I got this from Aperisolve: 

![[Avocado - image.png]]

Therefore, I thought it would have something to do with an embedded zip file of sorts, and therefore I ran Binwalk to get the embedded folder:

![[Avocado - image-2.png]]

Looks like it was two zip folders, however when going inside the folder I could only see a zip folder named `188F7.zip`. So I tried unzipping it but it needed a password:

![[Avocado - image-3.png]]

This I bruteforced using fcrackzip with `rockyou.txt`:

![[Avocado - image-4.png]]

After unzipping the zip folder we got another zip folder and a `wav` file:

![[Avocado - image-5.png]]

To get the needed password for the new zip folder, we should probably get it from the `staticnoise.wav` file somehow. After putting the file into `https://academo.org/demos/spectrum-analyzer/` I got the following: 

![[Avocado - image-6.png]]

However, this was unreadable so I opened the file in Audacity and setting Spectrogram on I got the following:

![[Avocado - image-8.png]]

This was also a bit unreadable but much better. However this looked similar to the common passwords created from Aperi Solve: 

![[Avocado - image-9.png]]

Therefore, I tried `d41v3ron` as the password and it worked! (I had also tried rockyou.txt again and bigger wordslists, however they didn't work also stated by using Exiftool on the `staticnoise.wav`)

![[Avocado - image-10.png]]

By reading the `flag.txt` file we get the flag: 

```bash
scriptCTF{1_l0ve_d41_v3r0n}
```

