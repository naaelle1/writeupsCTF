# Ph4nt0m 1ntrud3r

## Platform
picoCTF

## Category
Forensics / Network Forensics

## Difficulty
Easy

---

## Challenge Description

We were given a PCAP file named `myNetworkTraffic.pcap` and asked to find the flag.

Hints:
- Filter your packets to narrow down your search
- Attacks were done in a timely manner
- Time is essential

---

## Initial Analysis

I started by attempting to inspect the PCAP file using `tshark`:

```bash
tshark -r myNetworkTraffic.pcap
````

However, I encountered a permission error:

```text id="err1"
tshark: You don't have permission to read the file "myNetworkTraffic.pcap".
```

Even with elevated privileges:

```bash
sudo tshark -r myNetworkTraffic.pcap
```

the issue persisted.

To confirm the file integrity, I checked its metadata:

```bash id="chk1"
ls -lah
file myNetworkTraffic.pcap
```

Output:

```text id="chk2"
-rw-rw-r-- 1 elok elok 1452 Jun 12 22:37 myNetworkTraffic.pcap

pcap capture file, microsecond ts (little-endian)
```

The file was valid and relatively small (1452 bytes), indicating a limited number of packets.

---

## Switching Tools

Since `tshark` was problematic, I switched to `tcpdump`:

```bash id="tcp1"
tcpdump -nn -r myNetworkTraffic.pcap
```

Output showed timestamps and packet summaries:

```text id="tcp2"
reading from file myNetworkTraffic.pcap...
10:31:49.252452 ...
10:31:49.254895 ...
10:31:49.251729 ...
```

Observations:

* Only ~22 packets
* All packets occurred within the same second
* Timestamps were not in chronological order

This directly aligned with the hint:

> Time is essential

So the packet order was likely meaningful.

---

## Inspecting Packet Payloads

To extract raw packet data, I used:

```bash id="pay1"
tcpdump -XX -nn -r myNetworkTraffic.pcap
```

Inside the hex output, I found encoded byte sequences such as:

```text id="hex1"
42 4e 41 55 64 36 55 3d
```

When converted to ASCII, this became:

```
BNAUd6U=
```

This confirmed that Base64 data was embedded in packet payloads.

---

## Extracting and Ordering Data

I extracted all Base64-like strings from the packets and sorted them by timestamp to reconstruct the correct sequence.

After ordering, I obtained:

```text id="b64list"
cGljb0NURg==
ezF0X3c0cw==
bnRfdGg0dA==
XzM0c3lfdA==
YmhfNHJfZQ==
NWU4Yzc4ZA==
fQ==
```

---

## Decoding Base64

Each segment was decoded individually:

| Base64       | Decoded |
| ------------ | ------- |
| cGljb0NURg== | picoCTF |
| ezF0X3c0cw== | {1t_w4s |
| bnRfdGg0dA== | nt_th4t |
| XzM0c3lfdA== | _34sy_t |
| YmhfNHJfZQ== | bh_4r_e |
| NWU4Yzc4ZA== | 5e8c78d |
| fQ==         | }       |

---

## Flag

```text id="flag1"
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_e5e8c78d}
```

---

## Conclusion

The main challenge was not decoding the data itself, but reconstructing the correct order of packets.

Once the timestamps were used to reorder the Base64 fragments, decoding them revealed the full flag.

This challenge demonstrates how network forensics can hide data through timing and fragmentation rather than encryption alone.

---

## Key Takeaways

* PCAP files may require multiple tools (`tcpdump`, `tshark`)
* Permission issues can sometimes be environment-related, not file-related
* Packet timing/order can be critical in reconstruction challenges
* Base64 is commonly used to hide structured payload fragments
* Always consider both content and temporal structure in network forensics

```
```
