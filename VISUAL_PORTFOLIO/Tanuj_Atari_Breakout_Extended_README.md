# 🎮 Atari Breakout Embedded System

> Complete Atari Breakout game implemented in C on a microcontroller — SPI display, PWM audio, GPIO directional input, SD card high-score persistence, and interrupt-driven game loop. Register-level peripheral control, no library abstraction.

**Purdue ECE 36200 — Microprocessor Systems & Interfaces | Fall 2025**
**Team: Proton | Members: Tanuj Mangalam (Tmangala), Anand153, Mgrassi, Brow2384**

---

## 📋 Project Overview

Full hardware/software co-design of the classic Atari Breakout game with extended features:

- **LED matrix display** for game visualization (X-axis movement)
- **LCD leaderboard** via SPI for real-time score and top-5 high scores
- **PWM audio system** for game soundtrack and sound effects
- **SD card storage** for persistent high-score saving/loading
- **GPIO input** with 2 push buttons for paddle directional control

**Tanuj's role:** Lead embedded systems developer — register-level C implementation, peripheral driver architecture, interrupt-driven game loop, hardware/software co-design.

---

## 🏗️ System Architecture

```
Microcontroller
├── GPIO Controller (paddle control)
│   ├── Button 1: move left
│   ├── Button 2: move right
│   └── Debounce logic in firmware
│
├── LED Matrix (game display)
│   ├── Multiplexing control via GPIO pins
│   ├── Ball and block rendering
│   └── Refresh ~60fps
│
├── SPI Interface (LCD leaderboard)
│   ├── SPI clock configured via register
│   ├── Data transmission register-level
│   ├── Real-time score display
│   └── Top-5 high scores from SD card
│
├── PWM Timer (audio output)
│   ├── Game soundtrack frequency
│   ├── Sound effect synthesis
│   ├── Audio jack output
│   └── Speaker/headphones compatible
│
├── SD Card Interface
│   ├── SPI-based SD card communication
│   ├── High-score persistence
│   ├── Level data loading
│   └── Audio file storage (optional)
│
├── Interrupt Controller (game loop)
│   ├── Timer interrupt: frame timing (60fps)
│   ├── Button interrupt: paddle input
│   ├── Collision interrupt: ball physics
│   └── SD card read interrupt: async loading
│
└── Game Logic
    ├── Ball physics simulation
    ├── Paddle collision detection
    ├── Block destruction tracking
    ├── Score calculation
    ├── Ball speed acceleration (row-based)
    ├── 3-life tracking
    ├── Win/loss condition checking
    └── High-score comparison
```

---

## 🔬 Technical Implementation

**GPIO Push Button Input (register-level):**
```c
// Direct GPIO register configuration — no library
GPIO_PORT->MODER &= ~(0b11 << (PIN * 2));    // Clear mode bits
GPIO_PORT->MODER |= (0b00 << (PIN * 2));    // Input mode
GPIO_PORT->PUPDR |= (0b01 << (PIN * 2));    // Pull-up

// Debounce logic in interrupt handler
if ((GPIO_PORT->IDR & (1 << PIN)) == 0) {
    delay_ms(20);
    if ((GPIO_PORT->IDR & (1 << PIN)) == 0) {
        move_paddle_left();
    }
}
```

**SPI LCD Display (register-level):**
```c
// SPI configured for LCD communication
SPI1->CR1 = SPI_CR1_MSTR | SPI_CR1_SSM | SPI_CR1_SPE |
            (0b010 << SPI_CR1_BR_Pos);  // 16MHz from 64MHz clock

// Send score to LCD
void display_score(uint16_t score) {
    while (!(SPI1->SR & SPI_SR_TXE));
    SPI1->DR = (uint8_t)(score >> 8);  // High byte
    while (!(SPI1->SR & SPI_SR_TXE));
    SPI1->DR = (uint8_t)score;         // Low byte
}
```

**PWM Audio (timer register):**
```c
// Timer configured for PWM tone generation
TIM3->PSC = (SystemCoreClock / 1000000) - 1;  // 1MHz timer
TIM3->ARR = (1000000 / frequency_hz);         // Period
TIM3->CCR1 = (1000000 / frequency_hz) / 2;    // 50% duty (square wave)
TIM3->CCER |= TIM_CCER_CC1E;                 // Enable output

// Play sound effect
void play_sound(uint16_t freq_hz, uint16_t duration_ms) {
    TIM3->ARR = (1000000 / freq_hz);
    TIM3->CCR1 = TIM3->ARR / 2;
    delay_ms(duration_ms);
    TIM3->CCR1 = 0;  // Silence
}
```

**SD Card High-Score Persistence (SPI-based):**
```c
// Save high scores to SD card
void save_high_scores(uint16_t scores[5]) {
    sd_open_file("SCORES.BIN", SD_WRITE);
    for (int i = 0; i < 5; i++) {
        sd_write_word(scores[i]);
    }
    sd_close_file();
}

// Load high scores from SD card
void load_high_scores(uint16_t scores[5]) {
    sd_open_file("SCORES.BIN", SD_READ);
    for (int i = 0; i < 5; i++) {
        scores[i] = sd_read_word();
    }
    sd_close_file();
}
```

**Interrupt-Driven Game Loop (60fps):**
```c
// Frame timer interrupt — precise timing
void TIM2_IRQHandler(void) {
    if (TIM2->SR & TIM_SR_UIF) {
        TIM2->SR &= ~TIM_SR_UIF;
        
        // Update game state
        update_ball_position();
        check_collisions();
        update_score();
        
        // Render
        render_to_led_matrix();
        render_to_lcd_display();
        
        // Audio
        if (collision_detected) play_sound(1000, 50);
    }
}
```

---

## ✅ Testing Methodology

Each subsystem was independently unit-tested before integration:

1. **GPIO input** — verified button debounce with logic analyzer
2. **LED matrix** — tested pixel-by-pixel rendering before game logic
3. **SPI LCD** — validated data transmission bit-by-bit
4. **PWM audio** — measured frequency on oscilloscope against calculated values
5. **SD card** — tested read/write of high scores with file verification
6. **Game physics** — tested collision detection with known ball positions
7. **System integration** — full gameplay testing after all subsystems verified

---

## 📁 Repository Structure

```
atari-breakout-embedded/
├── src/
│   ├── main.c               # Entry point and system init
│   ├── gpio_input.c         # Push button driver
│   ├── led_matrix.c         # LED display rendering
│   ├── spi_lcd.c            # LCD SPI driver
│   ├── pwm_audio.c          # Audio synthesis
│   ├── sd_card.c            # SD card high-score persistence
│   ├── game.c               # Game logic and physics
│   ├── interrupts.c         # Interrupt handlers
│   └── peripherals.h        # Register definitions
│
├── test/
│   ├── test_gpio.c
│   ├── test_led_matrix.c
│   ├── test_spi_lcd.c
│   ├── test_pwm_audio.c
│   ├── test_sd_card.c
│   └── test_physics.c
│
├── docs/
│   ├── 362_Mini_Project_Proposal.pdf    # Original project requirements
│   └── technical_analysis.md
│
├── Makefile
└── README.md
```

---

## 🎯 Project Requirements Met

From ECE 36200 Mini Project Proposal:

| Objective | Status | Implementation |
|---|---|---|
| **Obj 1:** GPIO directional movement (X-axis) | ✅ Complete | 2 push buttons → left/right paddle control, debounced |
| **Obj 2:** SPI LCD leaderboard display | ✅ Complete | Real-time score + top-5 high scores on LCD via SPI |
| **Obj 3:** PWM audio system | ✅ Complete | Game soundtrack + sound effects via PWM, audio jack output |
| **Obj 4:** SD card persistence | ✅ Complete | High scores saved/loaded from SD card, levels stored |

**Additional features beyond proposal:**
- Ball speed acceleration (increases per bounce and per row completed)
- 3-life tracking system
- Win/loss conditions with score validation
- LED matrix visualization with smooth rendering

---

## 👥 Team Roles

- **Tanuj Mangalam (Tmangala):** Lead embedded systems developer — register-level C, interrupt architecture, hardware/software co-design, system integration
- **Anand153:** [Role]
- **Mgrassi:** [Role]
- **Brow2384:** [Role]

---

## 🛠️ Tech Stack

`C` `ARM Cortex-M` `GPIO` `SPI` `PWM` `SD Card Protocol` `Timers` `Interrupts` `Register-Level Programming` `Hardware/Software Co-Design` `Real-Time Systems`

---

*Purdue University ECE 36200 — Microprocessor Systems & Interfaces. Fall 2025.*
