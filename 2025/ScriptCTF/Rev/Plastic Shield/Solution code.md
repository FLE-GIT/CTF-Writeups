```python
#!/usr/bin/env python3
import subprocess

BIN = "./plastic-shield"
L = 10
HOT = int(0.6 * L)  # 6 when L=10

WHITESPACE = {0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x20}  # \t \n \v \f \r space

def run_with_byte(b: int) -> bytes:
    buf = bytearray(b"A" * L)
    buf[HOT] = b
    buf += b"\n"
    res = subprocess.run([BIN], input=bytes(buf), stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    return res.stdout

def is_printable(pt: bytes) -> float:
    return sum((32 <= x <= 126) or x in (9,10,13) for x in pt) / max(1, len(pt))

def extract_plain(out: bytes) -> bytes:
    tag = b"Decrypted text:"
    i = out.find(tag)
    return out[i+len(tag):] if i != -1 else out

best = None
for b in range(1, 256):              # exclude NUL
    if b in WHITESPACE:              # avoid truncating the %s read
        continue
    out = run_with_byte(b)
    pt = extract_plain(out)
    ratio = is_printable(pt)
    if best is None or ratio > best[0]:
        best = (ratio, b, out)

ratio, b, out = best
print(f"Top candidate: 0x{b:02x} ({chr(b) if 32 <= b <= 126 else 'ctrl'}) ratio={ratio:.2f}")
print(out.decode('utf-8', errors='replace'))

```