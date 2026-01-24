
📘 I²C Master Controller (RTL Implementation)
1️⃣ Project Title

I²C Master Controller – RTL Design & Verification (Verilog HDL)

2️⃣ Short Description (TL;DR)

A clean, FSM-based I²C Master controller implemented in Verilog HDL, supporting single-byte read and write transactions using a fully open-drain SDA/SCL bus model.
The project includes a deterministic behavioral I²C slave testbench for protocol-accurate simulation and verification.

3️⃣ Protocol Overview

I²C (Inter-Integrated Circuit) is a synchronous, serial, multi-master, multi-slave communication protocol using:

SDA – Serial Data (open-drain)

SCL – Serial Clock (open-drain)

Key Protocol Characteristics:

7-bit addressing + 1 R/W bit

START and STOP conditions

ACK/NACK handshaking

Data valid when SCL is HIGH

Data changes only when SCL is LOW

This project implements a single-master I²C controller operating in Standard Mode (100 kHz).

4️⃣ Project Scope
Included:

I²C Master RTL

Address + R/W transmission

Single-byte write

Single-byte read

START, ACK, NACK, STOP handling

Open-drain bus modeling

Protocol-accurate testbench

Not Included:

Multi-byte burst transfers

Clock stretching

Multi-master arbitration

Repeated START conditions

5️⃣ Features

✔ FSM-based I²C control logic

✔ Parameterized clock frequency

✔ Open-drain SDA/SCL modeling

✔ Single-byte READ and WRITE support

✔ Clean START / STOP generation

✔ Deterministic slave model for verification

✔ Reset-safe and transaction-safe design

✔ Readable, modular RTL style

6️⃣ Architecture Overview

The design consists of three major blocks:

Clock Divider

I²C Bus Interface (Open-Drain)

FSM-Controlled Protocol Engine

The controller converts a system clock into an I²C-compliant SCL, while the FSM sequences SDA behavior according to protocol timing rules.

7️⃣ Block Descriptions
🔹 Clock Divider

Converts SYS_CLK_HZ to desired I2C_CLK_HZ

Generates internal SCL toggle request

Ensures symmetric HIGH and LOW phases

🔹 Open-Drain Interface

SDA and SCL driven LOW only

Released to HIGH via pull-ups

Accurately models real I²C electrical behavior

🔹 Protocol FSM

Controls SDA output enable

Tracks bit position and transaction state

Handles ACK/NACK sampling and generation

8️⃣ Finite State Machine (FSM)
FSM States:

IDLE – Wait for transaction start

START – Generate START condition

SEND_ADDR – Send address + R/W bit

ADDR_ACK – Sample slave ACK/NACK

SEND_DATA – Send write data byte

DATA_ACK – Sample data ACK

READ_DATA – Receive data from slave

MASTER_NACK – Master NACK after read

STOP – Generate STOP condition

DONE – Transaction completion pulse

The FSM strictly follows I²C timing rules:

SDA changes only when SCL is LOW

SDA sampled only when SCL is HIGH

9️⃣ Interface Signals
Inputs
Signal	Width	Description
clk	1	System clock
rst	1	Asynchronous reset
start	1	Start transaction
rw	1	Read (1) / Write (0)
slave_addr	7	I²C slave address
wdata	8	Write data
Outputs
Signal	Width	Description
rdata	8	Read data
busy	1	Transaction in progress
done	1	One-cycle completion pulse
Bidirectional
Signal	Description
sda	Serial Data (open-drain)
scl	Serial Clock (open-drain)
🔟 Parameters & Configurability
Parameter	Description
SYS_CLK_HZ	System clock frequency
I2C_CLK_HZ	Desired I²C clock frequency

Example:

.SYS_CLK_HZ(1_000_000),
.I2C_CLK_HZ(100_000)

1️⃣1️⃣ Transaction / Operation Flow
Write Transaction

START condition

Send address + WRITE bit

Sample ACK

Send data byte

Sample ACK

STOP condition

Read Transaction

START condition

Send address + READ bit

Sample ACK

Read data byte

Master sends NACK

STOP condition

1️⃣2️⃣ Timing & Clocking Details

SCL derived from system clock via divider

SDA driven only during SCL LOW

SDA sampled during SCL HIGH

START and STOP generated while SCL HIGH

Fully compliant with I²C Standard Mode timing

1️⃣3️⃣ Simulation & Verification

Verification is performed using:

Behavioral slave model

Protocol-accurate ACK/NACK handling

Edge-aligned data sampling

Reset and error scenarios

Testbench Covers:

Single-byte write

Single-byte read

Reset mid-transaction

Slave NACK handling

1️⃣4️⃣ Example Simulation Results

Expected observations in waveform:

Correct START and STOP timing

SDA stable during SCL HIGH

Proper ACK cycles

Correct read data captured

done asserted for one cycle after STOP

1️⃣5️⃣ How to Run / Quick Start
# Compile
iverilog -o iic_sim iic_master.v iic_master_tb.v

# Run
vvp iic_sim

# View waveform
gtkwave iic_master_final_tb.vcd

1️⃣6️⃣ Tools Used

Verilog HDL

Icarus Verilog (iverilog)

GTKWave

Any standard RTL simulator (ModelSim, Vivado, etc.)

1️⃣7️⃣ Directory Structure
├── iic_master.v        # I²C Master RTL
├── iic_master_tb.v     # Testbench with slave model
├── README.md           # Project documentation
├── iic_master.vcd      # Simulation waveform (generated)

1️⃣8️⃣ Limitations

Single-byte transactions only

No clock stretching support

No repeated START

No multi-master arbitration

No noise/glitch filtering

1️⃣9️⃣ Future Enhancements

Multi-byte burst read/write

Repeated START support

Clock stretching detection

Arbitration loss handling

APB / AXI-Lite wrapper

FPGA synthesis & timing constraints

Assertion-based verification (SVA)
