# 🧩 AXI4-Lite Protocol Implementation (Master–Slave)

## 1️⃣ Project Title

**AXI4-Lite Master–Slave System (FSM-Based, Single Outstanding Transaction)**

---

## 2️⃣ Short Description (TL;DR)

A clean, FSM-based AXI4-Lite master and slave implementation supporting **single read or write transactions**, verified through **cycle-accurate waveform simulation** and **console logs**. Designed for **clarity, protocol correctness, and educational use**.

---

## 3️⃣ Protocol Overview

AXI4-Lite is a simplified subset of the AMBA AXI protocol intended for **low-bandwidth, memory-mapped control registers**.

**Key characteristics:**

* Separate read and write channels
* Independent address, data, and response handshakes
* No burst support (single beat only)
* Lightweight compared to full AXI4

**This project implements:**

* Full AXI4-Lite read/write handshake semantics
* Independent FSMs for read and write paths
* Proper response handling (**OKAY**, **SLVERR**)

---

## 4️⃣ Project Scope

### Included

✔ AXI4-Lite Master

✔ AXI4-Lite Slave (Register File)

✔ Top-level integration module

✔ Directed verification testbench

✔ Waveform + console-based validation

### Not Included

❌ AXI interconnect

❌ Bursts, out-of-order, or pipelining

---

## 5️⃣ Features

* FSM-driven AXI4-Lite Master
* Independent Read and Write control paths
* Single outstanding transaction model
* Edge-triggered user interface (`wr_en`, `rd_en`)
* Proper AXI channel decoupling
* SLVERR detection and reporting
* 32 × 32-bit memory-mapped slave registers
* Deterministic reset behavior
* Waveform and console-verified correctness

---

## 6️⃣ Architecture Overview

```
User Logic
   |
   v
AXI4-Lite Master
   |
   |  (AW, W, B, AR, R channels)
   |
AXI4-Lite Slave
```

* Master converts user requests into AXI transactions
* Slave responds with register-mapped data
* Top module acts as a minimal SoC wrapper

---

## 7️⃣ Block Descriptions

### 🔹 AXI4-Lite Master

* Two FSMs:

  * **Write FSM:** AW → W → B
  * **Read FSM:** AR → R
* Latches address and data at request time
* Exposes a simple user-side interface

### 🔹 AXI4-Lite Slave

* 32-entry register file
* Independent read/write handshakes
* Always returns **OKAY** response
* Fully synchronous design

### 🔹 Top Module

* Connects master and slave
* Hides AXI complexity from user logic
* Exposes only control/status signals

### 🔹 Testbench

* Directed read/write transactions
* Logs transaction timing and results
* Dumps VCD waveform for debugging

---

## 8️⃣ Finite State Machine (FSM)

### 🟦 Write FSM (Master)

```
W_IDLE → W_AW → W_W → W_B → W_DONE → W_IDLE
```

* Address sent first
* Data sent after address handshake
* Write completes after `BRESP`

### 🟩 Read FSM (Master)

```
R_IDLE → R_AR → R_R → R_DONE → R_IDLE
```

* Address phase
* Data returned with response
* Read completes on `RVALID`

---

## 9️⃣ Interface Signals

### User-Side Interface

| Signal       | Direction | Description             |
| ------------ | --------- | ----------------------- |
| `wr_en`      | Input     | Write request pulse     |
| `rd_en`      | Input     | Read request pulse      |
| `addr`       | Input     | 32-bit address          |
| `wdata_in`   | Input     | Write data              |
| `rdata_out`  | Output    | Read data               |
| `write_done` | Output    | Write completion        |
| `read_done`  | Output    | Read completion         |
| `busy`       | Output    | Transaction in progress |
| `error`      | Output    | SLVERR detected         |

### AXI4-Lite Channels

* **Write Address:** `AWVALID`, `AWREADY`, `AWADDR`
* **Write Data:** `WVALID`, `WREADY`, `WDATA`
* **Write Response:** `BVALID`, `BREADY`, `BRESP`
* **Read Address:** `ARVALID`, `ARREADY`, `ARADDR`
* **Read Data:** `RVALID`, `RREADY`, `RDATA`, `RRESP`

---

## 🔟 Parameters & Configurability

| Parameter    | Default | Description       |
| ------------ | ------- | ----------------- |
| `DATA_WIDTH` | 32      | AXI data width    |
| `ADDR_WIDTH` | 32      | AXI address width |

> Slave register depth is fixed to **32 entries**.

---

## 1️⃣1️⃣ Transaction / Operation Flow

### Write Operation

1. User asserts `wr_en`
2. Master sends `AWADDR`
3. Master sends `WDATA`
4. Slave responds with `BRESP`
5. `write_done` asserted

### Read Operation

1. User asserts `rd_en`
2. Master sends `ARADDR`
3. Slave returns `RDATA`
4. `read_done` asserted

---

## 1️⃣2️⃣ Timing & Clocking Details

* Single system clock (`clk`)
* All logic synchronous to rising edge
* No combinational paths across clock domains
* Latency depends on handshake readiness

---

## 1️⃣3️⃣ Simulation & Verification

Verification includes:

* Directed write transaction
* Directed read transaction
* Data coherency check
* Response correctness (OKAY)
* Busy flag behavior
* FSM sequencing validation

Waveforms generated using **VCD dumping**.

---

## 1️⃣4️⃣ Example Simulation Results

```
TXN 1 START WRITE  Addr: 0x00000010  Data: 0xAABBCCDD
TXN 1 END WRITE    Done: 1  Error: 0

TXN 1 START READ   Addr: 0x00000010
TXN 1 END READ     Data: 0xAABBCCDD  Error: 0
```

✔ Read data matches previously written value
✔ No protocol errors observed

---

## 1️⃣5️⃣ How to Run / Quick Start

### Compile & Simulate (Icarus Verilog)

```bash
iverilog -g2012 design.v top_tb.v -o sim.out
vvp sim.out
```

### View Waveform

```bash
gtkwave top_tb.vcd
```

---

## 1️⃣6️⃣ Tools Used

* Icarus Verilog – Simulation
* GTKWave / EPWave – Waveform viewing
* VS Code – Code editing
* Linux / Windows – Development environment

---

## 1️⃣7️⃣ Directory Structure

```
├── master.v        # AXI4-Lite Master
├── axi4.v          # AXI4-Lite Slave
├── top.v           # Top-level integration
├── top_tb.v        # Testbench
├── README.md
└── top_tb.vcd      # Simulation waveform
```

---

## 1️⃣8️⃣ Limitations

* Only one outstanding transaction
* No burst or pipelined transfers
* No AXI protection (PROT) signals
* Fixed slave register depth
* No randomized or coverage-driven verification

---

## 1️⃣9️⃣ Future Enhancements

* Support multiple outstanding transactions
* Add AXI interconnect (multi-slave)
* Parameterize slave register depth
* Add SLVERR and DECERR test cases
* Introduce assertions (SVA)
* Extend to AXI4-Full features
 
