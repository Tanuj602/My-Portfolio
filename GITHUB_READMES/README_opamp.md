# ⚡ Differential Current-Mirror CMOS Op-Amp with SC-CMFB

> Fully differential cascoded current-mirror Op-Amp designed for high-speed ADC interfaces in GPDK045 (45nm CMOS). 45.6 µW power consumption within a strict 50 µW budget. Unity-gain bandwidth of 39.2 MHz — 164x above the target.

**Purdue ECE 40656 — CMOS Analog & Mixed-Signal IC Design | Spring 2026**

---

## 📋 Project Overview

Designed a low-power, high-performance differential Op-Amp targeting analog-to-digital interface applications. All design decisions were analytically justified before simulation, then verified against SPICE results.

**Technology:** GPDK045 (45nm CMOS) — benchmarked against TSMC 0.18-µm CMOS reference designs from IEEE literature.

---

## 📊 Performance Summary

| Parameter | Achieved | Target | Status |
|---|---|---|---|
| Power Consumption | **45.6 µW** | < 50 µW | ✅ Met |
| Unity-Gain Bandwidth | **39.2 MHz** | 238.83 kHz | ✅ Exceeded 164x |
| Slew Rate | **4.986 V/µs** | 5 V/µs | ✅ Met |
| DC Gain | **52.6 dB** | 54.2 dB | ~Close (1.6 dB gap) |
| Noise Floor | **-79 dB** | -86 dB | ~Close (7 dB gap) |

---

## 🏗️ Architecture

```
PMOS Differential Input Pair (N=140 multiplier, W=280µm, L=10µm)
        ↓
Current Mirror Load (cascode topology)
        ↓
High-Impedance Output Nodes (rout maximized by long-channel devices)
        ↓
Differential Output
        ↑
SC-CMFB Network (phi_1, phi_2 non-overlapping clocks, 50fF capacitors)
```

---

## 🔬 Design Decisions & Rationale

**Why current mirror topology?**
Maintains high performance at minimal current draw — ideal for sub-50 µW applications where folded-cascode would waste voltage headroom.

**Why cascoding?**
Cascode transistors multiply output resistance of adjacent devices, creating high-impedance nodes that boost DC gain without increasing quiescent current.

**Why SC-CMFB over continuous-time CMFB?**
SC-CMFB consumes zero static power — continuous-time CMFB requires a biased amplifier which would consume power we cannot afford in a 50 µW budget.

**Why N=140 PMOS multiplier with L=10µm?**
- N=140: Maximizes gm1 (transconductance) — thermal noise ∝ 1/gm, so higher gm = lower noise
- L=10µm: Long channel maximizes Rout for high DC gain — deliberate gain-bandwidth trade-off
- Together: Maximizes gm/ID ratio for optimal noise-power trade-off

**Noise gap explanation:**
Remaining 7 dB gap to -86 dB IEEE target is dominated by 1/f flicker noise. Proposed fix: **chopper stabilization** to modulate signal above the 1/f corner frequency.

---

## 📐 Design Equations

```
Voltage Gain:     Av = gm4 · (rup || rdown)
Thermal Noise:    Vin² = 4kTγ / gm1
Slew Rate:        SR = I_tail / C_load
GBW:              GBW = gm / (2π · Cc)
```

---

## 🔧 Testbench Configuration

- Supply: ±1.7V dual rails
- Load: 2× 10µF capacitors
- Input: 1V AC differential, 180° phase shift
- SC-CMFB clocks: 50 MHz, 9ns pulse width, non-overlapping

---

## 📁 Repository Structure

```
cmos-opamp-design/
├── report/
│   └── CurrentMirrorOpAmpFinalLatest.pdf   # Full technical report
├── README.md
└── schematics/                             # Cadence schematic screenshots
```

---

## 🛠️ Tools

`Cadence Virtuoso` `SPICE` `GPDK045` `45nm CMOS Technology`

---

*Purdue University ECE 40656 — CMOS Analog & Mixed-Signal IC Design.*
