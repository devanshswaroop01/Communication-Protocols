📘 AMBA-APB Master–Slave Subsystem (RTL + Verification)
1️⃣ Project Title

AMBA APB Master–Slave Subsystem with Dual Slaves and Verification Testbench

2️⃣ Short Description (TL;DR)

A fully functional AMBA-APB (Advanced Peripheral Bus) subsystem implemented in Verilog HDL, consisting of a single APB master, two memory-mapped APB slaves, and a comprehensive, protocol-aware testbench.
The design correctly implements the two-phase APB protocol (SETUP + ACCESS), supports read/write transactions, back-to-back transfers, reset safety, and includes extensive functional and protocol verification.

3️⃣ Protocol Overview

APB (Advanced Peripheral Bus) is part of the ARM AMBA architecture, designed for low-power, low-bandwidth peripherals.

Key APB Characteristics

Single-cycle address phase (SETUP)

Single-cycle or multi-cycle data phase (ACCESS)

No pipelining

No burst transfers

Simple control and low power

APB Phases

IDLE – No transfer

SETUP – Address and control asserted

ACCESS – Data transfer, controlled by PENABLE and PREADY

4️⃣ Project Scope
Included

APB Master FSM (IDLE / SETUP / ACCESS)

Two APB slaves with independent memory

Address-based slave decoding

Read and write transactions

Back-to-back transfer support

PSLVERR handling (slave side)

Reset-safe design

Comprehensive self-checking testbench

Protocol monitors and functional coverage

Not Included

APB bridges (APB-to-AXI/AHB)

Burst or pipelined transfers

Dynamic slave discovery

Low-power clock gating

5️⃣ Features

✔ Fully APB-compliant two-phase protocol

✔ Clean FSM-based APB master

✔ Dual slave support with address decoding

✔ Independent memory per slave

✔ Back-to-back transfer support

✔ Reset-safe operation

✔ Read and write support

✔ Protocol monitors for rule checking

✔ Functional coverage collection

✔ Extensive verification test suite

6️⃣ Architecture Overview

The subsystem consists of:

APB Master

Two APB Slaves

APB Interconnect (implicit decoding)

Verification Testbench

The master drives the APB bus, selects one slave at a time using address decoding, and completes transactions using the standard APB handshake.

7️⃣ Block Descriptions
🔹 APB Master

Implements APB FSM (IDLE → SETUP → ACCESS)

Captures address, control, and write data in SETUP

Asserts PENABLE during ACCESS

Waits for PREADY to complete transfer

Supports back-to-back transfers

Decodes address bit [8] to select slave

🔹 APB Slaves

Two independent slaves

Each slave contains:

256 × 8-bit memory

Read and write logic

PREADY generation

PSLVERR signaling

Respond only when selected (PSEL && PENABLE)

🔹 APB Testbench

Drives APB master using transaction-level tasks

Verifies:

Protocol correctness

Address stability

Slave selection

Reset behavior

Back-to-back transfers

Includes monitors, assertions, and coverage counters

8️⃣ Finite State Machine (FSM)
APB Master FSM States
State	Description
S_IDLE	No active transfer
S_SETUP	Address & control phase
S_ACCESS	Data phase (wait for PREADY)
FSM Rules Enforced

PSEL asserted in SETUP and ACCESS

PENABLE asserted only in ACCESS

Address and control stable during SETUP + ACCESS

Transfer completes only when PREADY = 1

9️⃣ Interface Signals
APB Master Inputs
Signal	Width	Description
pclk	1	APB clock
presetn	1	Active-low reset
transfer	1	Transfer request (1-cycle pulse)
write_enable	1	Write = 1, Read = 0
apb_write_paddr	9	Write address
apb_write_data	8	Write data
apb_read_paddr	9	Read address
pready	1	Slave ready
pslverr	1	Slave error
prdata	8	Read data from slave
APB Master Outputs
Signal	Width	Description
psel1	1	Slave-1 select
psel2	1	Slave-2 select
penable	1	ACCESS phase indicator
paddr	9	APB address
pwrite	1	Write control
pwdata	8	Write data
apb_read_data_out	8	Captured read data
🔟 Parameters & Configurability
Slave Parameters

SLAVE_ID – Identifies slave instance for debug

Fixed Design Choices

Address width: 9 bits

Data width: 8 bits

Two slaves selected via PADDR[8]

1️⃣1️⃣ Transaction / Operation Flow
Write Transaction

Master detects transfer

SETUP phase:

PSELx = 1

PADDR, PWRITE, PWDATA valid

ACCESS phase:

PENABLE = 1

Slave asserts PREADY

Write completes

Bus returns to IDLE or next SETUP

Read Transaction

Master enters SETUP with read address

ACCESS phase asserted

Slave drives PRDATA

Master captures data when PREADY = 1

Transfer completes

1️⃣2️⃣ Timing & Clocking Details

Single clock domain (PCLK)

All signals synchronous to PCLK

Address and control stable during SETUP + ACCESS

PENABLE asserted one cycle after PSEL

PREADY can extend ACCESS phase (wait states supported)

1️⃣3️⃣ Simulation & Verification

The verification environment includes:

Transaction-level APB write/read tasks

Timeout protection for deadlock detection

Protocol monitors checking:

PENABLE without PSEL

Address stability

One-hot slave selection

Reset verification

Functional coverage counters

1️⃣4️⃣ Example Simulation Results

Example console output:

PASS: Read data matches expected 0xAA
PASS: Slave 1 correctly selected
PASS: Both SETUP and ACCESS phases detected
ALL TESTS COMPLETED


Final coverage summary:

Total Write Operations:  XX
Total Read Operations:   XX
Slave 1 Accesses:        XX
Slave 2 Accesses:        XX

1️⃣5️⃣ How to Run / Quick Start
# Compile
iverilog -o apb_sim APB_master.v APB_slave.v APB_top.v apb_testbench.v

# Run simulation
vvp apb_sim

# View waveforms
gtkwave apb_testbench.vcd

1️⃣6️⃣ Tools Used

Verilog HDL

Icarus Verilog (iverilog)

GTKWave

Compatible with ModelSim / Questa / Vivado Simulator

1️⃣7️⃣ Directory Structure
├── APB_master.v        # APB master FSM
├── APB_slave.v         # Generic APB slave
├── APB_top.v           # Master + slaves integration
├── apb_testbench.v     # Verification environment
├── apb_testbench.vcd   # Simulation waveforms
├── README.md           # Documentation

1️⃣8️⃣ Limitations

Fixed address decoding scheme

No dynamic slave configuration

No APB bridge to AXI/AHB

No power management features

Single master only (by APB definition)

1️⃣9️⃣ Future Enhancements

APB-to-AXI/AHB bridge

Configurable number of slaves

Wait-state configurable slaves

Assertion-based verification (SVA)

UVM-based APB agent

Register abstraction layer (RAL)

Low-power clock gating support 
