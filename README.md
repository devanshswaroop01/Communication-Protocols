📡 Communication Protocols – RTL Implementations (Verilog HDL)

📌 Overview

This repository contains clean, protocol-accurate RTL implementations of commonly used digital communication and on-chip bus protocols, written in Verilog HDL and verified using simulation-driven testbenches.

Each protocol is implemented as an independent, well-documented module, with:

FSM-based control logic

Timing-correct signal behavior

Self-checking testbenches

Waveform-validated protocol compliance

The repository is intended for:

VLSI / Digital Design learning

RTL portfolio demonstration

Protocol understanding via waveforms

Interview and academic evaluation

📂 Repository Structure
Communication-Protocols/

├── AXI4-Lite-Protocol/

│   └── README.md
│

├── Advanced-Peripheral-Bus-Protocol/

│   └── README.md
│

├── Inter-Integrated-Communication-Protocol/

│   └── README.md
│

├── Serial-Peripheral-Interface-Protocol/

│   └── README.md
│

├── UART-Protocol/

│   └── README.md
│

└── README.md  


Each subfolder is a standalone project with:

RTL source code

Testbench

Waveform verification

Dedicated README

🧠 Protocols Implemented

1️⃣ AXI4-Lite Protocol

Category: Memory-Mapped On-Chip Bus

Master–slave architecture

Read & write channels

Valid/ready handshaking

Address, data, and response channels

Suitable for register access in SoCs

📁 Folder: AXI4-Lite-Protocol/


2️⃣ AMBA Advanced Peripheral Bus (APB)

Category: Low-Power Peripheral Bus

Two-phase protocol (SETUP / ACCESS)

Single master, multiple slaves

Address-based slave decoding

Read/write memory-mapped slaves

Extensive protocol verification

📁 Folder: Advanced-Peripheral-Bus-Protocol/


3️⃣ I²C (Inter-Integrated Communication)

Category: Serial, Multi-Drop Bus

Open-drain SDA/SCL signaling

START / STOP condition handling

Address + R/W bit sequencing

ACK / NACK handling

Single-byte read & write support

📁 Folder: Inter-Integrated-Communication-Protocol/


4️⃣ SPI (Serial Peripheral Interface)

Category: High-Speed Serial Interface

Master-driven clock

SPI Mode-0 (CPOL=0, CPHA=0)

MSB-first transmission

Chip-select controlled framing

Continuous multi-frame transfers

📁 Folder: Serial-Peripheral-Interface-Protocol/


5️⃣ UART (Universal Asynchronous Receiver/Transmitter)

Category: Asynchronous Serial Communication

8N1 frame format

115200 baud rate

TX & RX FSMs

16× RX oversampling

Loopback-based verification

📁 Folder: UART-Protocol/


⚙️ Design Philosophy

All implementations follow these principles:

FSM-driven control logic

Strict protocol timing compliance

Reset-safe operation

No gated clocks

Readable, modular RTL

Simulation-first verification

Waveform-proven correctness


🧪 Verification Methodology

Each protocol includes:

Directed test cases

Edge-case handling (reset, NACK, back-to-back transfers, etc.)

Self-checking testbenches

Console-based pass/fail reporting

GTKWave / EPWave waveform inspection

Verification focuses on:

Signal timing correctness

FSM sequencing

Protocol rule enforcement


🛠 Tools & Environment

Verilog HDL

Icarus Verilog (iverilog)

GTKWave / EPWave

EDA Playground

Compatible with ModelSim / Questa / Vivado Simulator


🎯 Intended Audience

This repository is useful for:

Undergraduate / postgraduate VLSI students

RTL / Digital Design learners

Interview preparation (protocol + waveform based)

Academic lab submissions

Portfolio demonstrations


📌 How to Use This Repository

Clone the repository:

git clone https://github.com/devanshswaroop01/Communication-Protocols.git


Enter any protocol directory:

cd UART-Protocol


Follow the README inside that folder to run simulations.


🚧 Future Extensions

AXI4-Full / AXI-Stream

APB-to-AXI bridge

Multi-master I²C

SPI multi-mode (CPOL/CPHA variants)

UART with FIFO & parity

Assertion-based verification (SVA)

UVM-based protocol agents


👤 Author

Devansh Swaroop

RTL & VLSI Design Enthusiast

Focused on protocol-accurate hardware design and verification 
