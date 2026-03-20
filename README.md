
# STM32 Blue Pill Game Console Input System (Proteus + UART + vJoy)

## Overview

This project implements a simulated game console input system using the STM32 Blue Pill microcontroller in Proteus. The system reads multiple button inputs, encodes them into a byte, and transmits the data via UART to a PC. A serial-to-vJoy bridge then maps this data to a virtual joystick, allowing interaction with applications as a game controller.

---

## Objective

To design and simulate a hardware-software system that:

* Reads button inputs using GPIO pins
* Encodes input states efficiently
* Transmits data over UART
* Interfaces with a PC to control a virtual joystick (vJoy)

---

## System Architecture

The system consists of four major components:

1. Input Hardware (Proteus Simulation)
2. STM32 Firmware
3. UART Communication (COMPIM / Virtual Terminal)
4. PC-side vJoy Integration

---
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7e73c832-e43c-444b-80fb-9658fa9212cb" />

## Circuit Design (Proteus)

### Microcontroller

* STM32F103C8T6 (Blue Pill)

### Inputs

* Multiple push-buttons representing game controls:

  * LB (Left Button)
  * LT (Left Trigger)
  * START
  * BACK
  * RT (Right Trigger)
  * RB (Right Button)

### Connections

* Each button is connected to a GPIO pin
* Uses pull-down resistor configuration

  * Default state: LOW (0)
  * Pressed state: HIGH (1)

### Additional Components

* LEDs for visual feedback of button presses
* LCD (for optional display/debug)
* Potentiometer for LCD contrast
* Power rail configuration block
* Virtual terminal for debugging UART output

### UART Interface

* UART pins connected to:

  * COMPIM module, or
  * Virtual Terminal (for debugging)

---

## Firmware Design

### Working Principle

1. Continuously read GPIO inputs
2. Encode button states into a single byte
3. Transmit the byte via UART at fixed intervals (approximately 50 Hz)

---

### Data Encoding Format

Each bit in a byte represents a button:

| Bit Position | Button   |
| ------------ | -------- |
| Bit 0        | LB       |
| Bit 1        | LT       |
| Bit 2        | START    |
| Bit 3        | BACK     |
| Bit 4        | RT       |
| Bit 5        | RB       |
| Bit 6–7      | Reserved |

Example:
`0b00010101` indicates LB, START, and RT are pressed.

---

### Transmission Rate

* Approximately 50 Hz (every 20 ms)

---

### Key Firmware Tasks

* GPIO initialization
* UART initialization
* Bitwise packing of input states
* Periodic transmission loop

---

## Simulation and Testing

### Steps

1. Load compiled firmware into STM32 in Proteus
2. Run the simulation
3. Press buttons in the schematic
4. Observe:

   * LED indicators
   * UART output in Virtual Terminal or COMPIM

---

## PC Integration (vJoy)

### Workflow

1. UART data is received on the PC via a COM port
2. A Python (or similar) script reads serial data
3. The script maps each bit to a vJoy button
4. The vJoy driver updates the virtual joystick state

---

### Result

Pressing buttons in Proteus updates the corresponding buttons in the Windows Game Controllers panel.

---

## Project Structure

```
GameConsoleInputSystem
 ┣ Proteus
 ┃ ┗ .pdsprj file
 ┣ Firmware
 ┃ ┣ main.c
 ┃ ┣ gpio.c
 ┃ ┗ uart.c
 ┣ PC_Interface
 ┃ ┗ serial_to_vjoy.py
 ┣ Media
 ┃ ┗ demo screenshots / video
 ┗ README.md
```

---

## Demonstration

The system demonstrates the complete pipeline:

Button press in Proteus → UART transmission → PC script processing → vJoy button activation

---

## Design Choices

### Efficient Data Encoding

* Single-byte transmission minimizes bandwidth and latency

### Modular Design

* Hardware, firmware, and PC interface are decoupled

### Real-time Feedback

* LEDs and UART terminal aid debugging

### Scalability

* Additional buttons or analog inputs can be added easily




