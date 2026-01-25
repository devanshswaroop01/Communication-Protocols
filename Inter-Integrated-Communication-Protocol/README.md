# 📘 I²C Master Controller (RTL Implementation)

A **clean, FSM-based I²C Master controller** implemented in **Verilog HDL**, supporting single-byte read and write transactions with accurate **open-drain SDA/SCL bus modeling**. The project includes a deterministic behavioral I²C slave for protocol-correct simulation and verification.

---

## 🔖 Overview (TL;DR)

This project implements a **single-master I²C controller** operating in **Standard Mode (100 kHz)**. The design strictly follows I²C timing rules and demonstrates correct handling of:

* START and STOP conditions
* Address + R/W transmission
* ACK / NACK handshaking
* Open-drain bus behavior

It is intended for **RTL learning, labs, and VLSI/SoC interviews**, not for full-featured production use.

---

## 🧠 I²C Protocol Overview

I²C (Inter-Integrated Circuit) is a **synchronous, serial, multi-master, multi-slave** communication protocol using two open-drain signals:

* **SDA** – Serial Data
* **SCL** – Serial Clock

### Key Protocol Characteristics

* 7-bit slave addressing + 1 R/W bit
* START and STOP conditions
* ACK / NACK handshaking
* Data valid when **SCL is HIGH**
* Data changes only when **SCL is LOW**

> This project implements a **single-master I²C controller in Standard Mode (100 kHz)**.

---

## 🎯 Project Scope

### ✔ Included

* I²C Master RTL
* Address + R/W transmission
* Single-byte write transaction
* Single-byte read transaction
* START, ACK, NACK, and STOP handling
* Open-drain SDA/SCL modeling
* Protocol-accurate verification testbench

### ❌ Not Included

* Multi-byte burst transfers
* Clock stretching
* Multi-master arbitration
* Repeated START conditions

---

## ✨ Features

* FSM-based I²C protocol controller
* Parameterized system and I²C clock frequencies
* Accurate open-drain SDA/SCL modeling
* Single-byte READ and WRITE support
* Clean START and STOP generation
* Deterministic slave model for verification
* Reset-safe and transaction-safe behavior
* Readable, modular RTL structure

---

## 🏗️ Architecture Overview

The controller is composed of three major blocks:

1. **Clock Divider**
2. **Open-Drain I²C Bus Interface**
3. **FSM-Controlled Protocol Engine**

The system clock is divided to generate an I²C-compliant SCL, while the FSM sequences SDA behavior according to strict I²C timing rules.

---

## 🧩 Block Descriptions

### 🔹 Clock Divider

* Converts `SYS_CLK_HZ` to desired `I2C_CLK_HZ`
* Generates internal SCL toggle timing
* Ensures symmetric HIGH and LOW phases

### 🔹 Open-Drain Interface

* SDA and SCL are driven **LOW only**
* Lines are released to HIGH via pull-ups
* Accurately models real I²C electrical behavior

### 🔹 Protocol FSM

* Controls SDA output enable
* Tracks bit index and transaction phase
* Handles ACK/NACK sampling and generation

---

## 🔄 Finite State Machine (FSM)

### FSM States

| State       | Description                  |
| ----------- | ---------------------------- |
| IDLE        | Wait for transaction request |
| START       | Generate START condition     |
| SEND_ADDR   | Send slave address + R/W bit |
| ADDR_ACK    | Sample slave ACK/NACK        |
| SEND_DATA   | Send write data byte         |
| DATA_ACK    | Sample data ACK              |
| READ_DATA   | Receive data byte from slave |
| MASTER_NACK | Master sends NACK after read |
| STOP        | Generate STOP condition      |
| DONE        | Transaction completion pulse |

### FSM Timing Rules

* SDA changes only when **SCL is LOW**
* SDA sampled only when **SCL is HIGH**

---

## 🔌 Interface Signals

### Inputs

| Signal     | Width | Description          |
| ---------- | ----- | -------------------- |
| clk        | 1     | System clock         |
| rst        | 1     | Asynchronous reset   |
| start      | 1     | Start transaction    |
| rw         | 1     | Read (1) / Write (0) |
| slave_addr | 7     | I²C slave address    |
| wdata      | 8     | Write data           |

### Outputs

| Signal | Width | Description                |
| ------ | ----- | -------------------------- |
| rdata  | 8     | Read data                  |
| busy   | 1     | Transaction in progress    |
| done   | 1     | One-cycle completion pulse |

### Bidirectional

| Signal | Description               |
| ------ | ------------------------- |
| sda    | Serial Data (open-drain)  |
| scl    | Serial Clock (open-drain) |

---

## ⚙️ Parameters & Configuration

| Parameter  | Description                 |
| ---------- | --------------------------- |
| SYS_CLK_HZ | System clock frequency      |
| I2C_CLK_HZ | Desired I²C clock frequency |

### Example Configuration

```
.SYS_CLK_HZ(1_000_000),
.I2C_CLK_HZ(100_000)
```

---

## 🔁 Transaction Flow

### Write Transaction

1. Generate START condition
2. Send slave address + WRITE bit
3. Sample ACK from slave
4. Send data byte
5. Sample ACK
6. Generate STOP condition

### Read Transaction

1. Generate START condition
2. Send slave address + READ bit
3. Sample ACK from slave
4. Receive data byte
5. Master sends NACK
6. Generate STOP condition

---

## ⏱️ Timing & Clocking Details

* SCL derived from system clock using divider
* SDA driven only during SCL LOW phase
* SDA sampled during SCL HIGH phase
* START and STOP generated while SCL is HIGH
* Fully compliant with I²C Standard Mode timing

---

## 🧪 Simulation & Verification

Verification uses a **protocol-aware behavioral slave model**:

* Accurate ACK/NACK handling
* Edge-aligned data sampling
* Reset and error scenario testing

### Testbench Coverage

* Single-byte write
* Single-byte read
* Reset during transaction
* Slave NACK handling

---

## 📊 Expected Simulation Observations

* Correct START and STOP timing
* SDA stable during SCL HIGH
* Proper ACK cycles
* Correct read data capture
* `done` asserted for one cycle after STOP

---

## 🚀 Quick Start

```
# Compile
iverilog -o iic_sim iic_master.v iic_master_tb.v

# Run
vvp iic_sim

# View waveform
gtkwave iic_master_final_tb.vcd
```

---

## 🛠️ Tools Used

* Verilog HDL
* Icarus Verilog (iverilog)
* GTKWave
* ModelSim / Vivado Simulator (optional)

---

## 📁 Directory Structure

```
├── iic_master.v        # I²C Master RTL
├── iic_master_tb.v     # Testbench with slave model
├── README.md           # Project documentation
├── iic_master.vcd      # Simulation waveform (generated)
```

---

## ⚠️ Limitations

* Single-byte transactions only
* No clock stretching support
* No repeated START conditions
* No multi-master arbitration
* No noise or glitch filtering

---

## 🔮 Future Enhancements

* Multi-byte burst read/write
* Repeated START support
* Clock stretching detection
* Arbitration loss handling
* APB / AXI-Lite wrapper
* FPGA synthesis and timing constraints
* Assertion-based verification (SVA)

---

**Author:** Devansh Swaroop
**Domain:** RTL Design · I²C Protocol · Digital Design · VLSI
 
