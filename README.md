# Reaction Time Race Game (RTR-Game)

A hardware-only, microcontroller-free 3-Channel Asynchronous Competition Arbiter (Quiz Buzzer Circuit) featuring an integrated 15-second master timing window. This project was designed and verified using the Proteus 8 Professional Schematic Capture suite for the Digital Electronics and Computer Design course curriculum.

## 🚀 Project Overview

This architecture resolves millisecond-level arrival timing differences between near-simultaneous user inputs. Unlike traditional layouts that rely heavily on complex, multi-stage digital decade counters ($74\text{LS}90$) and BCD-to-7-segment decoders ($74\text{LS}47$), this streamlined design utilizes a **counter-free static display network**. It drives visual feedback concurrently through direct LED illumination and discrete 7-segment display activation using high-efficiency **BC547 NPN transistor switching arrays**.

### Key Features
* **Asynchronous Arbitration:** Built using negative-edge-triggered JK Flip-Flops ($74\text{LS}73$) to instantly latch the fastest player's input edge.
* **Hardwired Lockout Rail:** Implements a high-speed diode matrix ($1\text{N}4148$) that permanently isolates and ignores trailing player responses once a dominant channel is locked.
* **Monostable Master Control:** Integrates a precision $\text{NE}555$ timer configuration to enforce a strict, 15-second active response window.
* **Optimized Hardware Footprint:** Eliminates cascading digital counters to reduce propagation delay, wiring complexity, and power draw.

---

## ⚙️ Circuit Architecture & Methodology

The system operates on a regulated $+5\text{V}$ $V_{CC}$ rail and is organized into four standalone functional blocks:

1. **Input Stage:** Three pulled-up tactile push-buttons for the contestants and a master reset switch for the moderator.
2. **Control Unit:** An $\text{NE}555$ timer configured in monostable mode. The active gameplay window duration ($T$) is determined by an external $RC$ network:
   $$T = 1.1 \cdot R \cdot C \approx 15\text{ seconds}$$
3. **Logic Memory Core:** Configured $74\text{LS}73$ ICs that switch state immediately upon receiving a falling edge from a player button, pulling a shared status lockout rail high via $1\text{N}4148$ steering diodes to block rival inputs.
4. **Counter-Free Output Stage:** Active-high latched outputs route directly into the bases of $\text{BC}547$ NPN transistors. The saturated transistor completes the ground path for the corresponding player LED and a common-cathode 7-segment display hardwired to project a static player number ("1", "2", or "3").

---

## 📊 Component Inventory

| Component Type | Part Number / Specification | Purpose |
| :--- | :--- | :--- |
| **Logic IC** | $74\text{LS}73$ (Dual JK Flip-Flop with Clear) | Captures input edges and maintains memory |
| **Timer IC** | $\text{NE}555$ (Precision Timer) | Drives the 15-second monostable master gate |
| **Transistors** | $\text{BC}547$ (NPN Bipolar Junction Transistor) | Low-side saturation switches for indicators |
| **Diodes** | $1\text{N}4148$ (High-Speed Switching Diode) | Constructs the hardwired lockout feedback rail |
| **Displays** | 7-Segment Display (Common-Cathode) | Visual static player position indication |
| **Acoustic** | Piezoelectric Buzzer | Provides immediate auditory registration feedback |
| **Passives** | Resistors ($330\,\Omega, 1\text{ k}\Omega, 10\text{ k}\Omega, 120\text{ k}\Omega$) | Current-limiting and logic pull-ups |
| **Passives** | Capacitors ($0.01\,\mu\text{F}, 100\,\mu\text{F}$) | Timer duration adjustments and noise filtering |

---

## 💻 Simulation & Testing

The schematic design and timing constraints were fully validated inside **Proteus 8 Professional**. 
* **Race Condition Testing:** Simultaneous inputs introduced at the microsecond level successfully resolve with zero-latency, establishing absolute lockout security for trailing channels.
* **Electrical Safety:** Resistor limits are matched to deliver stable $2.2\text{V}$ drops across the display filaments, avoiding voltage sagging across the driving TTL pins.
