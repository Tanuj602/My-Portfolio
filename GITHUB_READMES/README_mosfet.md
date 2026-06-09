# 🔬 MOSFET Characterization & Threshold Voltage Engineering

> NMOS transistor characterization and threshold voltage engineering through work function and interface charge tuning. Analytical results derived from first principles and validated against SPICE simulation — both matched exactly.

**Purdue ECE 305 — Semiconductor Devices | Spring 2026**

---

## 📋 Project Overview

Two-part semiconductor device project:
1. **Part 1:** Engineer NMOS threshold voltage to meet target leakage specifications without changing device geometry
2. **Part 2:** Analytically derive all device parameters from physics and validate against simulation

**Key result:** Derived SS = 161.94 mV/dec and Vth = 0.92 V from first principles — both matched SPICE simulation exactly.

---

## 📊 Engineering Results

| Parameter | Baseline | Engineered | Change |
|---|---|---|---|
| Threshold Voltage (Vth,lin) | 0.289 V | **0.605 V** | +109% |
| Threshold Voltage (Vth,sat) | 0.318 V | **0.626 V** | +97% |
| Off-Current (Ioff,lin) | 8.76×10⁻¹⁰ A | **3.95×10⁻¹³ A** | **-3 orders** |
| Off-Current (Ioff,sat) | 2.57×10⁻⁸ A | **2.84×10⁻¹¹ A** | -3 orders |
| On-Current (Ion,lin) | 8.41×10⁻⁶ A | 5.40×10⁻⁶ A | -36% (expected trade-off) |
| DIBL | 131 mV/V | **130.5 mV/V** | Stable (geometry preserved) |
| Subthreshold Swing | 88.5 mV/dec | 88.6 mV/dec | Unchanged |

---

## 🔬 Analytical Validation (Part 2)

**Device parameters:**
```
Gate stack:  7nm SiO₂ (εr=3.9) + 2nm HfO₂ (εr=25) in series
Cox = [7nm/ε₀·3.9 + 2nm/ε₀·25]⁻¹ = 4.72×10⁻⁷ F/cm²

Channel doping: NA = 2×10¹⁷ cm⁻³
Cd = εsi / Wdmax = 1.29×10⁻⁷ F/cm²

Interface trap density (unannealed process):
Cit = 6.73×10⁻⁷ F/cm²
```

**Subthreshold Swing derivation:**
```
SS = 60 · (1 + (Cd + Cit)/Cox)
   = 60 · (1 + (1.29e-7 + 6.73e-7) / 4.72e-7)
   = 60 · (1 + 1.699)
   = 161.94 mV/dec

Simulation result: ~162 mV/dec ✅ MATCHED
```

**Threshold Voltage derivation:**
```
Vth = ΦMS + 2ΦF - QB/Cox - QIT,total/Cox

ΦMS = -0.15 V
2ΦF = 0.87 V (strong inversion, NA=2×10¹⁷ cm⁻³)
QB = -1.71×10⁻⁷ C/cm²
QIT,total = -7.65×10⁻⁸ C/cm²

Vth = -0.15 + 0.87 - (-1.71e-7/4.72e-7) - (-7.65e-8/4.72e-7)
    = 1.082 - 0.162
    = 0.92 V

Simulation result: 0.92 V ✅ MATCHED EXACTLY
```

---

## 💡 Key Insights

**Why does Vth increase reduce Ioff by 3 orders of magnitude?**
Ioff flows at Vgs=0. The Id-Vg curve shifts right when Vth increases. The subthreshold slope determines how steeply current drops — shifting Vth from 0.289V to 0.605V forces the current at Vgs=0 much further down the exponential slope.

**Why does DIBL stay constant?**
DIBL = (Vth,lin - Vth,sat)/(VDS,lin - VDS,sat) is a geometric parameter — determined by channel length and oxide thickness. Since only WF and Ninterface were changed (not geometry), DIBL is unchanged.

**Why does SS stay constant?**
SS = 60·(1+(Cd+Cit)/Cox) — determined by capacitive coupling ratios, not Vth. Same geometry = same SS. The Id-Vg curve shifts horizontally but the slope is identical.

**The Ion/Ioff trade-off:**
Ion ∝ (Vgs-Vth)² — higher Vth reduces overdrive voltage at fixed Vdd, unavoidably reducing Ion. Cannot reduce leakage without paying a drive current penalty.

---

## 📁 Repository Structure

```
mosfet-characterization/
├── report/
│   └── Report-Tanuj_Mangalam.pdf    # Full technical report with figures
├── README.md
└── analysis/
    └── analytical_derivation.py     # SS and Vth calculation scripts
```

---

## 🛠️ Tools

`NANOMOS` `SPICE` `Python (analytical calculations)` `MATLAB`

---

*Purdue University ECE 305 — Semiconductor Devices. Final Project.*
