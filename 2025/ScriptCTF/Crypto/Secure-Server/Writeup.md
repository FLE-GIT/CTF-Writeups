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

We're given two files inside the zip folder. A `capture.pcap` file and a `server.py` file. By inspecting the `server.py` file, we see how it's a server which tages the input and XOR's the secret already XORed by the user with a randomly generated key:

![](Images/SecureServer - image-2.png)

Let's now inspect the `capture.pcap` file and see if we can find any interesting in Wireshark. By following the TCP stream in wireshark we get the following conversation: 

![](Images/SecureServer - image.png)

Looking at this, we can model how the conversation went in terms of the secret. Let's call the secret `m`, the user key `u` and the randomly generated server key `k`, we get the following model from the conversation: 

1. Client $\rightarrow$ Server: $A = m \ \oplus \ u$                                       151e71ce4$\dots$ff05a2b2b0a83ba03
2. Server $\rightarrow$ Client: $B=A\ \oplus \ k =m \ \oplus \ u \ \oplus \ k$               e1930164280e4$\dots$f15d8a10d70
3. Client $\rightarrow$ Server: $C=B \ \oplus \ u=m \ \oplus \ k$                       87ee02c312a7f1$\dots$b0871ff303960e

So if we then XOR all these together (no matter the order since it's commutative) we get the original message `m`:

$$
A \ \oplus \ B \ \oplus \ C = (m \ \oplus \ u) \ \oplus \  (m \ \oplus \ u \ \oplus \ k) \ \oplus \ (m \ \oplus \ k) = m
$$

This is due to the fact that the following is true for XOR:


$$
x \ \oplus \ x = 0
$$

So by simply XORing the messages we can get our original secret message. This can be done like the following in CyberChef:

![](Images/SecureServer - image-1.png)

This gives us our final flag: 

```bash
scriptCTF{x0r_1s_not_s3cur3!!!!}
```