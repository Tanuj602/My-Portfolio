# 👀 Real-Time Gaze Estimation via Sim2Real Transfer Learning

> Real-time line-of-sight estimation using only a standard webcam — no infrared sensors, no specialized hardware. Sim2Real Transfer Learning bridges synthetic training to real-world deployment; Reinforcement Learning optimizes accuracy across arbitrary screen positions.

**Purdue ECE 49595 — Computer Vision | Fall 2025 | Grade: A**

---

## 📋 Project Overview

Gaze estimation (knowing where a user is looking on screen) typically requires expensive dedicated hardware (Tobii, etc. — $200–$3,000). This project achieves real-time gaze tracking using only a standard webcam by training in synthetic simulation and transferring to real-world deployment.

**Core challenge:** Real labeled gaze data is extremely expensive to collect. Synthetic data (rendered eyes with known gaze directions) is free to generate but looks different from real webcam feeds — the **Sim2Real gap**.

---

## 🏗️ System Architecture

```
Standard Webcam Feed (real-time)
        ↓
Eye Region Detection (OpenCV)
        ↓
Gaze Estimation Model
├── Trained on: Synthetic rendered eyes (Sim)
├── Adapted to: Real webcam feeds (Real)
└── Method: Sim2Real Transfer Learning
        ↓
Reinforcement Learning Optimization
├── Reward: Gaze accuracy (predicted vs actual)
├── Action: Model parameter adjustments
└── Policy: Generalize across all screen positions
        ↓
Real-Time Gaze Point Output
```

---

## 🔬 Technical Details

**Sim2Real Transfer Learning:**
- Train on synthetically rendered eyes with known ground-truth gaze labels (free, infinite data)
- Domain randomization: varied lighting, skin tone, eye color, background during synthetic training
- Adapted to real webcam feeds — model learns generalizable features not synthetic artifacts

**Reinforcement Learning:**
- RL optimizes model across arbitrary head positions and screen locations
- Reward function: accuracy of predicted gaze point vs actual gaze position
- Policy generalizes to scenarios not seen during training — no need for labeled data at every position

**Real-Time Constraints:**
- Model must run at sufficient frame rate for live user feedback
- Inference optimized for standard consumer laptop GPU/CPU
- Latency is a hard requirement — not just a metric

---

## 📁 Repository Structure

```
gaze-estimation-sim2real/
├── src/
│   ├── detector.py      # Eye region detection
│   ├── estimator.py     # Gaze estimation model
│   ├── sim2real.py      # Domain adaptation
│   └── rl_optimizer.py  # RL-based optimization
├── synthetic/
│   └── data_generator.py # Synthetic eye rendering
├── demo.py              # Real-time webcam demo
├── requirements.txt
└── README.md
```

---

## 🛠️ Tech Stack

`Python` `OpenCV` `PyTorch` `Reinforcement Learning` `NumPy` `Sim2Real Transfer Learning`

---

*Purdue University ECE 49595 — Computer Vision. Lead developer.*
