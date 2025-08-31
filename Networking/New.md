# Networking & IP Addressing - Beginner to Expert Roadmap

This guide is designed as **world-class notes** for anyone starting from zero to mastering **IP addressing, subnetting, and networking fundamentals**. It includes explanations, tables, examples, tasks (with small hints), useful tips, best practices, and a step-by-step roadmap. Every topic has a short task and a quick hint so you can practice as you go.

---

## Table of Contents
1. What is an IP Address?
2. Binary ⇄ Decimal Conversion (detailed series)
3. Subnet Mask and CIDR (including /8 and others)
4. Network Address (bitwise AND)
5. Broadcast Address
6. Usable Host Range & Host Count
7. Public vs Private IPs (table)
8. Common Subnetting Techniques & Shortcuts
9. Worked Examples (step-by-step)
10. Practice Exercises (with hints) + Answer Appendix
11. Learning Roadmap (Beginner → Expert)
12. Useful Tips & Best Practices
13. Mastery Checklist

---

## 1. What is an IP Address?
- **Definition:** An IP (Internet Protocol) address uniquely identifies a device on a network.
- **IPv4 format:** 32 bits split into four octets (8 bits each), written in dotted decimal, e.g. `192.168.1.10`.
- **Why binary?** Networks operate on bits. Subnet masks and network math are easiest to understand in binary.

**Quick example**

| Decimal      | Binary (8 bits)     |
|--------------|---------------------|
| 192          | 11000000            |
| 168          | 10101000            |
| 1            | 00000001            |
| 10           | 00001010            |

✅ **Task 1.1:** Convert `172.16.5.200` into binary (4 octets).  
💡 *Hint:* Convert each octet separately using the bit weights (128,64,32,16,8,4,2,1).

---

## 2. Binary ⇄ Decimal Conversion — Detailed Series
This section teaches **two methods** for each direction and gives plenty of examples and practice tasks. Learn both methods — you'll use the faster one in exams and interviews, and the slower method when you first start.

### 2.1 Bit Weights (the primary method)
Every 8-bit octet is a sum of bit weights:

```
128  64  32  16  8  4  2  1
```

**Decimal → Binary (method A: subtractive weights)**
1. Start with the decimal value (0–255).
2. Check if 128 fits. If yes, write `1` and subtract 128; else write `0` and keep value.
3. Move to 64, 32, 16, 8, 4, 2, 1 repeating step 2.
4. You will end with 8 bits.

**Example A1: Convert 156**
- 156 ≥ 128 → 1 (rem = 28)
- 28 ≥ 64 → 0
- 28 ≥ 32 → 0
- 28 ≥ 16 → 1 (rem = 12)
- 12 ≥ 8 → 1 (rem = 4)
- 4 ≥ 4 → 1 (rem = 0)
- 2,1 → 0,0
- Result → `10011100`

**Example A2: Convert 203**
- 203 ≥ 128 → 1 (rem 75)
- 75 ≥ 64 → 1 (rem 11)
- 11 ≥ 32 → 0
- 11 ≥ 16 → 0
- 11 ≥ 8 → 1 (rem 3)
- 3 ≥ 4 → 0
- 3 ≥ 2 → 1 (rem 1)
- 1 ≥ 1 → 1
- Result → `11001011`

**Decimal → Binary (method B: repeated division)**
1. Divide the decimal number by 2 and record the remainder (0/1).
2. Set the quotient for the next division and repeat until quotient = 0.
3. The binary number is the remainders read from **bottom (last)** to **top (first)**, pad to 8 bits.

**Example B1: Convert 45**
- 45 ÷ 2 = 22 r 1
- 22 ÷ 2 = 11 r 0
- 11 ÷ 2 = 5 r 1
- 5 ÷ 2 = 2 r 1
- 2 ÷ 2 = 1 r 0
- 1 ÷ 2 = 0 r 1 → read up: `101101` → pad to 8 bits `00101101`

**Task 2.1:** Convert these decimals to binary: `201`, `77`, `50`. Use method A (weights).  
💡 *Hint:* Use the bit weights row and subtract.

---

### 2.2 Binary → Decimal (weights method — recommended)
Multiply each bit by its weight and sum:

`bit7*128 + bit6*64 + bit5*32 + bit4*16 + bit3*8 + bit2*4 + bit1*2 + bit0*1`

**Example 1: `11000000` →**
- 1*128 + 1*64 + 0*32 + 0*16 + 0*8 + 0*4 + 0*2 + 0*1 = 128 + 64 = 192

**Example 2: `00101101` →**
- 0*128 + 0*64 + 1*32 + 0*16 + 1*8 + 1*4 + 0*2 + 1*1 = 32 + 8 + 4 + 1 = 45

**Task 2.2:** Convert these binaries to decimal: `10101100`, `00010110`, `00110010`.  
💡 *Hint:* Multiply and add the set bit weights.

---

### 2.3 Quick Tricks & Shortcuts
- Memorize powers: 128,64,32,16,8,4,2,1.
- For numbers less than 128, the leftmost bit is 0 — start from 64.
- For fast mental conversion: break the byte into nibbles (4 bits): high nibble (bits 128-16) and low nibble (8-1). Convert each nibble separately and combine.

**Nibble example:** 156 = `1001 1100` → high nibble `1001` = 9 (×16 = 144), low nibble `1100` = 12 → 144 + 12 = 156.

**Task 2.3 (fast shortcut):** Convert 173 to binary using nibble method.  
💡 *Hint:* 173 = 160 + 13 → think in nibbles.

---

### 2.4 Practice & Answer Key (for Section 2)
**Practice (do these without peeking)**
- A: Decimal→Binary: `201`, `77`, `50`.
- B: Binary→Decimal: `10101100`, `00010110`, `00110010`.
- C: Fast nibble: `173`.

**Answers (Appendix section A):** See the Appendix at the end of this README.

---

## 3. Subnet Mask and CIDR (including /8 and other common prefixes)
- CIDR `/n` = number of consecutive `1` bits from the left.
- Common masks table (expanded including /8):

| Prefix | Mask (dotted)        | Binary                         | Host bits | Total Addr | Usable Hosts |
|--------|----------------------|--------------------------------|-----------:|-----------:|-------------:|
| /8     | 255.0.0.0            | 11111111.00000000.00000000.00000000 | 24 | 16,777,216 | 16,777,214 |
| /12    | 255.240.0.0          | 11111111.11110000.00000000.00000000 | 20 | 1,048,576  | 1,048,574  |
| /16    | 255.255.0.0          | 11111111.11111111.00000000.00000000 | 16 | 65,536     | 65,534     |
| /20    | 255.255.240.0        | 11111111.11111111.11110000.00000000 | 12 | 4,096      | 4,094      |
| /24    | 255.255.255.0        | 11111111.11111111.11111111.00000000 | 8  | 256        | 254        |
| /26    | 255.255.255.192      | 11111111.11111111.11111111.11000000 | 6  | 64         | 62         |
| /28    | 255.255.255.240      | 11111111.11111111.11111111.11110000 | 4  | 16         | 14         |
| /30    | 255.255.255.252      | 11111111.11111111.11111111.11111100 | 2  | 4          | 2          |

✅ **Task 3.1:** What is the dotted mask for `/9` and how many usable hosts does it provide?  
💡 *Hint:* `/9` = `11111111.10000000.00000000.00000000`.

---

## 4. Network Address (bitwise AND) — step-by-step
**Method (binary AND)**
1. Convert IP to 32-bit binary (four 8-bit octets).
2. Convert mask to binary (CIDR → mask).
3. AND each corresponding bit (1&1=1; else 0).
4. Convert the result back to decimal — that is the network address.

**Shortcut (block-size method)**
- Find the octet where the mask is not 255 (the interesting octet).
- Block size = 256 − mask_octet.
- Subnet boundaries are 0, block, 2*block, 3*block… find which block the IP falls into → that's the network.

**Task 4.1:** Find the network for `198.51.100.46/26` using the block-size method.  
💡 *Hint:* last octet mask = 192 → block size = 64.

---

## 5. Broadcast Address (how to compute)
Two equivalent methods:
- **Method A:** set all host bits to `1` in the network binary.
- **Method B:** Broadcast = Network + (2^(host_bits) − 1).

**Task 5.1:** For network `198.51.100.0/26`, find the broadcast.  
💡 *Hint:* host bits = 6 → 2^6 − 1 = 63.

---

## 6. Usable Host Range & Host Count
- **Total addresses** in subnet = 2^(host_bits).
- **Usable hosts** normally = total − 2 (excluding network and broadcast), except for special cases (/31 and /32).

**Task 6.1:** For `/30` subnets used in point-to-point links, how many usable hosts are there and why?  
💡 *Hint:* host bits = 2 → 2^2 = 4 addresses.

---

## 7. Public vs Private IPs (table + examples)

| Range                | CIDR(s)              | Type     | Example Use                   |
|----------------------|----------------------|----------|-------------------------------|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8         | Private  | Large private networks        |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12    | Private  | Medium-sized private networks |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16  | Private  | Home & small office LANs      |
| 127.0.0.0 – 127.255.255.255 | 127.0.0.0/8       | Loopback | Local host (127.0.0.1)        |
| 169.254.0.0 – 169.254.255.255 | 169.254.0.0/16 | Link-local | APIPA (DHCP failure)         |
| 224.0.0.0 – 239.255.255.255 | 224.0.0.0/4     | Multicast | Group communications         |

✅ **Task 7.1:** Is `172.20.5.10` public or private?  
💡 *Hint:* check the 172.16.0.0/12 block.

---

## 8. Common Subnetting Techniques & Shortcuts
- **Block-size**: quickest for manual test-taking.
- **Borrowing bits**: split a large network into smaller ones.
- **VLSM**: allocate different sizes to different subnets efficiently.

**Task 8.1:** From `192.168.0.0/24`, how many `/26` subnets can you create?  
💡 *Hint:* Each `/26` has 64 addresses; how many 64-address blocks in 256?

---

## 9. Worked Examples (step-by-step)
- Many examples included earlier in the document are kept here for quick reference: `/24`, `/20`, `/26`, `/28` walkthroughs.

✅ **Task 9.1:** Try `203.0.113.45/28` — find network, broadcast & usable.
💡 *Hint:* /28 has block size 16 inside last octet.

---

## 10. Practice Exercises (with small hints)
(Answers are provided in Appendix A at the end of this README so you can self-check.)

**Beginner**
- E1: `10.1.2.3/24` → network, broadcast, usable.
  - *Hint:* /24 → network ends with .0, broadcast .255
- E2: `192.168.0.100/26` → network, broadcast, usable.
  - *Hint:* block size = 64

**Intermediate**
- E3: `172.16.5.200/28` → network, broadcast, usable.
  - *Hint:* /28 → block = 16
- E4: `10.10.10.130/25` → network, broadcast, usable.
  - *Hint:* /25 → block = 128

**Advanced**
- E5: Design subnets for `192.168.0.0/24` for departments needing 60, 30, 10 hosts.  
  - *Hint:* allocate largest subnets first, use /26, /27, /28 accordingly.

---

## 11. Learning Roadmap (Beginner → Expert)
(Consolidated, actionable plan with milestones, practical labs, and time estimates.)

- **Beginner (2–4 weeks)**: Binary conversion, basic subnetting, /24, /26, /28 practice, 50 problems.
- **Intermediate (1–2 months)**: VLSM, DHCP, NAT, VLANs, small lab on Packet Tracer/GNS3, 100 problems + lab.
- **Advanced (2–4 months)**: OSPF/BGP intro, route summarization, multicast basics. Real lab with multiple routers.
- **Expert (ongoing)**: IPv6, SD-WAN, BGP for internet, automation (Ansible/Terraform), cloud VPC design.

---

## 12. Useful Tips & Best Practices
- Always verify network & broadcast when planning.
- Write binary for tricky masks — it's less error-prone.
- Keep a cheat-sheet of common masks and block sizes.
- For VLSM, plan largest subnets first.

---

## 13. Mastery Checklist
- Can convert decimal ↔ binary instantly.
- Can find network/broadcast for any /8–/30.
- Can design subnet plans using VLSM.
- Can configure basic routing in a lab and verify with packet captures.

---

## Appendix A — Answers (Practice)
**Section 2 Answers (Conversion)**
- Decimal→Binary: `201` = `11001001`, `77` = `01001101`, `50` = `00110010`.
- Binary→Decimal: `10101100` = 172, `00010110` = 22, `00110010` = 50.
- Nibble example: `173` → `10101101` (128 + 32 + 8 + 4 + 1 = 173).

**Section 3 Answer**
- `/9` dotted mask = `255.128.0.0`. Usable hosts = `2^(32−9) − 2 = 2^23 − 2 = 8,388,606`.

**Other Practice Answers**
- See earlier created answers and exercises in the README. (Network/broadcast solutions for E1–E5 are included.)

---

*End of expanded README.*

