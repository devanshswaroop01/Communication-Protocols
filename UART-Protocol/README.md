📘 UART Controller – Verilog HDL Implementation

1️⃣ Project Title

UART Transmitter & Receiver (115200 Baud, 8N1) – Verilog HDL

2️⃣ Short Description (TL;DR)

A fully functional UART controller implemented in Verilog HDL, supporting 8-bit data, no parity, 1 stop bit (8N1) at 115200 baud using a 50 MHz system clock.
The design includes a baud rate generator, reset-safe transmitter, and a robust receiver with 16× oversampling, along with a self-checking loopback testbench.

3️⃣ Protocol Overview

UART (Universal Asynchronous Receiver/Transmitter) is an asynchronous serial communication protocol that uses:

No shared clock between transmitter and receiver

Start and stop bits for synchronization

Fixed baud rate on both ends

UART Configuration Used
Parameter	Value
System Clock	50 MHz
Baud Rate	115200
Data Bits	8
Parity	None
Stop Bits	1
Frame Format	8N1
RX Oversampling	16×
UART Frame Format
Idle (1) → Start (0) → D0 → D1 → D2 → D3 → D4 → D5 → D6 → D7 → Stop (1)

4️⃣ Project Scope
Included

Baud rate generator (TX and RX)

UART transmitter (TX)

UART receiver (RX)

16× RX oversampling for noise tolerance

Reset-safe FSM design

Ready/Busy handshaking

Loopback testbench with self-checking

Not Included

Parity generation/checking

Framing error detection

FIFO buffering

Flow control (RTS/CTS)

Multi-baud runtime selection

5️⃣ Features

✔ Standard-compliant UART (8N1)

✔ Accurate baud generation from 50 MHz clock

✔ 16× RX oversampling for reliable reception

✔ Reset-safe FSMs

✔ Clean TX busy signaling

✔ Explicit RX ready clear handshake

✔ Modular and readable RTL

✔ Comprehensive self-checking testbench

6️⃣ Architecture Overview

The UART design is divided into four main blocks:

Baud Rate Generator

UART Transmitter

UART Receiver

Top-Level Integration Module

All blocks operate in a single clock domain (50 MHz) and communicate using clock-enable pulses instead of gated clocks.

7️⃣ Block Descriptions
🔹 Baud Rate Generator

Divides 50 MHz clock to generate:

TX baud tick (1× baud)

RX oversampling tick (16× baud)

Uses counters and clock-enable pulses

Parameterized for baud calculation

🔹 Transmitter (TX)

FSM-based UART transmitter

Latches data on wr_en

Sends:

Start bit

8 data bits (LSB first)

Stop bit

Provides tx_busy to prevent overwrite

🔹 Receiver (RX)

FSM-based UART receiver

Uses 16× oversampling

Samples data at bit center

Handles minor baud mismatch

Provides rdy flag with explicit clear (rdy_clr)

🔹 Top-Level UART Module

Instantiates baud generator, TX, and RX

Provides a clean external interface

Suitable for SoC or FPGA integration

8️⃣ Finite State Machine (FSM)
Transmitter FSM States
State	Description
IDLE	Line idle, waiting for write
START	Transmit start bit
DATA	Transmit 8 data bits
STOP	Transmit stop bit
Receiver FSM States
State	Description
START	Detect start bit and align sampling
DATA	Sample 8 data bits
STOP	Validate stop bit and complete frame
9️⃣ Interface Signals
Top-Level UART Ports
Inputs
Signal	Width	Description
clk_50m	1	50 MHz system clock
rst	1	Asynchronous reset
din	8	Transmit data
wr_en	1	Transmit request
rx	1	UART RX input
rdy_clr	1	Clear RX ready flag
Outputs
Signal	Width	Description
tx	1	UART TX output
tx_busy	1	Transmitter busy flag
dout	8	Received data
rdy	1	RX data valid
🔟 Parameters & Configurability
Baud Generator Parameters

RX_ACC_MAX – Divider for RX oversampling

TX_ACC_MAX – Divider for TX baud

Counter widths derived using $clog2

⚠️ Current design uses fixed baud rate (115200) and fixed clock (50 MHz) at compile time.

1️⃣1️⃣ Transaction / Operation Flow
Transmit Operation

User asserts wr_en with valid din

TX latches data and asserts tx_busy

Start bit transmitted

8 data bits transmitted (LSB first)

Stop bit transmitted

TX returns to idle

Receive Operation

RX detects falling edge (start bit)

Oversampling aligns to bit center

8 data bits sampled

Stop bit validated

rdy asserted with valid dout

User clears rdy using rdy_clr

1️⃣2️⃣ Timing & Clocking Details

Single 50 MHz system clock

Baud generator creates clock-enable pulses

TX uses 1× baud enable

RX uses 16× baud enable

Data sampling occurs at bit center

All logic fully synchronous

1️⃣3️⃣ Simulation & Verification

Verification is done using a self-checking testbench featuring:

TX-to-RX loopback

Automatic data comparison

Timeout protection

Reset validation

Multiple test patterns

1️⃣4️⃣ Example Simulation Results

Console output example:

✅ RX OK: 55
✅ RX OK: 00
✅ RX OK: FF
🎉 ALL UART TESTS PASSED 🎉


Waveforms show:

Correct start/stop bit timing

Proper baud spacing

Stable sampling at RX bit centers

1️⃣5️⃣ How to Run / Quick Start
# Compile
iverilog -o uart_sim uart.v uart_tb.v

# Run simulation
vvp uart_sim

# View waveforms
gtkwave uart_tb.vcd

1️⃣6️⃣ Tools Used

Verilog HDL

Icarus Verilog (iverilog)

GTKWave

Compatible with ModelSim, Questa, Vivado Simulator

1️⃣7️⃣ Directory Structure
├── baud_rate_gen.v     # Baud rate generator
├── transmitter.v       # UART transmitter
├── receiver.v          # UART receiver
├── uart.v              # Top-level UART
├── uart_tb.v           # Self-checking testbench
├── uart_tb.vcd         # Waveform output
├── README.md           # Documentation

1️⃣8️⃣ Limitations

Fixed baud rate and clock frequency

No parity or framing error detection

No RX/TX FIFO buffering

No flow control (RTS/CTS)

Single UART instance only

1️⃣9️⃣ Future Enhancements

Add parity support (even/odd)

Add framing and overrun error flags

Parameterize baud rate and clock

Add RX and TX FIFOs

AXI-Lite / APB UART wrapper

Interrupt generation

Multi-UART support 
