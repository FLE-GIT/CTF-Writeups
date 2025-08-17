# Secure-Server
**Category:** Crypto  
**Points:** Dynamic  
**CTF:** ScriptCTF 2025  
**Author:** ChickenJockey64

---

## 🧠 Challenge Description

> John Doe uses this secure server where plaintext is never shared. Our Forensics Analyst was able to capture this traffic and the source code for the server. Can you recover John Doe's secrets?

---

## 🗂️ Files Provided

- `files.zip`
	- `capture.pcap`
	- `server.py`

---

## 🧠 Solution Strategy

We're given two files inside the zip folder. A `capture.pcap` file and a `server.py` file. By inspecting the `server.py` file, we see how a server takes input and encrypts it by XORing it 

Let's now inspect the `capture.pcap` file and see if we can find any interesting in Wireshark. By following the TCP streaam in wireshark we get the following conversation: 

![[image.png]]

So it seems the user has given XORed their secret twice with the key 
:



```bash
scriptCTF{1_l0v3_m461c_7r1ck5}
```

