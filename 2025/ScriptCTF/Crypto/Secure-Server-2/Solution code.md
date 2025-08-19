```python
# pip install pycryptodome
from typing import Tuple, Dict, List

# Captured values from the pcap
HEX_USER = "19574ac010cc9866e733adc616065e6c019d85dd0b46e5c2190c31209fc57727"
HEX_QUADRUPLE = "0239bcea627d0ff4285a9e114b660ec0e97f65042a8ad209c35a091319541837"
HEX_REPLY = "4b3d1613610143db984be05ef6f37b31790ad420d28e562ad105c7992882ff34"

User = bytes.fromhex(HEX_USER)
Quadruple = bytes.fromhex(HEX_QUADRUPLE)
UserReply = bytes.fromhex(HEX_REPLY)

try:
    from Crypto.Cipher import AES
except Exception:
    from Cryptodome.Cipher import AES


def expand_key(two_bytes: bytes) -> bytes:
    """Turn 2 raw bytes into 16-byte AES key via ASCII '0'/'1' bits."""
    b0, b1 = two_bytes
    return f"{b0:08b}{b1:08b}".encode("ascii")


def aes_ecb(data: bytes, key: bytes, do_decrypt: bool = False) -> bytes:
    aes = AES.new(key, AES.MODE_ECB)
    return aes.decrypt(data) if do_decrypt else aes.encrypt(data)


def mitm_solver(input_block: bytes, output_block: bytes) -> Tuple[bytes, bytes]:
    """
    Given output_block = E_kB(E_kA(input_block)), recover (kA, kB).
    """
    # Forward map of middle states
    forward: Dict[bytes, List[bytes]] = {}
    for n in range(65536):
        kA_raw = bytes([n >> 8, n & 0xFF])
        mid = aes_ecb(input_block, expand_key(kA_raw), do_decrypt=False)
        forward.setdefault(mid, []).append(kA_raw)

    # Backward search
    for n in range(65536):
        kB_raw = bytes([n >> 8, n & 0xFF])
        mid2 = aes_ecb(output_block, expand_key(kB_raw), do_decrypt=True)
        if mid2 in forward:
            for kA_raw in forward[mid2]:
                test = aes_ecb(aes_ecb(input_block, expand_key(kA_raw)), expand_key(kB_raw))
                if test == output_block:
                    return kA_raw, kB_raw

    raise ValueError("Keys not found")


def main():
    # Step 1: Recover server-side keys (k3,k4) from User -> Quadruple
    k3_raw, k4_raw = mitm_solver(User, Quadruple)

    # Step 2: Recover client-side keys (k1,k2) from UserReply -> Quadruple
    k1_raw, k2_raw = mitm_solver(UserReply, Quadruple)

    # Step 3: Decrypt the original secret
    secret = aes_ecb(
        aes_ecb(User, expand_key(k2_raw), do_decrypt=True),
        expand_key(k1_raw),
        do_decrypt=True,
    )

    # Step 4: Assemble final flag
    flag_bytes = secret + k1_raw + k2_raw + k3_raw + k4_raw

    print("Secret message:", secret.decode(errors="replace"))
    try:
        print("Flag:", flag_bytes.decode("latin1"))
    except UnicodeDecodeError:
        print("Flag (hex):", flag_bytes.hex())


if __name__ == "__main__":
    main()
```