# Arduino UNO Q Project Briefing

## Goal
Port an existing Arduino sketch written for the **UNO R4 Minima** to the **Arduino UNO Q**. Future work includes generating new code for the Linux (Qualcomm MPU) side with a Bridge connection to the MCU.

---

## Target Hardware: Arduino UNO Q (ABX00162)

| Component | Details |
|---|---|
| MPU | Qualcomm QRB2210 — runs full Debian Linux |
| MCU | STMicroelectronics STM32U585 (ARM Cortex-M33) — runs Arduino sketches on Zephyr OS |
| Communication | Arduino Bridge (RPC library between MPU and MCU) |

The UNO Q is fundamentally different from the R4 Minima:
- The R4 Minima uses a Renesas RA4M1 MCU with the `arduino:renesas_uno` core
- The UNO Q uses an STM32U585 MCU with the **`arduino:zephyr`** core
- Any R4-specific libraries or peripheral APIs in the source sketch will need to be replaced with STM32U585/Zephyr equivalents

---

## Board Connection

The board is connected **over Wi-Fi** (not USB serial).

| Field | Value |
|---|---|
| IP Address | `192.168.1.105` |
| Protocol | `network` |
| FQBN | `arduino:zephyr:unoq` |
| Core | `arduino:zephyr` |

Output of `arduino-cli board list`:
```
Port          Protocol Type         Board Name    FQBN                Core
192.168.1.105 network  Network Port Arduino UNO Q arduino:zephyr:unoq arduino:zephyr
```

---

## arduino-cli Commands

**Compile:**
```bash
arduino-cli compile -b arduino:zephyr:unoq MySketch
```

**Upload (over network):**
```bash
arduino-cli upload -p 192.168.1.105 -b arduino:zephyr:unoq --protocol network MySketch
```

**Monitor board list:**
```bash
arduino-cli board list
```

---

## Immediate Task

1. Read the existing UNO R4 Minima sketch
2. Identify any R4/RA4M1-specific APIs, libraries, or peripherals
3. Port them to STM32U585/Zephyr equivalents
4. Compile with `arduino-cli` using the FQBN above
5. Fix any errors iteratively
6. Upload to the board at `192.168.1.105`

---

## Future Work

- Write Linux-side (Debian/Python or C++) code on the Qualcomm QRB2210
- Use the **Arduino Bridge RPC library** to communicate between the Linux MPU and the Arduino MCU
- Deploy and test Linux-side code on the board (SSH access to Debian environment)

---

## Suggested Tools / Agent Setup

- **arduino-cli** must be installed locally with the `arduino:zephyr` core
- Confirm core is installed: `arduino-cli core list` (look for `arduino:zephyr`)
- Install if missing: `arduino-cli core install arduino:zephyr`
- The Arduino Development MCP skill for Claude Code can automate compile/upload/monitor loops if configured

---

## How to Use This File in Claude Code

Start your Claude Code session with:
```
Read uno-q-project-briefing.md and use it as context for this session. 
Then read [your sketch file] and begin porting it to the Arduino UNO Q.
```
