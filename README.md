# MULTI RETRO DRAM TESTER  
## USER MANUAL

---

## Firmware Update (Windows 10 / 11)

### Download & Flash Instructions

1. Download the **latest firmware (.UF2)** from the **Releases** section.
2. Use a **USB-C to USB-A or USB-C cable** (depending on your PC).
3. Connect the **Multi Retro DRAM Tester** to your PC.

### Enter Bootloader Mode

Use **either** method below:

- **Method 1**  
  Hold the **BOOT** button → Press and release **RESET**

- **Method 2**  
  Hold **BOOT** while connecting the USB cable

4. A new removable drive will appear named:

RPI-RP2

5. Drag and drop the **.UF2 firmware file** onto this drive.
6. The tester will **automatically program, reboot, and run the new firmware**.

---

## ⚠️ IMPORTANT POWER WARNING ⚠️

DO NOT connect the Multi Retro DRAM Tester to USB  
while it is powered via the 2.1mm DC jack.  
Use ONE power source only.

---

## OVERVIEW

The **Multi Retro DRAM Tester** is a comprehensive diagnostic tool for vintage **Dynamic RAM (DRAM)** chips spanning from the **1970s (4116)** through to the **1990s (514400)**.

It is powered by a **dual-core ARM Cortex-M0+ processor**, capable of generating precise DRAM timing signals to accurately verify memory integrity, reliability, and marginal behaviour in aging chips.

---

## FAST TEST TIMES

Optimised test algorithms and selectable access modes allow:

- Rapid screening of known-good chips  
- Deep stress-testing of marginal devices  
- Accurate detection of timing-related failures

## Test Times measured with firmware release V4.08b   

|   DRAM | Memory Configuration | March B | March C- | March B + True Row Retention | Retention Time |
| -----: | -------------------- | ------: | -------: | ---------------------------: | -------------: |
|   4027 | 4K × 1               |   39 ms |    57 ms |                       372 ms |           2 ms |
|   4108 | 8K × 1               |   67 ms |   101 ms |                       666 ms |           2 ms |
|   4116 | 16K × 1              |  120 ms |   191 ms |                       740 ms |           2 ms |
|   4816 | 16K × 1              |  120 ms |   191 ms |                       740 ms |           2 ms |
|   4532 | 32K × 1              |  248 ms |   402 ms |                       1.43 s |           2 ms |
|   3732 | 32K × 1              |  248 ms |   402 ms |                       1.43 s |           2 ms |
|   4164 | 64K × 1              |  443 ms |   727 ms |                        2.7 s |           4 ms |
|   4128 | 128K × 1             |  1.25 s |   2.07 s |                        3.8 s |           4 ms |
|  41256 | 256K × 1             |  1.96 s |   3.25 s |                        7.8 s |           4 ms |
|   4416 | 16K × 4              |  197 ms |   320 ms |                        2.4 s |           4 ms |
|   4464 | 64K × 4              |  750 ms |   1.25 s |                        3.7 s |           4 ms |
|  44256 | 256K × 4             |     3 s |      5 s |                       10.5 s |           4 ms |
| 511000 | 1M × 1               |   7.4 s |   12.2 s |                         30 s |           8 ms |
| 514400 | 1M × 4               |    12 s |     20 s |                         58 s |          16 ms |


---

## SOCKET SELECTION (IMPORTANT!)

The tester includes **two ZIF sockets**.  
**Always use the correct socket** to avoid damage to the chip or tester.

---

### ▶ ZIF SOCKET 1 (SK1) — STANDARD +5V

For **single-supply 5V DRAM chips**.

**16-Pin**
- HM4816 (16K×1, 5V-only 4116 variant)
- M3732L / M3732H (32Kx1)
- TMS4532-NL3 / NL4 (32K×1)
- 4164 (64K×1)
- 4128 (128K×1, piggyback)
- 41256 (256K×1)

**18-Pin**
- 4416 (16K×4)
- 4464 (64K×4)
- 411000 (1M×1)

**20-Pin**
- 44256 (256K×4)
- 514400 / 71C4400 (1M×4)

---

### ▶ ZIF SOCKET 2 (SK2) — MULTI-VOLTAGE

For **legacy DRAM requiring −5V, +5V, and +12V**.

- 4116 (16K×1)
- MK4027 (4K×1)
- TMS4108 (8K×1)

---

## MAIN DRAM TEST ALGORITHMS

Selectable algorithms that stress the internal memory cells in different ways.

---

### March B

**Type:** Industry Standard FSM  
**Complexity:** Low–Medium  

**Description**  
Writes zeros to the entire array, then performs read/write transitions.

**Best For**
- Stuck-at faults  
- Basic coupling faults  
- Faster screening than March C-  

---

### March C- (Minus)

**Type:** Industry Standard (Gold Standard)  
**Complexity:** High  

**Description**  
A bidirectional algorithm that repeatedly reads and writes inverted data patterns.

**Best For**
- Stuck-at faults  
- Transition faults  
- Coupling faults  
- Address decoder faults  

### March C- Mix

**Type:** Timing Stress Test  

**Description**  
Runs March C- twice:

1. Standard Page Mode  
2. Fast Page Mode (RAS held low, CAS toggled rapidly)

**Best For**
- Detecting chips that fail only under high-speed access  

---

### Checkerboard

**Type:** Leakage / Weak-Bit Test  

**Description**  
Alternating bit patterns are written, delayed, and verified.

**Best For**
- Detecting weak or aging memory cells  

---

### Row Retention

**Type:** True Row-by-Row Retention  

**Description**  
Each row is charged, delayed for an exact time, and verified independently.

**Best For**
- Identifying marginal or weak retention cells  
- Accurate retention profiling  

---

### Extreme Mode

**Type:** Composite Test Suite  

**Description**  
User-configurable combination of multiple algorithms for maximum stress testing.

---

## CONFIGURATION OPTIONS

- **Loops**
  - `0` = Infinite
  - Any other value = Fixed count

- **Power Mode**
  - **ON** — Power remains applied after a pass
  - **CYCLE** — Power cycles between loops

- **On Error**
  - **STOP** — Halt immediately
  - **RESTART** — Log error and continue testing

---

## PRE-TESTS (SAFETY CHECKS)

⚠️ **Strongly recommended to keep ALL enabled**  
These exist primarily for developer diagnostics.

---

### Pin Check

Detects shorts, stuck pins, and illegal connections **before power is applied**.

---

### Wake-Up Refresh

Applies RAS-only refresh pulses to stabilise internal charge pumps.

---

### Presence Check

Determines whether a DRAM chip is present and actively driving the bus.

---

### Address Sweep

Detects broken or shorted address lines.

---

### Data / WE Check

Verifies data and write-enable signal integrity.

---

## MENU SYSTEM OVERVIEW

### Main Menu

- **Select Chip Type**  
- **Start Test**  
- **Settings**  

---

### Test Settings

- Test Algorithm  
- Access Mode (Standard / Fast Page)  
- Loop Count  (1, 2, 3 , 4, 5 .... to INFINITY)
- On Error Behaviour  (Stop or Restart)
- End-of-Test Power Mode  (Power ON / Power Cycle)
- Retention Time  (Allows the user to change the default Retention Time)
- Cycle Delay (Time betweeen cyclic/loop tests)

---

### Visual & UI Settings

⚡ *Disable Phase Messages for fastest testing*

- Phase Messages ON / OFF  (Shows the current test being carried out)
- Result Display Size (Small / Large)  (Small - detailed fault report Large - Large FAIL message)

---

### Advanced & Diagnostics

- Extreme Test Configuration  (Allows the user to select a combination of tests)
- Pre-Test Controls  
- Hardware Pin Test (Manual / Auto) 
- Reset Defaults  

---

## CONTROLS

- **Rotary Scroll:** Navigate menus  
- **Rotary Press:** Select / Toggle  
- **Long Press (during test):** Abort test and cut power  

**PC Commands**
- Reverse encoder direction  
- Toggle SSD1306 / SH1106 OLED driver  

---

## TROUBLESHOOTING

**“OFFLINE” Message**
- Check USB cable  
- Verify correct COM port selection  

---
