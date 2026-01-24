📘 SPI Master Controller – FSM-Based RTL Implementation
1️⃣ Project Title

SPI Master Controller (Transmit-Only) – Verilog HDL FSM Design

2️⃣ Short Description (TL;DR)

A simple, FSM-based SPI Master controller implemented in Verilog HDL, capable of transmitting 16-bit data frames over MOSI using SPI Mode-0 (CPOL = 0, CPHA = 0).
The project includes a self-checking testbench that validates frame completion, bit-level timing, and continuous SPI transfers.

3️⃣ Protocol Overview

SPI (Serial Peripheral Interface) is a synchronous serial communication protocol using:

SCLK – Serial Clock (driven by master)

MOSI – Master Out, Slave In

CS / SS – Chip Select (active low)

SPI Characteristics Used in This Design

Master-driven clock

Full frame controlled by CS

Data sampled on clock edge

No addressing (point-to-point)

SPI Mode Implemented
Parameter	Value
CPOL	0 (Clock idle LOW)
CPHA	0 (Sample on rising edge)
Data Order	MSB first
4️⃣ Project Scope
Included

SPI master (TX-only)

16-bit frame transmission

FSM-controlled clock and data

Active-low chip select handling

Continuous frame transmission

Testbench with frame and bit-level monitoring

Not Included

MISO (no receive path)

Clock divider

Slave device model

Multi-slave support

Configurable SPI modes

5️⃣ Features

✔ FSM-based SPI timing control

✔ SPI Mode-0 compliant

✔ MSB-first data transmission

✔ Registered outputs (glitch-free)

✔ Continuous frame transmission

✔ Deterministic testbench logging

✔ Waveform-friendly design

6️⃣ Architecture Overview

The design consists of three main components:

Finite State Machine (FSM)

Shift Register for Data Serialization

SPI Signal Control Logic

The FSM generates:

Chip-select timing

SPI clock edges

MOSI data sequencing

7️⃣ Block Descriptions
🔹 Shift Register

Holds the 16-bit parallel input data

Feeds one bit at a time to MOSI

Loaded only when SPI is idle

🔹 Bit Counter

Tracks current bit index (15 → 0)

Controls frame length and termination

🔹 SPI Signal Registers

spi_cs_l – Active-low chip select

spi_sclk – SPI clock

spi_data – MOSI output

🔹 FSM Controller

Sequences data setup and sampling

Ensures SPI-compliant timing

8️⃣ Finite State Machine (FSM)
FSM States
State	Description
IDLE	Bus idle, CS deasserted
LOAD	Drive MOSI with current bit
CLK_H	Raise SCLK (sampling edge)
CLK_L	Lower SCLK and shift next bit
FSM Behavior

MOSI changes only when SCLK is LOW

Slave samples data on SCLK rising edge

CS asserted for exactly 16 clock cycles

9️⃣ Interface Signals
Inputs
Signal	Width	Description
clk	1	System clock
reset	1	Asynchronous reset
datain	16	Parallel data input
Outputs
Signal	Width	Description
spi_cs_l	1	Active-low chip select
spi_sclk	1	SPI serial clock
spi_data	1	MOSI
counter	5	Bit counter (debug)
🔟 Parameters & Configurability

⚠️ Current version has no parameters

Hard-coded design choices:

16-bit data width

SPI Mode-0

MSB-first transmission

System clock directly used as SPI clock

1️⃣1️⃣ Transaction / Operation Flow

FSM enters IDLE

Input data is loaded into shift register

CS asserted LOW

For each bit:

MOSI driven while SCLK LOW

SCLK raised (slave samples)

SCLK lowered

After last bit:

CS deasserted

FSM returns to IDLE

Next frame begins automatically

1️⃣2️⃣ Timing & Clocking Details

SPI clock derived directly from system clock

Each SPI bit requires two FSM cycles

Data setup occurs before SCLK rising edge

SCLK idle state = LOW

Output signals are fully registered

1️⃣3️⃣ Simulation & Verification

Verification is performed using a self-contained testbench featuring:

Frame-completion monitoring

Bit-level MOSI sampling logs

Multiple data frames

Reset behavior validation

Waveform generation for GTKWave

1️⃣4️⃣ Example Simulation Results

Observed in simulation:

Exactly 16 SCLK pulses per frame

Correct MSB-first transmission

CS asserted only during valid frames

Stable MOSI before rising edge

Correct frame separation

Example console output:

SPI FRAME 1 COMPLETED
Time      : 320 ns
Data Sent : 0x0412

1️⃣5️⃣ How to Run / Quick Start
# Compile
iverilog -o spi_sim spi_state.v test_bench.v

# Run simulation
vvp spi_sim

# View waveform
gtkwave waveform.vcd

1️⃣6️⃣ Tools Used

Verilog HDL

Icarus Verilog (iverilog)

GTKWave

Compatible with ModelSim / Vivado Simulator

1️⃣7️⃣ Directory Structure
├── spi_state.v        # SPI Master RTL
├── test_bench.v      # Verification testbench
├── waveform.vcd      # Simulation waveform
├── README.md         # Project documentation

1️⃣8️⃣ Limitations

Transmit-only (no MISO)

No clock divider

No start/enable signal

Fixed SPI mode

Single-slave support

Continuous transmission only

1️⃣9️⃣ Future Enhancements

Add MISO (full-duplex SPI)

Configurable CPOL / CPHA

Programmable data width

Clock divider for SPI frequency control

Start/enable signal

Multi-slave CS handling

Assertion-based verification (SVA) 
