📘 AMBA APB Master–Slave Implementation (Verilog)

1️⃣ Project Title

AMBA APB Master–Slave System (Verilog HDL)
A protocol-compliant, deadlock-safe APB implementation with multi-slave support and verification testbench

2️⃣ Short Description (TL;DR)

This project implements a fully functional AMBA APB (Advanced Peripheral Bus) system in Verilog HDL, including a finite-state-machine based APB master, multiple APB slaves, a central interconnect, and a protocol-aware testbench.
The design strictly follows APB timing rules and demonstrates correct read/write operations, wait-state handling, and error reporting.

3️⃣ Protocol Overview

APB (Advanced Peripheral Bus) is part of the AMBA bus family and is intended for:

Low-bandwidth peripherals

Simple control and register access

Low power and low complexity

Key APB Characteristics:

Single-cycle address phase (SETUP)

Single or multi-cycle data phase (ENABLE)

No burst transactions

No pipelining

Simple handshake using PREADY and PSLVERR

This project follows APB3-style behavior.

4️⃣ Project Scope

✔ Educational and interview-grade APB implementation
✔ Demonstrates protocol correctness and clean design
✔ Suitable for:

VLSI / SoC learning

Lab assignments

RTL design interviews

APB protocol understanding

❌ Not intended as a high-performance production bus fabric

5️⃣ Features

✔ FSM-based APB master (IDLE–SETUP–ENABLE)

✔ Protocol-compliant signal timing

✔ Support for read and write transactions

✔ Two APB slaves with address-based selection

✔ Deadlock-free PREADY aggregation

✔ Proper PSLVERR handling

✔ Back-to-back transfer support

✔ Deterministic reset behavior

✔ Clean, protocol-aware testbench

✔ Waveform and console-based verification

6️⃣ Architecture Overview

The system consists of four main components:

User/Testbench

      │
      ▼
 APB Master (FSM-based)
 
      │
      ▼
 APB Interconnect (Top Module)
 
      │
 ┌────┴────┐
 ▼         ▼
 
APB Slave1 APB Slave2


The APB Master generates protocol signals

The Interconnect decodes addresses and aggregates responses

Slaves implement memory-mapped peripherals

7️⃣ Block Descriptions

🔹 APB Master

Implements APB protocol sequencing

Uses FSM to control SETUP and ENABLE phases

Latches address/control signals in SETUP

Supports wait states via PREADY

🔹 APB Interconnect (Top)

Performs slave selection based on address

Aggregates PREADY and PSLVERR

Multiplexes read data

Prevents deadlock on invalid decode

🔹 APB Slave

Implements a 256-byte memory

Responds only in ENABLE phase

Performs read/write operations

Detects invalid address accesses

🔹 Testbench

Drives valid and invalid transactions

Captures signals at true transfer completion

Displays transaction summaries

Generates waveforms for analysis

8️⃣ Finite State Machine (FSM)
FSM States:

State	Description

IDLE	No active transfer

SETUP	Address and control phase (PSEL=1, PENABLE=0)

ENABLE	Data phase (PENABLE=1, wait for PREADY)

FSM Behavior:

IDLE → SETUP on transfer request


SETUP → ENABLE unconditionally


ENABLE → IDLE or SETUP based on PREADY and new request

9️⃣ Interface Signals

Master Inputs

pclk – APB clock

presetn – Active-low reset

transfer – Transfer request

read, write – Operation type

apb_read_paddr, apb_write_paddr – Addresses

apb_write_data – Write data

APB Bus Signals

PSELx – Slave select

PENABLE – Data phase indicator

PWRITE – Read/Write control

PADDR – Address bus

PWDATA – Write data

PRDATA – Read data

PREADY – Transfer complete

PSLVERR – Error indicator

🔟 Parameters & Configurability

Current design assumptions:

Address width: 8 bits

Data width: 8 bits

Number of slaves: 2

Address map:

Slave 1: 0x00 – 0x7F

Slave 2: 0x80 – 0xFF

The design can be extended by parameterizing data width, address width, and slave count.

1️⃣1️⃣ Transaction / Operation Flow

User asserts transfer with read or write

Master enters SETUP phase and latches signals

Master enters ENABLE phase

Slave processes request

Slave asserts PREADY (and PSLVERR if needed)

Master completes transfer

Read data captured (for read transactions)

1️⃣2️⃣ Timing & Clocking Details

All logic synchronous to pclk

Reset is asynchronous active-low

APB timing strictly followed:

Signals stable from SETUP through ENABLE

PREADY sampled only in ENABLE

1️⃣3️⃣ Simulation & Verification

Verification is performed using:

Directed testbench

Timeout-protected waits

Snapshot capture at PENABLE && PREADY

Console logs for transaction summaries

Waveform inspection (VCD)

1️⃣4️⃣ Example Simulation Results

Example Console Output:

WRITE to 0x25 → SUCCESS

READ from 0x25 → Data = 0xAB

WRITE to 0x80 → PSLVERR asserted


Waveform Confirms:

Correct SETUP/ENABLE sequencing

Stable signals during ENABLE

Proper error handling without deadlock

1️⃣5️⃣ How to Run / Quick Start

# Compile
iverilog -o apb_tb *.v

# Run simulation
vvp apb_tb

# View waveform
gtkwave waveform.vcd

1️⃣6️⃣ Tools Used

Verilog HDL

Icarus Verilog (iverilog)

GTKWave

(Optional) EPWave / ModelSim / Vivado Simulator

1️⃣7️⃣ Directory Structure

├── APB_master.v

├── APB_slave.v

├── APB_top.v

├── testbench.v

├── waveform.vcd

└── README.md

1️⃣8️⃣ Limitations

Single outstanding transaction

Fixed address map

APB3-style only (no APB4 features)

No burst or pipelining

Slaves respond with fixed latency

No assertion-based verification

1️⃣9️⃣ Future Enhancements

Parameterized number of slaves

APB4 feature support (PSTRB, PPROT)

Configurable data/address width

Assertion-based verification (SVA)

Randomized and coverage-driven testing

Power-aware enhancements (clock gating) 
