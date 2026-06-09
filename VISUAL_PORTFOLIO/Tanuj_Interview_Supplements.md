# Tanuj Mangalam — Interview Preparation Supplements

**Use these documents during interview prep.**

---

# Part 1: Rapid Facts Sheet

## Print this or paste into your phone 5 min before any technical call

### Clinical AI/ML
| Project | Key Metric | Why It Matters |
|---------|-----------|---|
| **Parkinson MoCA** | RMSE **2.17** | ±2 MoCA points on 0–30 scale = screening-level acceptable |
| **DR/DME CNN** | **60% accuracy** | Outperformed RF (57.5%), SVM (55%), XGBoost (53.75%) |

### Hardware / Analog
| Project | Key Metric | Why It Matters |
|---------|-----------|---|
| **CMOS Op-Amp** | **45.6 µW** | Beats 50 µW power budget; GBW 39.2 MHz = 164x target |
| **MOSFET Vth** | Vth: **0.289→0.605 V** | Ioff reduced **3 orders of magnitude** |
| **Atari Breakout** | Real-time on MCU | SPI, PWM, GPIO, interrupts — no library abstraction |

### Why These Matter to Atomic Semi (Embedded Systems)
- **Register-level C** on MCU (Atari, MOSFET hw support)
- **Hardware timing** understanding (PWM, SPI, interrupts)
- **Power awareness** (Op-Amp 45.6 µW design)
- **Cleanroom fabrication** experience (VIP dry etching SOP)
- **Production quality** (not just working code — optimized for constraints)

---

## 🎤 Top 5 Interview Questions & Your Answers

### Q1: "Why did Ridge/ElasticNet win over XGBoost?"
**Answer (30 seconds):**
"Clinical accelerometer data is high-dimensional with many *correlated* movement features. Tree-based models like XGBoost overfit on small clinical datasets. Ridge and ElasticNet apply L2/combined regularization specifically to shrink redundant coefficients toward zero — exactly the right inductive bias for this problem."

---

### Q2: "Tell me about a time you found and fixed a bug."
**STAR Format (2 min):**
- **Situation:** Parkinson's ML pipeline development
- **Task:** Model outputting z-scores instead of MoCA scores (0–30)
- **Action:** Traced pipeline — StandardScaler was transforming target variable. Separated feature scaling from target handling.
- **Result:** Corrected pipeline produced RMSE 2.17. Documented lesson.

---

### Q3: "Walk me through the Op-Amp architecture."
**Answer (60 seconds):**
"Cascoded Current Mirror Op-Amp with SC-CMFB. Current mirror for sub-50 µW power. Cascoding multiplies output resistance for DC gain. SC-CMFB uses zero static power — continuous-time CMFB would waste budget.

N=140 PMOS (W=280 µm, L=10 µm) maximizes gm, minimizes thermal noise. L=10 µm long channel for high output resistance — trades bandwidth for gain. Result: 45.6 µW, 39.2 MHz GBW, -79 dB noise. Gain gap (1.6 dB) fixed via gain-boosting amps. Noise gap (7 dB) is 1/f flicker — fixed via chopper stabilization."

---

### Q4: "How do you approach constrained embedded systems?"
**Answer (60 seconds):**
"Constraints first. Op-Amp: 50 µW power → immediately rules out continuous-time CMFB. Atari: real-time frame rate → interrupt-driven loop, no recursion. MOSFET: raise Vth without geometry change → work function + interface charge tuning only.

Size everything backward from constraints. Not about fanciest solution — about best solution *within limits*."

---

### Q5: "What's your biggest weakness?"
**Answer (honest recovery):**
"I haven't worked on ultra-low-power wireless or production RTOS at scale. Op-Amp was 45 µW lab-scale; production wearables on coin cells hit picoamp budgets I haven't designed for. That's why [this role] appeals to me — to push into that regime."

---

# Part 2: Project Metrics Summary

## 🧠 Parkinson's Cognitive Assessment ML
- **RMSE:** **2.17** (vs. 2.18–2.64 for alternatives)
- **Dataset:** 5-minute accelerometer windows
- **Models tested:** 7 (Linear, Ridge, ElasticNet, RF, GB, XGBoost, AdaBoost)
- **Clinical:** ±2 points on 0–30 MoCA = screening-acceptable
- **Insight:** Ridge/ElasticNet beat XGBoost on correlated features

## 👁️ Diabetic Retinopathy CNN
- **Accuracy:** **60%** (vs. RF 57.5%, SVM 55%, XGBoost 53.75%)
- **Dataset:** MESSIDOR-2, 1,200 images, 3-class
- **Architecture:** 7-layer CNN, 64→4096 filters
- **Hardest class:** DR+DME (40% acc) — only 226 images
- **Insight:** Spatial reasoning of CNNs > flat models

## 👀 Gaze Estimation (Sim2Real + RL)
- **Real-time** line-of-sight on standard webcam
- **Techniques:** Domain randomization, RL optimization
- **Grade:** **A**
- **Advantage:** No specialized hardware needed

## ⚡ CMOS Op-Amp
- **Power:** **45.6 µW** (budget <50) ✓
- **GBW:** **39.2 MHz** (target 238.83 kHz) — 164x ✓
- **Slew rate:** **4.986 V/µs** ✓
- **DC gain:** 52.6 dB (target 54.2) — 1.6 dB gap
- **Noise:** -79 dB (target -86) — 1/f flicker

## 🔬 MOSFET Characterization
- **Vth:** 0.289 → **0.605 V** (+109%)
- **Ioff:** **3+ orders** reduction
- **SS analytical:** 161.94 (matched 162 simulation)
- **Insight:** Vth engineering without geometry change

## 🎮 Atari Breakout
- **Peripherals:** GPIO, SPI, PWM, SD card
- **Implementation:** Register-level C, real-time
- **Architecture:** Interrupt-driven 60fps loop
- **Grade:** Functional system, team project

## 🤖 GenAI Externship (Cognizant)
- **Focus:** LLM, agentic frameworks, prompt engineering
- **Deliverable:** Live production project
- **Insight:** Demo AI vs production AI gap

## 🏭 VIP Senior Design (Cleanroom)
- **Dry etching SOP** execution on real wafer
- **2 semesters** (Fall 2025–Spring 2026)
- **Grade:** **A+**

---

# Part 3: Before Every Call

## 5 min before
Reread "Rapid Facts Sheet" above

## 10 min before
Review their job description — which of your 7 projects most directly relates?

## During call
If they ask something you didn't prep: "Great question — let me think" (5 sec pause is fine)

## After call
Write 2–3 sentences: What went well? What surprised you?

---

*Last updated: December 2026*
