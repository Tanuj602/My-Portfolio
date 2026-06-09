# 🎮 Atari Breakout Embedded System

> Complete Atari Breakout game implemented in low-level C on a microcontroller — SPI display communication, PWM audio output, interrupt-driven game loop, and register-level peripheral configuration. No library abstraction.

**Purdue ECE 36200 — Microprocessor Systems & Interfaces | Fall 2025**

---

## 📋 Project Overview

Full hardware/software co-design project — designed and implemented a complete embedded gaming system from scratch in C at the register level. Every peripheral is configured via direct register manipulation, not library calls, demonstrating deep hardware understanding.

---

## 🏗️ System Architecture

```
Microcontroller
├── SPI Controller (display)
│   ├── Clock: configured via timer register
│   ├── Data: bit-banged at register level
│   └── CS: GPIO register direct control
├── PWM Timer (audio)
│   ├── Frequency: set via timer compare register
│   └── Duty cycle: game event driven
├── Interrupt Controller (game loop)
│   ├── Timer interrupt: frame timing
│   ├── Input interrupt: paddle control
│   └── Collision interrupt: ball physics
└── Game Logic
    ├── Ball physics
    ├── Paddle control
    ├── Brick collision detection
    └── Score tracking
```

---

## 🔬 Technical Implementation

**SPI Display Communication (register-level):**
```c
// Direct SPI register configuration — no library
SPI1->CR1 = SPI_CR1_MSTR | SPI_CR1_SSM | SPI_CR1_SSI |
            SPI_CR1_SPE | (0b010 << SPI_CR1_BR_Pos);
// Bit-banged data transmission
while (!(SPI1->SR & SPI_SR_TXE));
SPI1->DR = data;
while (!(SPI1->SR & SPI_SR_RXNE));
```

**PWM Audio (timer register):**
```c
// Timer configured for PWM at register level
TIM3->PSC = SystemCoreClock / 1000000 - 1;  // 1MHz timer
TIM3->ARR = frequency_hz;                    // Period
TIM3->CCR1 = frequency_hz / 2;              // 50% duty cycle
TIM3->CCER |= TIM_CCER_CC1E;               // Enable channel
```

**Interrupt-Driven Game Loop:**
```c
// Frame timer interrupt — precise 60fps timing
void TIM2_IRQHandler(void) {
    if (TIM2->SR & TIM_SR_UIF) {
        TIM2->SR &= ~TIM_SR_UIF;
        update_game_state();
        render_frame();
    }
}
```

---

## ✅ Testing Methodology

Each subsystem was independently unit-tested before integration:

1. **SPI display** — verified pixel-by-pixel before game logic
2. **PWM audio** — measured frequency on oscilloscope against register values
3. **Interrupt timing** — verified frame rate with logic analyzer
4. **Game physics** — tested collision detection with known ball positions
5. **System integration** — full gameplay testing after all subsystems verified

---

## 📁 Repository Structure

```
atari-breakout-embedded/
├── src/
│   ├── main.c           # Entry point and system init
│   ├── display.c        # SPI register-level driver
│   ├── audio.c          # PWM timer configuration
│   ├── game.c           # Game logic and physics
│   ├── interrupts.c     # Interrupt handlers
│   └── peripherals.h    # Register definitions
├── test/
│   ├── test_display.c
│   ├── test_audio.c
│   └── test_physics.c
├── Makefile
└── README.md
```

---

## 🛠️ Tech Stack

`C` `ARM Cortex-M` `SPI` `PWM` `Timers` `Interrupts` `Register-Level Programming` `Hardware/Software Co-Design`

---

# README: Portfolio Root
cat > /home/claude/github_readmes/README_profile.md << 'EOF'
---

*Purdue University ECE 36200 — Microprocessor Systems & Interfaces.*
