# 📘 SPI Master Controller – FSM-Based RTL Implementation

A **simple, FSM-driven SPI Master (Transmit-Only)** implemented in **Verilog HDL**, capable of transmitting **16-bit data frames** over MOSI using **SPI Mode‑0 (CPOL = 0, CPHA = 0)**. The project includes a **self-checking testbench** that validates frame completion, bit-level timing, and continuous SPI transfers.

---

## 🔖 Overview (TL;DR)

This project demonstrates a **clean and timing-accurate SPI Master controller** built using an FSM and shift-register based serialization. It focuses on **protocol correctness, deterministic behavior, and waveform clarity**, making it ideal for **RTL learning, labs, and interview preparation**.

> The design is intentionally minimal and transmit-only, emphasizing SPI fundamentals rather than full production features.

---

## 🧠 SPI Protocol Overview

SPI (Serial Peripheral Interface) is a **synchronous serial communication protocol** that uses:

* **SCLK** – Serial clock (driven by master)
* **MOSI** – Master Out, Slave In
* **CS / SS** – Chip Select (active LOW)

### SPI Characteristics Used in This Design

* Master-driven clock
* Frame-based transfer controlled by CS
* Data sampled on clock edge
* No addressing (point-to-point)

### SPI Mode Implemented

| Parameter  | Value                     |
| ---------- | ------------------------- |
| CPOL       | 0 (Clock idle LOW)        |
| CPHA       | 0 (Sample on rising edge) |
| Data Order | MSB first                 |

---

## 🎯 Project Scope

### ✔ Included

* SPI Master (Transmit-only)
* 16-bit frame transmission
* FSM-controlled clock and data sequencing
* Active-low chip-select handling
* Continuous frame transmission
* Self-checking testbench with bit-level monitoring

### ❌ Not Included

* MISO (no receive path)
* Clock divider
* Slave device model
* Multi-slave support
* Configurable SPI modes

---

## ✨ Features

* FSM-based SPI timing control
* SPI Mode‑0 compliant operation
* MSB-first data transmission
* Fully registered, glitch-free outputs
* Continuous frame transmission
* Deterministic testbench logging
* Waveform-friendly and readable RTL

---

## 🏗️ Architecture Overview

The design consists of three main components:

1. **Finite State Machine (FSM)**
2. **Shift Register for Data Serialization**
3. **SPI Signal Control Logic**

The FSM controls:

* Chip-select timing
* SPI clock generation
* MOSI data sequencing

---

## 🧩 Block Descriptions

### 🔹 Shift Register

* Holds the 16-bit parallel input data
* Outputs one bit at a time to MOSI
* Loaded only when SPI is idle

### 🔹 Bit Counter

* Tracks the current bit index (15 → 0)
* Determines frame length and termination

### 🔹 SPI Signal Registers

* `spi_cs_l` – Active-low chip select
* `spi_sclk` – SPI serial clock
* `spi_data` – MOSI output

### 🔹 FSM Controller

* Sequences data setup and sampling
* Enforces SPI Mode‑0 timing rules

---

## 🔄 Finite State Machine (FSM)

### FSM States

| State | Description                   |
| ----- | ----------------------------- |
| IDLE  | Bus idle, CS deasserted       |
| LOAD  | Drive MOSI with current bit   |
| CLK_H | Raise SCLK (sampling edge)    |
| CLK_L | Lower SCLK and shift next bit |

### FSM Timing Rules

* MOSI changes only when **SCLK is LOW**
* Slave samples data on **SCLK rising edge**
* CS asserted for exactly **16 clock cycles**

---

## 🔌 Interface Signals

### Inputs

| Signal | Width | Description         |
| ------ | ----- | ------------------- |
| clk    | 1     | System clock        |
| reset  | 1     | Asynchronous reset  |
| datain | 16    | Parallel data input |

### Outputs

| Signal   | Width | Description            |
| -------- | ----- | ---------------------- |
| spi_cs_l | 1     | Active-low chip select |
| spi_sclk | 1     | SPI serial clock       |
| spi_data | 1     | MOSI output            |
| counter  | 5     | Bit counter (debug)    |

---

## ⚙️ Parameters & Configurability

⚠️ **Current version has no parameters**

Hard-coded design choices:

* 16-bit data width
* SPI Mode‑0 operation
* MSB-first transmission
* System clock directly used as SPI clock

---

## 🔁 Transaction / Operation Flow

1. FSM enters `IDLE`
2. Input data loaded into shift register
3. CS asserted LOW
4. For each bit:

   * MOSI driven while SCLK LOW
   * SCLK raised (slave samples)
   * SCLK lowered
5. After last bit:

   * CS deasserted
   * FSM returns to `IDLE`
6. Next frame begins automatically

---

## ⏱️ Timing & Clocking Details

* SPI clock derived directly from system clock
* Each SPI bit requires two FSM cycles
* Data setup occurs before SCLK rising edge
* SCLK idle state = LOW
* All outputs are fully registered

---

## 🧪 Simulation & Verification

Verification is performed using a **self-contained testbench** featuring:

* Frame completion monitoring
* Bit-level MOSI sampling logs
* Multiple consecutive data frames
* Reset behavior validation
* Waveform generation for GTKWave

---

## 📊 Example Simulation Results

### Observed Behavior

* Exactly 16 SCLK pulses per frame
* Correct MSB-first transmission
* CS asserted only during valid frames
* Stable MOSI before rising edge
* Clean separation between frames

### Example Console Output

```
SPI FRAME 1 COMPLETED
Time : 320 ns
Data Sent : 0x0412
```

---

## 🚀 Quick Start

```
# Compile
iverilog -o spi_sim spi_state.v test_bench.v

# Run simulation
vvp spi_sim

# View waveform
gtkwave waveform.vcd
```

---

## 🛠️ Tools Used

* Verilog HDL
* Icarus Verilog (iverilog)
* GTKWave
* ModelSim / Vivado Simulator (compatible)

---

## 📁 Directory Structure

```
├── spi_state.v      # SPI Master RTL
├── test_bench.v    # Verification testbench
├── waveform.vcd    # Simulation waveform
├── README.md       # Project documentation
```

---

## ⚠️ Limitations

* Transmit-only (no MISO support)
* No clock divider
* No explicit start/enable signal
* Fixed SPI mode
* Single-slave support
* Continuous transmission only

---

## 🔮 Future Enhancements

* Add MISO for full-duplex SPI
* Configurable CPOL / CPHA modes
* Programmable data width
* Clock divider for SPI frequency control
* Start / enable control signal
* Multi-slave chip-select handling
* Assertion-based verification (SVA)

---

**Author:** Devansh Swaroop
**Domain:** RTL Design · SPI Protocol · Digital Design · VLSI
 
