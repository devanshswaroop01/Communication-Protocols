# 📘 UART Controller – Verilog HDL Implementation

A **fully functional UART Transmitter & Receiver** implemented in **Verilog HDL**, supporting **8-bit data, no parity, 1 stop bit (8N1)** at **115200 baud** using a **50 MHz system clock**. The design includes a baud rate generator, reset-safe TX/RX FSMs, **16× oversampling receiver**, and a **self-checking loopback testbench**.

---

## 🔖 Overview (TL;DR)

This project implements a **standard-compliant UART controller** suitable for SoC peripherals, FPGA labs, and RTL interviews. The design emphasizes **timing accuracy, robustness, and clean modular RTL**, along with a comprehensive verification environment.

---

## 🧠 UART Protocol Overview

UART (Universal Asynchronous Receiver/Transmitter) is an **asynchronous serial communication protocol** characterized by:

* No shared clock between transmitter and receiver
* Start and stop bits for synchronization
* Fixed baud rate agreed by both ends

### UART Configuration Used

| Parameter       | Value  |
| --------------- | ------ |
| System Clock    | 50 MHz |
| Baud Rate       | 115200 |
| Data Bits       | 8      |
| Parity          | None   |
| Stop Bits       | 1      |
| Frame Format    | 8N1    |
| RX Oversampling | 16×    |

### UART Frame Format

```
Idle (1) → Start (0) → D0 → D1 → D2 → D3 → D4 → D5 → D6 → D7 → Stop (1)
```

---

## 🎯 Project Scope

### ✔ Included

* Baud rate generator (TX and RX)
* UART transmitter (TX)
* UART receiver (RX)
* 16× RX oversampling for noise tolerance
* Reset-safe FSM design
* Ready / Busy handshaking
* Loopback self-checking testbench

### ❌ Not Included

* Parity generation or checking
* Framing error detection
* FIFO buffering
* Flow control (RTS/CTS)
* Runtime baud-rate selection

---

## ✨ Features

* Standard-compliant UART (8N1)
* Accurate baud generation from 50 MHz clock
* 16× RX oversampling for reliable reception
* Reset-safe FSMs for TX and RX
* Clean TX busy signaling
* Explicit RX ready-clear handshake
* Modular, readable RTL design
* Comprehensive self-checking testbench

---

## 🏗️ Architecture Overview

The UART controller is divided into four major blocks:

1. **Baud Rate Generator**
2. **UART Transmitter (TX)**
3. **UART Receiver (RX)**
4. **Top-Level Integration Module**

All blocks operate in a **single 50 MHz clock domain** and communicate using **clock-enable pulses** rather than gated clocks.

---

## 🧩 Block Descriptions

### 🔹 Baud Rate Generator

* Divides the 50 MHz system clock to generate:

  * TX baud tick (1× baud)
  * RX oversampling tick (16× baud)
* Uses counters and clock-enable pulses
* Parameterized divider calculation

### 🔹 Transmitter (TX)

* FSM-based UART transmitter
* Latches data on `wr_en`
* Transmits:

  * Start bit
  * 8 data bits (LSB first)
  * Stop bit
* Provides `tx_busy` to prevent overwrite

### 🔹 Receiver (RX)

* FSM-based UART receiver
* Uses 16× oversampling
* Samples data at bit center
* Tolerates minor baud mismatch
* Asserts `rdy` with valid data
* Uses explicit `rdy_clr` handshake

### 🔹 Top-Level UART Module

* Instantiates baud generator, TX, and RX
* Exposes a clean external interface
* Suitable for SoC or FPGA integration

---

## 🔄 Finite State Machines (FSMs)

### Transmitter FSM States

| State | Description                  |
| ----- | ---------------------------- |
| IDLE  | Line idle, waiting for write |
| START | Transmit start bit           |
| DATA  | Transmit 8 data bits         |
| STOP  | Transmit stop bit            |

### Receiver FSM States

| State | Description                          |
| ----- | ------------------------------------ |
| START | Detect start bit and align sampling  |
| DATA  | Sample 8 data bits                   |
| STOP  | Validate stop bit and complete frame |

---

## 🔌 Interface Signals

### Top-Level UART Ports

#### Inputs

| Signal  | Width | Description         |
| ------- | ----- | ------------------- |
| clk_50m | 1     | 50 MHz system clock |
| rst     | 1     | Asynchronous reset  |
| din     | 8     | Transmit data       |
| wr_en   | 1     | Transmit request    |
| rx      | 1     | UART RX input       |
| rdy_clr | 1     | Clear RX ready flag |

#### Outputs

| Signal  | Width | Description           |
| ------- | ----- | --------------------- |
| tx      | 1     | UART TX output        |
| tx_busy | 1     | Transmitter busy flag |
| dout    | 8     | Received data         |
| rdy     | 1     | RX data valid         |

---

## ⚙️ Parameters & Configurability

### Baud Generator Parameters

* `RX_ACC_MAX` – Divider for RX oversampling
* `TX_ACC_MAX` – Divider for TX baud rate
* Counter widths derived using `$clog2`

⚠️ **Current design uses fixed baud rate (115200) and fixed system clock (50 MHz) at compile time.**

---

## 🔁 Transaction / Operation Flow

### Transmit Operation

1. User asserts `wr_en` with valid `din`
2. TX latches data and asserts `tx_busy`
3. Start bit transmitted
4. 8 data bits transmitted (LSB first)
5. Stop bit transmitted
6. TX returns to IDLE

### Receive Operation

1. RX detects falling edge (start bit)
2. Oversampling aligns to bit center
3. 8 data bits sampled
4. Stop bit validated
5. `rdy` asserted with valid `dout`
6. User clears `rdy` using `rdy_clr`

---

## ⏱️ Timing & Clocking Details

* Single 50 MHz system clock
* Baud generator produces clock-enable pulses
* TX operates on 1× baud enable
* RX operates on 16× baud enable
* Sampling occurs at bit center
* All logic fully synchronous

---

## 🧪 Simulation & Verification

Verification is performed using a **self-checking loopback testbench**:

* TX output connected to RX input
* Automatic data comparison
* Timeout protection
* Reset behavior validation
* Multiple test patterns

---

## 📊 Example Simulation Results

### Console Output Example

```
✅ RX OK: 55
✅ RX OK: 00
✅ RX OK: FF
🎉 ALL UART TESTS PASSED 🎉
```

### Waveform Confirms

* Correct start and stop bit timing
* Accurate baud spacing
* Stable RX sampling at bit centers

---

## 🚀 Quick Start

```
# Compile
iverilog -o uart_sim uart.v uart_tb.v

# Run simulation
vvp uart_sim

# View waveforms
gtkwave uart_tb.vcd
```

---

## 🛠️ Tools Used

* Verilog HDL
* Icarus Verilog (iverilog)
* GTKWave
* ModelSim / Questa / Vivado Simulator (compatible)

---

## 📁 Directory Structure

```
├── baud_rate_gen.v   # Baud rate generator
├── transmitter.v    # UART transmitter
├── receiver.v       # UART receiver
├── uart.v            # Top-level UART
├── uart_tb.v         # Self-checking testbench
├── uart_tb.vcd       # Waveform output
├── README.md         # Documentation
```

---

## ⚠️ Limitations

* Fixed baud rate and system clock
* No parity or framing error detection
* No RX/TX FIFO buffering
* No flow control (RTS/CTS)
* Single UART instance

---

## 🔮 Future Enhancements

* Parity support (even/odd)
* Framing and overrun error flags
* Parameterized baud rate and clock
* RX and TX FIFOs
* AXI-Lite / APB UART wrapper
* Interrupt generation
* Multi-UART support

---

**Author:** Devansh Swaroop
**Domain:** RTL Design · UART Protocol · Digital Desig
 
