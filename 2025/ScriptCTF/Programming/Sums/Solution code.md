```Python
# solve_stream.py
import socket, sys

HOST, PORT = "play.scriptsorcerers.xyz", 10441

def main():
    s = socket.create_connection((HOST, PORT))
    f_r = s.makefile('rb', buffering=0)   # read bytes, line by line
    # Write directly on the socket to avoid extra buffering
    send = s.sendall

    # 1) nums line
    nums = list(map(int, f_r.readline().split()))
    n = len(nums)

    # 2) prefix sums
    ps = [0]*(n+1)
    acc = 0
    for i, v in enumerate(nums, 1):
        acc += v
        ps[i] = acc

    # 3) STREAM answers: read a range, immediately send its sum
    for _ in range(n):
        l, r = map(int, f_r.readline().split())
        ans = ps[r+1] - ps[l]
        send(f"{ans}\n".encode())  # immediate write, no extra buffering

    # 4) read whatever the server sends next (flag or error)
    # If they close quickly, this returns b""
    resp = f_r.read()
    if resp:
        sys.stdout.write(resp.decode(errors="ignore"))

if __name__ == "__main__":
    main()

```