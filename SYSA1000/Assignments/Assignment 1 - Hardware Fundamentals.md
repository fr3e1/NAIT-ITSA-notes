---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 1 - Hardware and OS Fundamentals"
type: assignment
tags: [SYSA1000, assignment, hardware, number-systems]
---
# Assignment 1 - Hardware Fundamentals
Back to [[SYSA1000]]

> [!NOTE]
> **Objective**
> Identify the internal components of a desktop, find out what's inside your own laptop and **who actually made each part**, and convert hardware values between decimal, binary and hex.

## Part 1 - Hardware Passport (in-person, partner signs off)
1. **Record the baseline from firmware** (written, not photographed): processor model, memory amount, storage device
2. Shut down, **unplug**, **touch bare metal** (ESD), open the case
3. Record model/part number, type, size/speed for **RAM, storage, motherboard, PSU**
	* Don't remove the **CPU cooler** - that's why the CPU model comes from firmware
4. **Remove + refit one RAM module** (clips at both ends) → boot → must pass **POST** (no beeps/errors) → firmware still shows the same memory
5. **Remove + refit the storage drive** (if reachable) → POST → firmware still detects it
6. Restore firmware settings/defaults, close the case, leave the station tidy

> [!TIP]
> **Why record a baseline first?**
> The "still reports the same thing" check only means something if you wrote down what it said before you touched anything.

## Part 2 - Laptop hardware identification
* **The brand on the lid didn't make the parts** - an Asus laptop might have an Intel CPU, Samsung RAM and a WD drive. Knowing the part maker is how you find drivers, specs and replacements

| Component | Where to look |
| --- | --- |
| Processor | **CPU-Z → CPU** tab |
| Memory | **CPU-Z → SPD** tab (per slot: manufacturer + part number; the *Memory* tab only shows totals) |
| Graphics | **CPU-Z → Graphics** tab |
| Storage | **Device Manager → Disk drives** (CPU-Z doesn't read drives) |

* Link to the **manufacturer's own page** for each part (not shops/reviews)
* Integrated graphics (Intel UHD / AMD Radeon Graphics) is a valid answer - say it's built into the CPU
* Blank SPD manufacturer (soldered RAM)? Record what it shows and say so - **an honest unknown is worth full marks, a guess isn't**
* Screenshots: CPU-Z CPU tab, CPU-Z SPD tab, Device Manager with Disk drives expanded

## Part 3 - Number systems
* Memory comes in **powers of two** (8, 16, 32, 64 GB) because it's addressed in binary - each extra binary digit doubles the value
* 1 KB = 1024 B, 1 MB = 1024 KB, 1 GB = 1024 MB → 32 GB = 34,359,738,368 bytes
* **Show the long-hand working** - the answer alone earns nothing

### Methods (worked on different values)
**Decimal → binary** (repeated division by 2, read remainders bottom-up)
```
45 ÷ 2 = 22 r1
22 ÷ 2 = 11 r0
11 ÷ 2 =  5 r1
 5 ÷ 2 =  2 r1
 2 ÷ 2 =  1 r0
 1 ÷ 2 =  0 r1      → 101101
```
**Decimal → hex** (repeated division by 16)
```
45 ÷ 16 = 2 r13 (D)
 2 ÷ 16 = 0 r2      → 2D
```
**Binary → decimal** (sum place values)
```
1 0 1 1 0 1
32+0+8+4+0+1 = 45
```
**Binary ↔ hex** (group into nibbles of 4 from the right)
```
0010 1101  →  2 D  →  0x2D
```
**Hex → decimal**
```
2D = 2×16 + 13×1 = 45
B7 = 11×16 + 7 = 183
```

| Place | 2^7 | 2^6 | 2^5 | 2^4 | 2^3 | 2^2 | 2^1 | 2^0 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Value | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

| Hex | A | B | C | D | E | F |
| --- | --- | --- | --- | --- | --- | --- |
| Dec | 10 | 11 | 12 | 13 | 14 | 15 |

> [!NOTE]
> Powers of two in binary are a single 1 followed by zeros (16 = `10000`). `F` = `1111` (all four bits on), so `FF` = 255 = a full byte.

Related: [[Hardware Identification and Handling]], [[HW and OS Fundamentals]]
