# Week 9: Final Documentation Submission 
 
Week 9 completes the VSDBabySoC journey by consolidating the full design flow into a final, polished repository. This stage brings together every prior step — from architecture and RTL to synthesis, placement, routing, SPEF extraction, and multi-corner STA — into a unified deliverable. The comprehensive documentation captures each milestone, experiment, and timing insight, preparing the design for sign-off and future extension.

---

## 📜 Table of Contents
[📋 Prerequisites](#-prerequisites) <br>
### <ins>Week 2</ins><br>
[1. Introduction to RISC-V](#1-introduction-to-risc-v)<br>
[2. Introduction to VSDBabySoC](#2-introduction-to-vsdbabysoc)<br>
[3. Components of VSDBabySoC](#3-components-of-vsdbabysoc)<br>
[4. Functional Modelling of VSDBabySoC](#4-functional-modelling-of-vsdbabysoc)<br>
### <ins>Week 3</ins><br>
[5. Gate-Level Simulation (GLS) of VSDBabySoC](#5-gate-level-simulation-gls-of-vsdbabysoc)<br>
### <ins>Week 7</ins><br>
[6. Overview of ORFS](#6-overview-of-orfs)<br>
[7. Environment Setup and File Organization](#7-environment-setup-and-file-organization)<br>
[8. Synthesis](#8-synthesis)<br>
[9. Floorplan](#9-floorplan)<br>
[10. Placement](#10-placement)<br>
[11. Clock Tree Synthesis (CTS)](#11-clock-tree-synthesis-cts)<br>
[12. Routing](#12-routing)<br>
[13. Post-Route SPEF Generation](#13-post-route-spef-generation)<br>
### <ins>Week 8</ins><br>
[14. Required Files for Post-SPEF STA](#14-required-files-for-post-spef-sta)<br>
[15. Procedure for Post-SPEF STA](#15-procedure-for-post-spef-sta)<br>
[16. Results of Post-SPEF STA](#16-results-of-post-spef-sta)<br>
[17. Comparison of Post-SPEF STA With Post-Synthesis STA Reports](#17-comparison-of-post-spef-sta-with-post-synthesis-sta-reports)<br>
[18. Interpretations and Discussions](#18-interpretations-and-discussions)<br><br>
[My Unique Contributions](#my-unique-contributions)<br>
[⚠️ Challenges](#%EF%B8%8F-challenges)<br>

---

## 📋 Prerequisites
- Successful completion of the entire flow of VSDBabySoC till Week 8.

---

## 1. Introduction to RISC-V
### <ins>1. What is RISC-V?</ins><br>
  RISC-V *(pronounced “risk-five”)* is an open-source Instruction Set Architecture (ISA). RISC stands for Reduced Instruction Set Computing, while V represents the fifth generation of the Reduced Instruction Set Computing (RISC) architectures developed at the University of California, Berkeley.

  <div align="center">
    <img src="Images/2.jfif" alt="Alt Text" width="600"/>
  </div>
  
  Key Features of RISC-V:
  * Open-Source
    * Unlike proprietary ISAs such as x86 (Intel/AMD) or ARM, RISC-V is free and open. Anyone can design a CPU core using RISC-V without paying licensing fees.
  * RISC Philosophy (Reduced Instruction Set Computer)
    * The design focuses on having a small set of simple instructions that can execute quickly.
    * Simplicity makes it easier to design efficient cores, especially for education, research, and startups.
  * Modular Design
    * RISC-V has a base instruction set (for basic integer operations).
    * Additional extensions can be added depending on the application, like floating-point math, atomic operations, or vector processing.
  * Scalability
    * Can be used to design anything from tiny microcontrollers to powerful high-performance CPUs.

### <ins>2. Why RISC-V gained so much popularity?</ins><br>
  RISC-V has gained tremendous attention in both academia and industry in recent years. Its popularity comes from a mix of technical strengths and strategic advantages that set it apart from older ISAs like x86 and ARM.
  - Open and Royalty-Free:
    * Anyone can use the RISC-V ISA without paying license fees.
    * This makes it attractive for universities, startups, and developing countries, where cost is a major barrier.
    * In contrast, ARM cores require licensing fees, and x86 is locked to Intel/AMD.
  - Simplicity and Clean Design:
    * RISC-V follows the RISC philosophy → a small, elegant set of instructions.
    * Easier to learn for students, easier to implement for chip designers.
    * Avoids decades of legacy baggage that makes x86 very complex.
  - Modularity and Extensibility:
    * RISC-V has a modular ISA:
      * Base ISA (RV32I, RV64I, RV128I).
      * Optional extensions (floating-point, atomic ops, vector instructions, compressed instructions, etc.).
    * Designers can pick and choose what they need → from tiny IoT chips to high-performance AI processors.
  - Ecosystem and Community Growth:
    * Huge support from universities, open-source communities, and big tech companies (like Google, NVIDIA, Western Digital, Alibaba).
    * Growing number of open-source cores, tools, and compilers.
    * This collective momentum accelerates innovation.
  - Freedom from Vendor Lock-In:
    * With ARM or x86, chip makers depend on Intel/AMD or ARM Holdings.
    * RISC-V gives complete design freedom: companies can customize cores without legal or licensing restrictions.
  - Educational Value:
    * Because it’s simple and open, RISC-V is being adopted worldwide in academic programs.
    * Students and researchers can actually study and modify the architecture, which was nearly impossible with proprietary ISAs.
    * RVMYTH in BabySoC is a direct example of this use in education.
  - Future-Proof:
    * Scales from 32-bit microcontrollers (RV32) to 64-bit desktop/server CPUs (RV64) and even 128-bit experimental processors (RV128).
    * Supports modern computing needs like vector extensions for AI/ML.

*Think of RISC-V as the Linux of hardware: free, open, flexible, and rapidly growing. Just like Linux took over servers and Android, RISC-V is on its way to becoming the foundation for a wide range of processors.*

### <ins>3. Base ISA and Extensions</ins><br>
  One of the most important features of RISC-V is its modular design. Instead of defining one giant, fixed instruction set, RISC-V has a small base ISA and then allows extensions to be added as needed. This keeps it flexible and scalable for different applications.
  - The Base ISA:
    * The base instruction set provides the minimal instructions needed for a processor to function.
    * It defines basic operations like:
      * Integer arithmetic (add, subtract).
      * Logical operations (AND, OR, XOR).
      * Data movement (load, store).
      * Branching (jumps, conditional branches).
    * Variants of Base ISA:
      * **RV32I** → Base integer instruction set for 32-bit processors.
      * **RV64I** → Base instruction set for 64-bit processors.
      * **RV128I** → Experimental 128-bit processors.

        *The “I” stands for Integer, meaning it only supports integer operations.*
  - Extensions:<br>
    Beyond the base ISA, RISC-V provides extensions that can be added depending on application needs.
    * Some common extensions:
      * **M (Multiply/Divide)** → Adds hardware support for multiplication and division.
      * **A (Atomic)** → Adds atomic memory operations (important for multi-core processors).
      * **F (Floating-Point, single-precision)** → For handling decimal numbers (e.g., 1.25).
      * **D (Floating-Point, double-precision)** → Higher precision floating-point.
      * **C (Compressed)** → Reduces instruction size (16-bit encoding), useful in embedded systems.
      * **V (Vector)** → Adds vector processing for AI, graphics, and scientific computing.
  - Modularity in Practice:
    * A small IoT device might only use RV32I + C (integer + compressed instructions) → lightweight and energy-efficient.
    * A supercomputer CPU might use RV64I + MAFDV (integer + multiply/divide, atomics, floating-point, vector).

      This flexibility is why RISC-V can scale from tiny microcontrollers to high-performance processors.

*Think of RISC-V like a customizable pizza base. The base ISA is the plain crust (you can eat it as-is). The extensions are toppings — you add cheese, veggies, or extra sauce depending on your taste (application).*

### <ins>4. Assembly Programming in RISC-V</ins><br>
  Every processor executes machine instructions — binary patterns of 0s and 1s. Writing programs directly in binary would be impractical for humans, so processors provide an assembly language: a human-readable representation of machine instructions. <br>For RISC-V, assembly programming means writing instructions that directly correspond to the RISC-V ISA.
  - What is Assembly Language?
    * Assembly is a low-level programming language where each instruction maps almost one-to-one with a CPU instruction.
    * Instead of writing binary like `0000000001010001`, you write instructions such as `ADD x5, x6, x7`. This means add the contents of registers x6 and x7, and store the result in x5.
  - Registers in RISC-V
    * RISC-V provides a set of general-purpose registers (32 registers, each 32 bits wide in RV32).
    * These are named `x0` to `x31`.
    * Each has a role or convention (for example, `x0` is always zero, `x1` is typically used for return addresses, etc.).
  - Common Instruction Categories

    RISC-V assembly has a small but powerful set of instructions:
    * Arithmetic and Logical Instructions
      * `ADD`, `SUB`, `AND`, `OR`, `XOR`
    * Data Transfer Instructions
      * `LW` (load word from memory)
      * `SW` (store word to memory)
    * Control Flow Instructions
      * `BEQ` (branch if equal)
      * `BNE` (branch if not equal)
      * `JAL` (jump and link)
  - Program Execution Flow
    * Write assembly code (e.g., `ADD x5, x6, x7`).
    * Assembler translates it into machine code (binary instructions).
    * The CPU fetches and decodes these instructions.
    * The core executes them using its datapath (ALU, registers, memory).

---

## 2. Introduction to VSDBabySoC

### <ins>1. What is VSDBabySoC?</ins>
VSDBabySoC is a miniature System-on-Chip (SoC) developed as part of the VLSI System Design (VSD) program. It is an educational SoC project, created with the purpose of helping students, researchers, and enthusiasts understand how different hardware components integrate together on a single chip.

Unlike complex commercial SoCs (such as those in smartphones or laptops), VSDBabySoC is a scaled-down, simplified design that still demonstrates the essential principles of SoC design.

<div align="center">
  <img src="Images/1.png" alt="Alt Text" width="600"/>
</div>

### <ins>2. Primary Objectives of VSDBabySoC</ins><br>
1. **Integration of Multiple IP Cores**<br>
   VSDBabySoC was designed to test the integration of three open-source IP cores:
   * RVMYTH (RISC-V based CPU core)
   * PLL (Phase-Locked Loop for clock generation)
   * UART (Universal Asynchronous Receiver/Transmitter for serial communication)
2. **Analog Component Calibration**<br>
   Includes a DAC (Digital-to-Analog Converter) that generates analog waveforms from digital values produced by the CPU. This allows testing of mixed-signal behavior (digital + analog).
3. **Educational Demonstration**<br>
   Provides hands-on exposure to:
   * CPU instruction execution (via RVMYTH).
   * On-chip communication (via UART).
   * Clock generation and management (via PLL).
   * Digital-to-analog conversion (via DAC).

### <ins>3. Architecture Overview of VSDBabySoC</ins><br>
The architecture of VSDBabySoC is designed to be simple yet representative of real-world SoCs. It integrates digital, analog, and communication blocks on a single chip, demonstrating the fundamental principles of system integration.<br>At a high level, the SoC consists of:
1. **RVMYTH CPU Core – The brain of the system.**
   * Executes instructions and controls the overall system.
   * Stores values in registers and sends them to peripherals like the DAC.
2. **PLL (Phase Locked Loop) – Clock generator and frequency stabilizer.**
   * Provides a stable, high-frequency clock signal.
   * Ensures that the CPU, UART, and DAC operate in synchrony.
   * Without the PLL, the chip would lack timing precision.
3. **UART (Universal Asynchronous Receiver/Transmitter) – Communication interface.**
   * Connects the SoC to the outside world.
   * Used for debugging, data transfer, and receiving instructions.
   * Enables programmers to communicate with the chip via a serial terminal.
4. **DAC (Digital-to-Analog Converter) – For analog waveform generation.**
   * Converts digital output values from the CPU into analog signals.
   * Demonstrates mixed-signal integration.

### <ins>4. Interaction Between Components</ins><br>
- The PLL generates the clock → drives the CPU, UART, and DAC.
- The CPU (RVMYTH) executes instructions → produces data (e.g., waveform values).
- These values are sent to the DAC, which converts them to analog signals.
- Simultaneously, the UART allows external devices (like a PC) to communicate with the CPU → sending/receiving data for testing or control.

---

## 3. Components of VSDBabySoC.
### <ins>1. RVMYTH CPU Core</ins><br>
The RVMYTH CPU core is the central processing unit of the VSDBabySoC. It is a RISC-V based, single-cycle processor designed for simplicity and educational purposes. Despite its small size, it demonstrates all the essential aspects of a modern CPU core—fetching, decoding, and executing instructions, handling registers, and interacting with peripherals.<br>RVMYTH not only powers the logic inside VSDBabySoC but also showcases the fundamental principles of processor design in the RISC-V ecosystem.
- **What is RVMYTH?**
  * RVMYTH is a minimalistic RISC-V CPU core.
  * It is based on the RV32I instruction set architecture (ISA), which supports 32-bit instructions and registers.
- **Single-Cycle Design and Purpose**<br>
  The RVMYTH CPU core is built as a single-cycle processor. This means that each instruction is fetched, decoded, and executed within one clock cycle.
  * How it works:
    * On every rising edge of the clock (from the PLL), the CPU completes one full instruction cycle.
    * This makes the design easy to understand and debug, as there are no complex pipelining or hazards.
  * Purpose of single-cycle design:
    * Simplicity: Ideal for learning and demonstration.
    * Deterministic behavior: One clock → one instruction, so execution timing is straightforward.
    * Tradeoff: It’s slower than pipelined processors, but speed is not the main goal here. The goal is clarity and educational value.
- **Register Usage**<br>
The CPU core contains a set of general-purpose registers (GPRs), typically 32 registers (`r0` to `r31`) as defined by the RISC-V ISA. Each register can hold 32-bit data, which the CPU uses for computation and communication with peripherals.
  * **Special focus: Register `r17`**
    * In VSDBabySoC, the `r17` register plays a unique role.
    * The CPU loads digital values (e.g., waveform samples, numeric data) into `r17`.
    * These values are then sent to the DAC (Digital-to-Analog Converter). This makes the design predictable: whenever `r17` is updated, the DAC knows that a new data value is ready for conversion.
    * The DAC converts them into a continuous analog signal, such as a sine wave or audio signal.

### <ins>2. PLL – Phase-Locked Loop</ins><br>
The Phase-Locked Loop (PLL) in VSDBabySoC is one of the three primary IP cores integrated into the design. While the CPU and DAC handle digital computation and analog conversion, the PLL ensures that everything operates in perfect timing by generating the necessary clock signals. In essence, without a stable clock, the SoC cannot function reliably. The PLL acts as the timekeeper of the system.
- **What is PLL?**<br>
  A Phase-Locked Loop (PLL) is an electronic circuit that:
  1. Takes an input clock signal (from an external crystal oscillator, for example).
  2. Multiplies or divides its frequency.
  3. Outputs a new, stable clock signal that is synchronized (phase-locked) to the input clock.
- **Why Clock Generation/Division is Required?**<br>
  Digital circuits (like the RVMYTH CPU) and analog circuits (like the DAC) need clocks to work, but each has different frequency requirements.
  * CPU: Needs a relatively fast clock (tens of MHz) to execute instructions efficiently.
  * UART: Needs a slower clock (like a few kHz or MHz) to match data communication speed.
  * DAC: May require a steady sampling clock to convert digital values smoothly into analog signals.
  Instead of having multiple external clock sources, the SoC uses:
  * One master clock input (like 10 MHz).
  * A PLL that generates higher frequencies (like 80 MHz).
  * Clock dividers to create the slower required frequencies for UART, DAC, or memory.
- **The 8× PLL Used in VSDBabySoC**<br>
  * **Meaning of 8× PLL:**<br>
    If the input clock is f_in, the PLL outputs f_out = 8 × f_in.
  * **Why 8× specifically?**<br>
    * It gives the CPU (RVMYTH) a fast enough clock to demonstrate meaningful execution speed.
    * The higher-frequency clock can then be divided down to slower clocks for the DAC and UART.

### <ins>3. UART – Universal Asynchronous Receiver-Transmitter</ins><br>
The UART (Universal Asynchronous Receiver-Transmitter) is one of the most widely used communication interfaces in digital systems. In VSDBabySoC, the UART core serves as the primary communication bridge between the SoC and the external world (like a PC).
- **What is UART?**<br>
  * A UART is a hardware module used for serial communication.
  * It sends and receives data one bit at a time over a single wire (TX for transmit, RX for receive), unlike parallel communication where multiple bits travel together.
  * “Asynchronous” means it doesn’t share a clock line with the receiver—both sides just agree on a baud rate (bits per second).
  * Key Features:
    * Simplicity: only needs 2 lines (TX, RX).
    * Widely supported: found in almost every microcontroller, SoC, or PC.
    * Useful for low-to-medium speed communication (commonly 9600, 115200 baud, etc.).
- **Role in Communication – Debugging, Data Transfer**<br>
  The UART plays two critical roles in SoCs like BabySoC:
  * Debugging:
    * When developing and testing the CPU (RVMYTH), designers need to know:
      * Is the CPU running instructions correctly?
      * What are the register values or memory contents?
    * UART provides a simple way to “print out” this information to a PC terminal.
  * Data Transfer:
    * Apart from debugging, UART can be used to send/receive real data between the SoC and external devices.
- **Integration in SoC**<br>
  In VSDBabySoC, the UART is tightly integrated as one of the three main IP cores (alongside RVMYTH and PLL).
  * **How it fits in:**
    1. The CPU (RVMYTH) generates digital data (instructions, register values, or processed outputs).
    2. This data can be sent to the UART module, which converts it into a serial bitstream.
    3. The serial data is sent out on the TX pin to an external device like a PC.
    4. Likewise, external inputs (commands/data) can be received via the RX pin, processed by UART, and fed back to the CPU.

### <ins>4. DAC – Digital-to-Analog Converter</ins><br>
The Digital-to-Analog Converter (DAC) in VSDBabySoC is the bridge between digital computation and the analog world. While CPUs and digital blocks work exclusively with binary numbers, most real-world systems (audio, video, sensors) are analog in nature. The DAC enables BabySoC to output real analog signals that can be interpreted by external devices.
- **What is DAC?**<br>
  * A Digital-to-Analog Converter (DAC) takes a digital binary value (e.g., `1010110010`) and produces an equivalent analog voltage or current.
  * The output analog level is proportional to the digital input value.
  * For example:
    * If the DAC is 10-bit, it can represent `2^10 = 1024` discrete voltage levels between 0 V and its maximum output (say 1 V).
    * Digital value `0000000000` → 0 V,
    * Digital value `1111111111` → Max voltage,
    * Middle value `1000000000` → Half of max voltage.
- **10-bit DAC in BabySoC**<br>
  In VSDBabySoC, a 10-bit DAC is implemented.
  * Why 10-bit?
    * 10 bits → 1024 distinct output voltage levels → fairly smooth analog signal.
    * It’s a sweet spot: not too complex for a teaching SoC, but enough resolution to demonstrate waveform generation and multimedia output.
  * Role in BabySoC:
    * Used to generate analog waveforms (like a sine wave or audio signals).
    * Demonstrates how digital computations can drive real-world analog outputs.
- **How RVMYTH Drives the DAC (`r17` Register to Analog Output)**<br>
  * The RVMYTH CPU core is programmed to sequentially update register r17 with digital values.
  * These values are then fed into the DAC.
  * As the CPU cycles through a pattern (for example, sine wave values stored in memory), r17 is updated continuously.
- **`OUT` File Generation in Simulation**<br>
  When simulating BabySoC, the DAC’s analog output is not directly fed into real hardware. Instead:
  * The DAC’s generated values are saved into a file named `OUT` during simulation.
  * This `OUT` file contains the analog-equivalent waveform data.
  * The file can then be:
    * Plotted on a graph (to visualize the waveform, e.g., a sine wave).
    * Fed into external tools or devices (like simulation software for TV/mobile phone inputs) to mimic real-world usage.

---

## 4. Functional Modelling of VSDBabySoC.
### <ins>1. Flow of Modelling</ins><br>
```
TL-Verilog (RVMYTH)  →  Verilog (via SandPiper)
                              ↓
                  +----------------------+
                  |   vsdbabysoc.v (Top) |
                  +----------------------+
                              ↓
                      +--------------+
                      | avsdpll.v    |
                      | avsddac.v    |
                      +--------------+
                              ↓
                Testbench → Simulation (Icarus)
                              ↓
                   Output Waveforms (GTKWave)

```
Explanation:
* The RVMYTH core is first compiled from TL-Verilog to Verilog using SandPiper.
* The top-level `vsdbabysoc.v` integrates RVMYTH, the DAC, and PLL.
* The testbench applies stimuli and clocks.
* Simulation results are analyzed using GTKWave to verify behavior.

### <ins>2. Setup/Installations before Simulation</ins><br>
- **Clone the Github Repository**:<br>
  Clone the Github Repository inside any arbitrary folder of your choice:
  ```
  git clone https://github.com/manili/VSDBabySoC.git
  cd VSDBabySoC/
  ```
- **Make another directory inside VSDBabySoC**:
  ```
  mkdir output
  ```
- **Final Directory Structure**:
  ```
  VSDBabySoC/
  ├── src/
  │   ├── include/                       # All the required header files (*.vh) are here
  │   │   ├── sandpiper.vh
  │   │   └── other header files...         
  │   ├── module/                        # All the required .v and .tlv files are here
  │   │   ├── vsdbabysoc.v               # Top-level module integrating all components
  │   │   ├── rvmyth.v                   # RISC-V core module (CPU)
  │   │   ├── avsdpll.v                  # PLL module (Clock)
  │   │   ├── avsddac.v                  # DAC module (Digital to Analog Converter)
  │   │   └── testbench.v                # Testbench for simulation
  └── output/                            # All the outputs after simulation will be stored here
  ```
- **Installation of Sandpiper-Saas**:<br>
  * **Installing prerequisites**:
    ```
    sudo apt update
    sudo apt install python3-venv python3-pip
    ```
  * **Creating and activating a virtual environment**:
    ```
    python3 -m venv vsd_env
    source vsd_env/bin/activate
    ```
  * **Installing SandPiper and dependencies**:
    ```
    pip install pyyaml click sandpiper-saas
    ```
    
### <ins>3. Conversion of `.tlv` files to `.v` files</ins><br>
Since the `rvmyth.tlv` file is written in TL-Verilog, we need to convert it to verilog file (`rvmyth.v`) before simulation.
```
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

Explanation:
* `-i ./src/module/*.tlv` → input all `.tlv` files from the src/module directory.
* `-o rvmyth.v` → main output Verilog file name.
* `--bestsv` → tells SandPiper to generate SystemVerilog-compliant output (best quality for modern simulators).
* `--noline` → removes extra debugging line markers (\line`) that sometimes confuse simulators like Icarus.
* `-p verilog` → target output language (plain Verilog).
* `--outdir ./src/module/` → specify where to place the generated Verilog files.

After this command, the `.v` files of the RVMYTH core can be found inside the `.../src/modules/` directory.

### <ins>4. Pre-synthesis Simulation</ins><br>
```
mkdir -p output/pre_synth_sim

iverilog -o output/pre_synth_sim/pre_synth_sim.out \
  -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v

cd output/pre_synth_sim
./pre_synth_sim.out
```
Explanation:
| Step | Command Breakdown | Purpose|
| :--- | :--- | :--- |
| 1 | `mkdir -p output/pre_synth_sim` | Create directory for simulation files |
| 2 | `iverilog ...` | Compile Verilog design + testbench |
| 3 | `-DPRE_SYNTH_SIM` | Define macro for conditional simulation |
| 4 | `-I` options | Tell compiler where to find include files |
| 5 | `src/module/testbench.v` | Main simulation input (testbench) |
| 6 | `cd output/pre_synth_sim` | Move to output folder |
| 7 | `./pre_synth_sim.out` | Run simulation executable |

### <ins>5. Observation in GTKWave</ins><br>
```
gtkwave pre_synth_sim.vcd
```
The observed output is as follows:
<div align="center">
  <img src="Images/3.png" alt="Alt Text" width="1000"/>
</div>

### <ins>6. Analysis of the observations</ins><br>
The pre-synthesis simulation of the VSDBabySoC design was performed successfully using Icarus Verilog and visualized on GTKWave. The simulation included the integrated subsystems — RVMYTH core, DAC, and PLL — interacting under the testbench environment.

- **Observation of Signal Hierarchy**:<br>
  Within GTKWave, the hierarchy tree under `vsdbabysoc_tb → uut` reveals the internal connections of the SoC:
  * The core generates the 10-bit digital output `RV_TO_DAC[9:0]`.
  * The DAC module receives this signal and produces a corresponding real-valued output (`OUT`), representing the analog equivalent of the digital data.
  * The PLL block stabilizes the system clock used across modules.

- **Digital-to-Analog Transition**:<br>
  The signal `RV_TO_DAC[9:0]` appears as a dense, rapidly switching digital pattern — this is expected, as it carries high-frequency digital codes generated by the RISC-V core.<br>
  Meanwhile, the `OUT` signal (when converted to analog format) forms a smooth, continuous waveform — an elegant transformation of the discrete digital codes into their analog voltage representation.<br>
  This verifies that:
  * The DAC module is functioning correctly, converting the 10-bit binary input to a proportional analog value.
  * The waveform shape (smooth red curve) indicates the variation in analog output as the digital input toggles through successive values.

- **How Digital Data Becomes Analog — The Role of `r17`**:<br>
  At the heart of the VSDBabySoC, the RVMYTH RISC-V core acts as the digital brain. Among its many registers, the `r17` register plays a special role — it holds the digital value that will be sent to the DAC.<br>
  During program execution, the RISC-V core continuously updates the value in `r17`. This register’s 10 least significant bits (LSBs) are routed to the `RV_TO_DAC[9:0]` bus — the digital pathway leading directly into the DAC module. Inside the DAC, this 10-bit digital input is converted into a proportional analog voltage using a resistor network model (or its behavioral equivalent in Verilog). Mathematically, this can be represented as:<br>

$$
V_{OUT} = \frac{Digital\ Value}{2^{10}-1} \times (V_{REFH} - V_{REFL}) + V_{REFL}
$$


  Where:<br>
  * *Digital Value* is the binary input from `r17[9:0]`
  * *V<sub>REFH</sub>* and *V<sub>REFL</sub>* are the high and low reference voltages
  * *V<sub>OUT</sub>* is the analog output corresponding to that digital code

  As the RISC-V core updates `r17` periodically, the DAC output (`OUT`) varies continuously, creating the smooth analog waveform seen in the simulation. Thus, `r17` serves as the bridge between the digital computation world and the analog behavior of the SoC — translating processor data into real-world analog signals.

- **Functional Validation**:<br>
  From the waveform:
  * The `CLK` and `reset` signals behave as expected — periodic clock pulses with a stable reset phase.
  * The `RV_TO_DAC` changes synchronously with the clock, confirming proper core-to-DAC communication.
  * The `OUT` waveform responds dynamically to these digital transitions, maintaining the correct analog correspondence.

    Thus, the data flow from RVMYTH → DAC → OUT is validated, confirming the functional correctness of the SoC before synthesis.

- **Insights**:<br>
  * The gradual rise and fall of the analog curve represent digital ramp sequences generated by the RISC-V processor.
  * The system demonstrates temporal coherence — meaning digital transitions directly affect analog output timing.
  * The SoC achieves functional integrity, verifying all inter-module handshakes before hardware-level synthesis.

---

## 5. Gate-Level Simulation (GLS) of VSDBabySoC.
### <ins>1. Synthesis of the Netlist.</ins>
1. **Step 1: Navigate to Module Directory & Launch Yosys**<br>
   Change the current directory to where the Verilog source files are located and start the Yosys synthesis tool.
   ```
   cd [path to your VSDBabySoC directory]/VSDBabySoC/src/module/
   yosys
   ```
   <div align="center">
     <img src="Images/6.png" alt="Alt Text" width="1000"/>
   </div>
2. **Step 2: Read Verilog Source Files**<br>
   Load the Verilog design files into Yosys for synthesis. The `-I` option specifies an additional include directory for any header or included files.
   ```
   read_verilog -I [path to your VSDBabySoC directory]/VSDBabySoC/src/include vsdbabysoc.v rvmyth.v clk_gate.v
   ```
   <div align="center">
     <img src="Images/7.png" alt="Alt Text" width="1000"/>
   </div>   
3. **Step 3: Load Standard Cell Libraries**<br>
   Import the Liberty format (`.lib`) timing and cell information files for the different modules and the target technology. These libraries provide Yosys with cell definitions, delays, and drive strengths for synthesis and mapping.
   ```
   read_liberty -lib [path to your VSDBabySoC directory]/VSDBabySoC/src/lib/avsdpll.lib
   read_liberty -lib [path to your VSDBabySoC directory]/VSDBabySoC/src/lib/avsddac.lib
   read_liberty -lib [path to your VSDBabySoC directory]/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
   <div align="center">
     <img src="Images/8.png" alt="Alt Text" width="1000"/>
   </div>   
4. **Step 4: Synthesize Top Module**<br>
   Perform RTL-to-gate-level synthesis for the top-level module `vsdbabysoc`, converting Verilog code into a technology-independent gate-level representation.
   ```
   synth -top vsdbabysoc
   ```
   <div align="center">
     <img src="Images/9.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
     <img src="Images/10.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
     <img src="Images/11.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
     <img src="Images/12.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
     <img src="Images/13.png" alt="Alt Text" width="1000"/>
   </div>
5. **Step 5: Map Flip-Flops to Standard Cells**<br>
   Map all the D flip-flops in the design to the corresponding flip-flop cells from the provided standard cell library (`sky130_fd_sc_hd__tt_025C_1v80.lib`).
   ```
   dfflibmap -liberty [path to your VSDBabySoC directory]/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
   <div align="center">
     <img src="Images/14.png" alt="Alt Text" width="1000"/>
   </div>
6. **Step 6: Optimize the Design**<br>
   Perform general logic optimizations to simplify the circuit and reduce area, delay, and redundant logic.
   ```
   opt
   ```
   <div align="center">
     <img src="Images/15.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
     <img src="Images/16.png" alt="Alt Text" width="1000"/>
   </div>
7. **Step 7: Technology Mapping with ABC**<br>
   Run the ABC tool to map the synthesized design to the target standard-cell library, applying logic optimization, retiming, and decomposition steps to generate an efficient gate-level netlist.
   ```
   abc -liberty [path to your VSDBabySoC directory]/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}
   ```
   <div align="center">
     <img src="Images/17.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
     <img src="Images/18.png" alt="Alt Text" width="1000"/>
   </div>
8. **Step 8: Flatten the Design and Clean Up**<br>
   * `flatten`: Collapse all module hierarchies into a single top-level design.
   * `setundef -zero`: Set all undefined signals to logic 0.
   * `clean -purge`: Remove unused cells and nets.
   * `rename -enumerate`: Rename all remaining signals and cells systematically for clarity.
   ```
   flatten
   setundef -zero
   clean -purge
   rename -enumerate
   ```
   <div align="center">
     <img src="Images/19.png" alt="Alt Text" width="1000"/>
   </div>
9. **Step 9: Design Statistics**<br>
   Run the `stat` command to display a summary of the current design, including the number of cells, wires, and hierarchical modules, helping you assess the complexity and size of the synthesized netlist.
   ```
   stat
   ```
   <div align="center">
     <img src="Images/20.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
     <img src="Images/21.png" alt="Alt Text" width="1000"/>
   </div>
10. **Step 10: Write Synthesized Netlist**<br>
   Use the `write_verilog` command to export the optimized gate-level netlist to a Verilog file, which can be used for post-synthesis simulations or further design analysis.
   ```
   write_verilog -noattr [path to your VSDBabySoC directory]/VSDBabySoC//output/post_synth_sim/vsdbabysoc.synth.v
   ```
   <div align="center">
     <img src="Images/22.png" alt="Alt Text" width="1000"/>
   </div>
   
11. **Step 11: Exit Yosys**<br>
   Terminate the Yosys synthesis session and return to the regular terminal shell.
   ```
   exit
   ``` 

### <ins>2. Gate-Level Simulation.</ins>
1. **Step 1: Compile Gate-Level Simulation**<br>
   Compile the gate-level netlist along with the testbench using Icarus Verilog, defining macros for post-synthesis simulation and functional behavior, and including necessary directories for source and GLS model files.
   ```
   iverilog -o [path to your VSDBabySoC directory]/VSDBabySoC/output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I [path to your VSDBabySoC directory]/VSDBabySoC/src/include -I [path to your VSDBabySoC directory]/VSDBabySoC/src/module -I [path to your VSDBabySoC directory]/VSDBabySoC/src/gls_model -I [path to your VSDBabySoC directory]/VSDBabySoC/output/post_synth_sim [path to your VSDBabySoC directory]/VSDBabySoC/src/module/testbench.v
   ```
2. **Step 2: Navigate to Output Directory**<br>
   Change the current directory to the post-synthesis simulation output folder to access the compiled simulation files and results.
   ```
   cd [path to your VSDBabySoC directory]/VSDBabySoC/output/post_synth_sim/
   ```
3. **Step 3: Run Post-Synthesis Simulation**<br>
   Execute the compiled simulation binary to verify that the gate-level netlist produces the expected functional outputs.
   ```
   ./post_synth_sim.out
   ```
4. **Step 4: View Simulation Waveforms in GTKWave**<br>
   Open the generated VCD (Value Change Dump) file in GTKWave to visually inspect the signal transitions and verify the correctness of the post-synthesis simulation.
   ```
   gtkwave post_synth_sim.vcd
   ```
   <div align="center">
     <img src="Images/23.png" alt="Alt Text" width="1000"/>
   </div>

### <ins>3. Output in GTKWave.</ins>
<div align="center">
  <img src="Images/24.png" alt="Alt Text" width="1000"/>
</div>

### <ins>4. Analysis after GLS.</ins>
After completing both the RTL simulations and gate-level simulations (GLS) of the VSDBabySoC design, a comparison of the outputs was performed using GTKWave. The key observations are as follows:
1. **Functional Equivalence:**
   The outputs of the gate-level simulation exactly match the outputs from the RTL simulation, confirming that the synthesis and subsequent technology mapping have preserved the intended functionality of the design.
2. **Timing and Signal Integrity:**
   No unexpected glitches or incorrect transitions were observed in the GLS outputs. All signals behaved as expected under the defined testbench stimuli, indicating that the design is stable and timing-correct at the synthesized level.
3. **Conclusion:**
   The successful verification of GLS against RTL simulations validates that the synthesized netlist is functionally equivalent to the RTL design. This confirms the correctness of the synthesis flow, cell mapping, and the overall implementation of the VSDBabySoC.

---

## 6. Overview of ORFS

### <ins>1. OpenROAD Flow Scripts Directory Overview</ins>
OpenROAD is organized into several key directories, each serving a specific role within the OpenROAD and RTL-to-GDSII flow:<br><br>
`OpenROAD-flow-scripts/`: This directory contains the complete flow framework and supporting infrastructure for executing the OpenROAD RTL-to-GDSII flow.
- `docker/`: Contains Docker-based installation files, run scripts, and related components.
- `docs/`: Documentation for OpenROAD and its flow scripts.
- `flow/`: Core files required for running the full RTL-to-GDSII flow.
  * `design/`: Built-in RTL-to-GDSII examples across multiple technology nodes.
  * `makefile`: Automation layer enabling end-to-end flow execution.
  * `platform/`: Technology-specific libraries, LEF files, GDS files, and related resources.
  * `tutorials/`: Reference tutorials demonstrating usage of the flow.
  * `util/`: Helper utilities supporting various flow operations.
  * `scripts/`: Execution scripts used within the flow.
- `jenkins/`: Regression tests associated with build verification and updates.
- `tools/`: All essential tools required to execute the RTL-to-GDSII flow.
- `etc/`: Dependency installers and supporting configuration utilities.
- `setup_env.sh`: Environment setup script used to source OpenROAD rules prior to running the flow.

   <div align="center">
     <img src="Images/25.png" alt="Alt Text" width="1000"/>
   </div>

---

## 7. Environment Setup and File Organization
1. Create the two required `vsdbabysoc` design directories:<br>
   ```
   cd /OpenROAD-flow-scripts/flow/
   mkdir -p OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc
   mkdir -p OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc
   ```

   * **Purpose of the Newly Created Directories**<br>
     * `designs/sky130hd/vsdbabysoc/`: This directory stores all technology-dependent and platform-specific files required for the physical design flow on the Sky130 HD PDK. Contents include:
       * **LEF files** – Abstract layouts for hard macros
       * **LIB files** – Timing libraries for synthesis and STA
       * **GDS files** – Final layout geometries for macros
       * **SDC** – Synthesis constraints
       * **Configuration files** – macro.cfg, pin_order.cfg, config.mk
      * `designs/src/vsdbabysoc/`: This directory holds all logical design sources that remain independent of technology. Contents include:
        * Verilog RTL modules
        * Include headers (`.vh` files)
        * Additional source definitions required during synthesis
     
2. Copy the resource folders from the source location `VSDBabySoC/src` into the `OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/` directory you just created.<br>
   Expected contents:<br>
   - `gds/` should include: `avsddac.gds`, `avsdpll.gds`
   - `include/` should include: `sandpiper.vh`, `sandpiper_gen.vh`, `sp_default.vh`, `sp_verilog.vh`
   - `lef/` should include: `avsddac.lef`, `avsdpll.lef`
   - `lib/` should include: `avsddac.lib`, `avsdpll.lib`
3. Copy the SDC constraint file from the source `VSDBabySoC/src/sdc/` to the `OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/` directory.
4. Copy the layout configuration files (`macro.cfg` and `pin_order.cfg`) from `VSDBabySoC/src/layout_conf/vsdbabysoc` directory to the `OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/` directory.
5. Create a file named `config.mk` inside the `OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/` directory with the following contents:
   ```
   export DESIGN_NICKNAME = vsdbabysoc
   export DESIGN_NAME = vsdbabysoc
   export PLATFORM    = sky130hd

   # export VERILOG_FILES_BLACKBOX = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/IPs/*.v
   # export VERILOG_FILES = $(sort $(wildcard $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/*.v))
   # Explicitly list the Verilog files for synthesis
   export VERILOG_FILES = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/vsdbabysoc.v \
                          $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/rvmyth.v \
                          $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/clk_gate.v

   export SDC_FILE      = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/vsdbabysoc_synthesis.sdc

   export vsdbabysoc_DIR = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)

   export VERILOG_INCLUDE_DIRS = $(wildcard $(vsdbabysoc_DIR)/include/)
   # export SDC_FILE      = $(wildcard $(vsdbabysoc_DIR)/sdc/*.sdc)
   export ADDITIONAL_GDS  = $(wildcard $(vsdbabysoc_DIR)/gds/*.gds.gz)
   export ADDITIONAL_LEFS  = $(wildcard $(vsdbabysoc_DIR)/lef/*.lef)
   export ADDITIONAL_LIBS = $(wildcard $(vsdbabysoc_DIR)/lib/*.lib)
   # export PDN_TCL = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/pdn.tcl

   # Clock Configuration (vsdbabysoc specific)
   # export CLOCK_PERIOD = 20.0
   export CLOCK_PORT = CLK
   export CLOCK_NET = $(CLOCK_PORT)

   # Floorplanning Configuration (vsdbabysoc specific)
   export FP_PIN_ORDER_CFG = $(wildcard $(DESIGN_DIR)/pin_order.cfg)
   # export FP_SIZING = absolute

   export DIE_AREA   = 0 0 1600 1600
   export CORE_AREA  = 20 20 1590 1590

   # Placement Configuration (vsdbabysoc specific)
   export MACRO_PLACEMENT_CFG = $(wildcard $(DESIGN_DIR)/macro.cfg)
   export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600: -exclude right:* -exclude top:* -exclude bottom:*
   # export MACRO_PLACEMENT = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/macro_placement.cfg

   export TNS_END_PERCENT = 100
   export REMOVE_ABC_BUFFERS = 1

   # Magic Tool Configuration
   export MAGIC_ZEROIZE_ORIGIN = 0
   export MAGIC_EXT_USE_GDS = 1

   # CTS tuning
   export CTS_BUF_DISTANCE = 600
   export SKIP_GATE_CLONING = 1

   # export CORE_UTILIZATION=0.1  # Reduce this value to allow more whitespace for routing.
   ```

6. Now copy the following files from `VSDBabySoC/src/module/` to `OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc`:
   * `vsdbabysoc.v`
   * `rvmyth.v`
   * `rvmyth_gen.v`
   * `clk_gate.v`
   * `avsddac.v`
   * `avsdpll.v`
8. The final structure of directories and files should look like below:
   ```
   designs/
   ├── src/vsdbabysoc/
   │           ├── vsdbabysoc.v
   │           ├── rvmyth.v
   │           ├── rvmyth_gen.v
   │           ├── clk_gate.v
   │           ├── avsddac.v
   │           └── avsdpll.v
   └── sky130hd/vsdbabysoc/
                    ├── gds/
                    │   ├── avsddac.gds
                    │   └── avsdpll.gds
                    ├── lef/
                    │   ├── avsddac.lef
                    │   └── avsdpll.lef
                    ├── lib/
                    │   ├── avsddac.lib
                    │   └── avsdpll.lib
                    └── include/
                    │   ├── sandpiper.vh
                    │   ├── sandpiper_gen.vh
                    │   ├── sp_default.vh
                    │   └── sp_verilog.vh
                    ├── macro.cfg
                    ├── pin_order.cfg
                    ├── config.mk
                    └── vsdbabysoc_synthesis.sdc
   ```

---

## 8. Synthesis
I had also completed the synthesis step in Week 3, prior to performing GLS. However, the synthesis was executed once again in Week 7 as part of the integrated OpenROAD flow.
### <ins>1. Commands</ins>
First run the following commands:
```
cd OpenROAD-flow-scripts
source env.sh
cd flow
```

Now, execute synthesis:
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

### <ins>2. Execution of Synthesis in the Terminal</ins>
<div align="center">
<img src="Images/26.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/27.png" alt="Alt Text" width="1000"/>
</div>

### <ins>3. Synthesized Netlist</ins>
<div align="center">
<img src="Images/28.png" alt="Alt Text" width="1000"/>
</div>

### <ins>4. Synthesis Log</ins>
<div align="center">
<img src="Images/29.png" alt="Alt Text" width="1000"/>
</div>

### <ins>5. Synthesis Check</ins>
<div align="center">
<img src="Images/30.png" alt="Alt Text" width="1000"/>
</div>

### <ins>6. Synthesis Stats</ins>
<div align="center">
<img src="Images/31.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/32.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/33.png" alt="Alt Text" width="1000"/>
</div>

---

## 9. Floorplan
### <ins>1. Commands</ins>
Now, execute floorplan:
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

### <ins>2. Execution of Floorplan in the Terminal</ins>
<div align="center">
<img src="Images/34.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/35.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/36.png" alt="Alt Text" width="1000"/>
</div>

### <ins>3. Visualize Floorplan</ins>
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```
<div align="center">
<img src="Images/37.png" alt="Alt Text" width="1000"/>
</div>

<div align="center">
<img src="Images/38.png" alt="Alt Text" width="1000"/>
</div>

---

## 10. Placement
### <ins>1. Commands</ins>
Now, execute floorplan:
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

### <ins>2. Execution of Placement in the Terminal</ins>
<div align="center">
<img src="Images/39.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/40.png" alt="Alt Text" width="1000"/>
</div>

### <ins>3. Visualize Placement</ins>
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
```

<div align="center">
<img src="Images/41.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/42.png" alt="Alt Text" width="1000"/>
</div>

Heatmap:
<div align="center">
<img src="Images/43.png" alt="Alt Text" width="1000"/>
</div>

Zooming into the different parts:
<div align="center">
<img src="Images/44.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/45.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/46.png" alt="Alt Text" width="1000"/>
</div>

---

## 11. Clock Tree Synthesis (CTS)
### <ins>1. Commands</ins>
Now, execute cts:
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

### <ins>2. Execution of CTS in the Terminal</ins>
<div align="center">
<img src="Images/47.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/48.png" alt="Alt Text" width="1000"/>
</div>

### <ins>3. Visualize CTS</ins>
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```

<div align="center">
<img src="Images/49.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/50.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/51.png" alt="Alt Text" width="1000"/>
</div>

### <ins>4. Final CTS Report</ins>
```

==========================================================================
cts final report_tns
--------------------------------------------------------------------------
tns max 0.00

==========================================================================
cts final report_wns
--------------------------------------------------------------------------
wns max 0.00

==========================================================================
cts final report_worst_slack
--------------------------------------------------------------------------
worst slack max 6.07

==========================================================================
cts final report_clock_min_period
--------------------------------------------------------------------------
clk period_min = 4.93 fmax = 202.83

==========================================================================
cts final report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.77 source latency core.CPU_rd_a5[0]$_DFF_P_/CLK ^
  -0.86 target latency core.CPU_Xreg_value_a4[26][6]$_SDFFE_PP0P_/CLK ^
   0.00 CRPR
--------------
  -0.08 setup skew


==========================================================================
cts final report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[4]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[4]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.15    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00    0.27 ^ clkbuf_3_0__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.20    0.21    0.31    0.58 ^ clkbuf_3_0__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_0__leaf_CLK (net)
                  0.21    0.00    0.58 ^ clkbuf_leaf_106_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.03    0.05    0.19    0.77 ^ clkbuf_leaf_106_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_106_CLK (net)
                  0.05    0.00    0.78 ^ core.CPU_rd_a2[4]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.30    1.07 ^ core.CPU_rd_a2[4]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_rd_a2[4] (net)
                  0.04    0.00    1.07 ^ core.CPU_rd_a3[4]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  1.07   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.15    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00    0.27 ^ clkbuf_3_1__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    14    0.23    0.24    0.33    0.60 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_1__leaf_CLK (net)
                  0.24    0.00    0.60 ^ clkbuf_leaf_105_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.04    0.06    0.21    0.81 ^ clkbuf_leaf_105_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_105_CLK (net)
                  0.06    0.00    0.81 ^ core.CPU_rd_a3[4]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.00    0.81   clock reconvergence pessimism
                         -0.03    0.78   library hold time
                                  0.78   data required time
-----------------------------------------------------------------------------
                                  0.78   data required time
                                 -1.07   data arrival time
-----------------------------------------------------------------------------
                                  0.29   slack (MET)



==========================================================================
cts final report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[2]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.15    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00    0.27 ^ clkbuf_3_1__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    14    0.23    0.24    0.33    0.60 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_1__leaf_CLK (net)
                  0.24    0.00    0.60 ^ clkbuf_leaf_64_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.05    0.07    0.21    0.82 ^ clkbuf_leaf_64_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_64_CLK (net)
                  0.07    0.00    0.82 ^ core.CPU_src1_value_a3[2]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
    11    0.05    0.47    0.62    1.43 ^ core.CPU_src1_value_a3[2]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[2] (net)
                  0.47    0.00    1.43 ^ _10761_/A (sky130_fd_sc_hd__ha_1)
     8    0.03    0.18    0.43    1.86 v _10761_/SUM (sky130_fd_sc_hd__ha_1)
                                         _00040_ (net)
                  0.18    0.00    1.86 v _07458_/B_N (sky130_fd_sc_hd__nor2b_1)
     3    0.01    0.06    0.21    2.08 v _07458_/Y (sky130_fd_sc_hd__nor2b_1)
                                         _02570_ (net)
                  0.06    0.00    2.08 v _07460_/A3 (sky130_fd_sc_hd__o31ai_1)
     3    0.02    0.54    0.47    2.54 ^ _07460_/Y (sky130_fd_sc_hd__o31ai_1)
                                         _02572_ (net)
                  0.54    0.00    2.54 ^ _07720_/A1 (sky130_fd_sc_hd__a2111oi_0)
     1    0.01    0.20    0.28    2.82 v _07720_/Y (sky130_fd_sc_hd__a2111oi_0)
                                         _02828_ (net)
                  0.20    0.00    2.82 v place267/A (sky130_fd_sc_hd__buf_4)
     1    0.00    0.03    0.20    3.02 v place267/X (sky130_fd_sc_hd__buf_4)
                                         net266 (net)
                  0.03    0.00    3.02 v _07721_/C (sky130_fd_sc_hd__or3_1)
     4    0.03    0.18    0.44    3.46 v _07721_/X (sky130_fd_sc_hd__or3_1)
                                         _02829_ (net)
                  0.18    0.00    3.46 v _07891_/A3 (sky130_fd_sc_hd__a311o_1)
     1    0.00    0.06    0.41    3.86 v _07891_/X (sky130_fd_sc_hd__a311o_1)
                                         _02995_ (net)
                  0.06    0.00    3.86 v _07892_/B (sky130_fd_sc_hd__xnor2_1)
     2    0.02    0.40    0.35    4.22 ^ _07892_/Y (sky130_fd_sc_hd__xnor2_1)
                                         _02996_ (net)
                  0.40    0.00    4.22 ^ _07910_/B1 (sky130_fd_sc_hd__o221ai_1)
     2    0.01    0.16    0.23    4.45 v _07910_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _03014_ (net)
                  0.16    0.00    4.45 v _07912_/A4 (sky130_fd_sc_hd__a41oi_2)
    11    0.07    0.89    0.79    5.24 ^ _07912_/Y (sky130_fd_sc_hd__a41oi_2)
                                         _03016_ (net)
                  0.89    0.01    5.25 ^ _08968_/A (sky130_fd_sc_hd__nand2_1)
     1    0.01    0.19    0.18    5.44 v _08968_/Y (sky130_fd_sc_hd__nand2_1)
                                         _03941_ (net)
                  0.19    0.00    5.44 v _08970_/A1 (sky130_fd_sc_hd__a21oi_1)
     1    0.01    0.22    0.25    5.69 ^ _08970_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _00889_ (net)
                  0.22    0.00    5.69 ^ core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.69   data arrival time

                         11.00   11.00   clock clk (rise edge)
                          0.00   11.00   clock source latency
     1    0.15    0.00    0.00   11.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01   11.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.26   11.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00   11.27 ^ clkbuf_3_2__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    19    0.27    0.28    0.36   11.63 ^ clkbuf_3_2__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_2__leaf_CLK (net)
                  0.28    0.00   11.63 ^ clkbuf_leaf_95_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.22   11.85 ^ clkbuf_leaf_95_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_95_CLK (net)
                  0.06    0.00   11.85 ^ core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.00   11.85   clock reconvergence pessimism
                         -0.09   11.76   library setup time
                                 11.76   data required time
-----------------------------------------------------------------------------
                                 11.76   data required time
                                 -5.69   data arrival time
-----------------------------------------------------------------------------
                                  6.07   slack (MET)



==========================================================================
cts final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[2]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.15    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00    0.27 ^ clkbuf_3_1__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    14    0.23    0.24    0.33    0.60 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_1__leaf_CLK (net)
                  0.24    0.00    0.60 ^ clkbuf_leaf_64_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.05    0.07    0.21    0.82 ^ clkbuf_leaf_64_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_64_CLK (net)
                  0.07    0.00    0.82 ^ core.CPU_src1_value_a3[2]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
    11    0.05    0.47    0.62    1.43 ^ core.CPU_src1_value_a3[2]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[2] (net)
                  0.47    0.00    1.43 ^ _10761_/A (sky130_fd_sc_hd__ha_1)
     8    0.03    0.18    0.43    1.86 v _10761_/SUM (sky130_fd_sc_hd__ha_1)
                                         _00040_ (net)
                  0.18    0.00    1.86 v _07458_/B_N (sky130_fd_sc_hd__nor2b_1)
     3    0.01    0.06    0.21    2.08 v _07458_/Y (sky130_fd_sc_hd__nor2b_1)
                                         _02570_ (net)
                  0.06    0.00    2.08 v _07460_/A3 (sky130_fd_sc_hd__o31ai_1)
     3    0.02    0.54    0.47    2.54 ^ _07460_/Y (sky130_fd_sc_hd__o31ai_1)
                                         _02572_ (net)
                  0.54    0.00    2.54 ^ _07720_/A1 (sky130_fd_sc_hd__a2111oi_0)
     1    0.01    0.20    0.28    2.82 v _07720_/Y (sky130_fd_sc_hd__a2111oi_0)
                                         _02828_ (net)
                  0.20    0.00    2.82 v place267/A (sky130_fd_sc_hd__buf_4)
     1    0.00    0.03    0.20    3.02 v place267/X (sky130_fd_sc_hd__buf_4)
                                         net266 (net)
                  0.03    0.00    3.02 v _07721_/C (sky130_fd_sc_hd__or3_1)
     4    0.03    0.18    0.44    3.46 v _07721_/X (sky130_fd_sc_hd__or3_1)
                                         _02829_ (net)
                  0.18    0.00    3.46 v _07891_/A3 (sky130_fd_sc_hd__a311o_1)
     1    0.00    0.06    0.41    3.86 v _07891_/X (sky130_fd_sc_hd__a311o_1)
                                         _02995_ (net)
                  0.06    0.00    3.86 v _07892_/B (sky130_fd_sc_hd__xnor2_1)
     2    0.02    0.40    0.35    4.22 ^ _07892_/Y (sky130_fd_sc_hd__xnor2_1)
                                         _02996_ (net)
                  0.40    0.00    4.22 ^ _07910_/B1 (sky130_fd_sc_hd__o221ai_1)
     2    0.01    0.16    0.23    4.45 v _07910_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _03014_ (net)
                  0.16    0.00    4.45 v _07912_/A4 (sky130_fd_sc_hd__a41oi_2)
    11    0.07    0.89    0.79    5.24 ^ _07912_/Y (sky130_fd_sc_hd__a41oi_2)
                                         _03016_ (net)
                  0.89    0.01    5.25 ^ _08968_/A (sky130_fd_sc_hd__nand2_1)
     1    0.01    0.19    0.18    5.44 v _08968_/Y (sky130_fd_sc_hd__nand2_1)
                                         _03941_ (net)
                  0.19    0.00    5.44 v _08970_/A1 (sky130_fd_sc_hd__a21oi_1)
     1    0.01    0.22    0.25    5.69 ^ _08970_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _00889_ (net)
                  0.22    0.00    5.69 ^ core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.69   data arrival time

                         11.00   11.00   clock clk (rise edge)
                          0.00   11.00   clock source latency
     1    0.15    0.00    0.00   11.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01   11.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.26   11.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00   11.27 ^ clkbuf_3_2__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    19    0.27    0.28    0.36   11.63 ^ clkbuf_3_2__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_2__leaf_CLK (net)
                  0.28    0.00   11.63 ^ clkbuf_leaf_95_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.22   11.85 ^ clkbuf_leaf_95_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_95_CLK (net)
                  0.06    0.00   11.85 ^ core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.00   11.85   clock reconvergence pessimism
                         -0.09   11.76   library setup time
                                 11.76   data required time
-----------------------------------------------------------------------------
                                 11.76   data required time
                                 -5.69   data arrival time
-----------------------------------------------------------------------------
                                  6.07   slack (MET)



==========================================================================
cts final report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
cts final max_slew_check_slack
--------------------------------------------------------------------------
0.19346469640731812

==========================================================================
cts final max_slew_check_limit
--------------------------------------------------------------------------
1.4979510307312012

==========================================================================
cts final max_slew_check_slack_limit
--------------------------------------------------------------------------
0.1292

==========================================================================
cts final max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_capacitance_check_slack
--------------------------------------------------------------------------
0.0034384692553430796

==========================================================================
cts final max_capacitance_check_limit
--------------------------------------------------------------------------
0.021067000925540924

==========================================================================
cts final max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.1632

==========================================================================
cts final max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
cts final max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
cts final max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
cts final setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
cts final hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
cts final report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[2]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34    0.60 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.82 ^ clkbuf_leaf_64_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.82 ^ core.CPU_src1_value_a3[2]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.62    1.43 ^ core.CPU_src1_value_a3[2]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.43    1.86 v _10761_/SUM (sky130_fd_sc_hd__ha_1)
   0.22    2.08 v _07458_/Y (sky130_fd_sc_hd__nor2b_1)
   0.47    2.54 ^ _07460_/Y (sky130_fd_sc_hd__o31ai_1)
   0.28    2.82 v _07720_/Y (sky130_fd_sc_hd__a2111oi_0)
   0.20    3.02 v place267/X (sky130_fd_sc_hd__buf_4)
   0.44    3.46 v _07721_/X (sky130_fd_sc_hd__or3_1)
   0.41    3.86 v _07891_/X (sky130_fd_sc_hd__a311o_1)
   0.35    4.22 ^ _07892_/Y (sky130_fd_sc_hd__xnor2_1)
   0.23    4.45 v _07910_/Y (sky130_fd_sc_hd__o221ai_1)
   0.79    5.24 ^ _07912_/Y (sky130_fd_sc_hd__a41oi_2)
   0.20    5.44 v _08968_/Y (sky130_fd_sc_hd__nand2_1)
   0.25    5.69 ^ _08970_/Y (sky130_fd_sc_hd__a21oi_1)
   0.00    5.69 ^ core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           5.69   data arrival time

  11.00   11.00   clock clk (rise edge)
   0.00   11.00   clock source latency
   0.00   11.00 ^ pll/CLK (avsdpll)
   0.26   11.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.37   11.63 ^ clkbuf_3_2__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.22   11.85 ^ clkbuf_leaf_95_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   11.85 ^ core.CPU_Xreg_value_a4[1][18]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.00   11.85   clock reconvergence pessimism
  -0.09   11.76   library setup time
          11.76   data required time
---------------------------------------------------------
          11.76   data required time
          -5.69   data arrival time
---------------------------------------------------------
           6.07   slack (MET)



==========================================================================
cts final report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[4]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[4]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.32    0.58 ^ clkbuf_3_0__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.77 ^ clkbuf_leaf_106_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.78 ^ core.CPU_rd_a2[4]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.30    1.07 ^ core.CPU_rd_a2[4]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.00    1.07 ^ core.CPU_rd_a3[4]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
           1.07   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34    0.60 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.81 ^ clkbuf_leaf_105_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.81 ^ core.CPU_rd_a3[4]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.00    0.81   clock reconvergence pessimism
  -0.03    0.78   library hold time
           0.78   data required time
---------------------------------------------------------
           0.78   data required time
          -1.07   data arrival time
---------------------------------------------------------
           0.29   slack (MET)



==========================================================================
cts final critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path delay
--------------------------------------------------------------------------
5.6896

==========================================================================
cts final critical path slack
--------------------------------------------------------------------------
6.0697

==========================================================================
cts final slack div critical path delay
--------------------------------------------------------------------------
106.680610

==========================================================================
cts final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.38e-03   4.77e-04   9.27e-09   4.86e-03  40.5%
Combinational          8.62e-04   1.90e-03   9.58e-09   2.76e-03  23.0%
Clock                  2.52e-03   1.87e-03   2.06e-09   4.39e-03  36.6%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  7.76e-03   4.25e-03   2.09e-08   1.20e-02 100.0%
                          64.6%      35.4%       0.0%
```

---

## 12. Routing
> [!CAUTION]
> The routing stage initially encountered several congestion-related challenges. In this section, I present only the final routing results obtained after resolving those issues. However, it is likely that similar congestion problems may arise during your own routing attempts. For reference, I have documented the complete debugging process I went through [here](#2-week-7), which you may review for guidance.


### <ins>1. Commands</ins>
Now, execute routing:
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```

### <ins>2. Execution of Routing in the Terminal</ins>
<div align="center">
<img src="Images/52.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/53.png" alt="Alt Text" width="1000"/>
</div>

### <ins>3. Visualize Routing</ins>
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_route
```
<div align="center">
<img src="Images/54.png" alt="Alt Text" width="1000"/>
</div>

The final routed layout:
<div align="center">
<img src="Images/55.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/56.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
<img src="Images/57.png" alt="Alt Text" width="1000"/>
</div>

---

## 13. Post-Route SPEF Generation
This section outlines the step-by-step procedure for generating the post-route Standard Parasitic Exchange Format (SPEF) file and the post-placement Verilog netlist for the VSDBabySoC design using OpenROAD. These outputs are critical for accurate post-routing timing analysis and signoff. The SPEF file captures the parasitic resistance and capacitance extracted from the physical layout, while the updated Verilog netlist represents the finalized connectivity after placement and routing.

### <ins>1. Ensure Availability of RC Extraction Rule Files</ins>
Before initiating SPEF generation, it is necessary to have access to the following RCX rule files:
- `rules.openrcx.sky130A.nom.calibre`
- `rules.openrcx.sky130A.min.calibre`
- `rules.openrcx.sky130A.max.calibre`

These files are required by OpenRCX to perform accurate parasitic extraction for the Sky130A process. Since they were not present in my environment, the first step was to install open_pdks, which provides the necessary technology files, extraction rules, and associated PDK resources required for post-route SPEF generation.

1. **Create the PDK directory**<br>
   This ensures you have a clean place to install it.
   ```
   mkdir -p /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/external_resources/pdks
   ```

2. **Export the PDK_ROOT environment variable**<br>
   ```
   export PDK_ROOT=/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/external_resources/pdks
   ```
   To make this permanent, add it to your `.bashrc`:
   ```
   echo "export PDK_ROOT=/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/external_resources/pdks" >> ~/.bashrc
   source ~/.bashrc
   ```

3. **Install open_pdks (Sky130)**<br>
   ```
   cd /home/bitopan/VSD_Tapeout_Program

   git clone https://github.com/RTimothyEdwards/open_pdks.git

   cd open_pdks

   ./configure --enable-sky130-pdk --prefix=$PDK_ROOT

   make -j$(nproc)

   make install
   ```

4. **Verify the extraction rules exist**<br>
   After installation, run:
   ```
   find /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/external_resources/pdks -type f -name "*.calibre"
   ```
   <div align="center">
      <img src="Images/58.png" alt="Alt Text" width="1000"/>
   </div>

### <ins>2. Generating the Post-Route `.def` File</ins>
Before initiating SPEF extraction, the post-route `.def` file is required as a key input. However, after completing the OpenROAD flow for VSDBabySoC, the only routing output available was the `5_2_route.odb` database. Since the `.def` file was not automatically generated, it had to be manually extracted from the `.odb` file using OpenROAD.
```
openroad

read_db /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.odb

write_def /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.def
```

<div align="center">
   <img src="Images/59.png" alt="Alt Text" width="1000"/>
</div>

### <ins>3. SPEF file generation</ins>
Follow these commands for SPEF generation:
```
# Navigate to the root directory of the OpenROAD flow environment.
cd ~/OpenROAD-flow-scripts

# Load all required environment variables and tool paths for OpenROAD and its dependencies.
source env.sh

# Enter the flow directory where design files and run outputs are organized.
cd flow/

# Launch the OpenROAD interactive shell for manual commands and extraction flows.
openroad

# Load the technology LEF containing metal layers, design rules, and process constraints.
read_lef /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/platforms/sky130hd/lef/sky130_fd_sc_hd.tlef

# Load the merged standard-cell LEF with physical abstracts of all library cells.
read_lef /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/platforms/sky130hd/lef/sky130_fd_sc_hd_merged.lef

# Load the LEF abstract for the PLL macro used in the design.
read_lef /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsdpll.lef

# Load the LEF abstract for the DAC macro included in the VSDBabySoC.
read_lef /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsddac.lef

# Import the timing library for the typical-corner standard cells.
read_liberty /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Load the post-route DEF file containing final placement, routing, and geometries.
read_def /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.def

# Define the RC extraction corner using the OpenRCX model for the Sky130 process.
define_process_corner -ext_model_index 0 /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/external_resources/pdks/share/pdk/sky130A/libs.tech/openlane/rules.openrcx.sky130A.nom.calibre

# Perform parasitic RC extraction on the routed layout using the specified model.
extract_parasitics -ext_model_file /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/external_resources/pdks/share/pdk/sky130A/libs.tech/openlane/rules.openrcx.sky130A.nom.calibre

# Write out the extracted parasitics in SPEF format for timing analysis.
write_spef /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/vsdbabysoc.spef

# Export the updated post-route Verilog netlist reflecting final connectivity.
write_verilog /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/vsdbabysoc_post_place.v
```

<div align="center">
   <img src="Images/60.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
   <img src="Images/61.png" alt="Alt Text" width="1000"/>
</div>

The generated `vasbabysoc.spef` file:
<div align="center">
   <img src="Images/62.png" alt="Alt Text" width="1000"/>
</div>

The generated `vsdbabysoc_post_place.v` file:
<div align="center">
   <img src="Images/63.png" alt="Alt Text" width="1000"/>
</div>

---

## 14. Required Files for Post-SPEF STA
This section gives a concise but clear breakdown of all the files needed for Post-Layout STA - what each file represents, how it was produced in earlier stages of the flow (or maybe needs to be created now), and where it can be found within the project directory. This ensures that anyone following the flow can trace the origin of every input before running post-SPEF multi-corner STA.

### <ins>1. Gate-Level Netlist (Post-Route).</ins>
- File name: `5_2_route.v`
- Generated in: Need to generate from the `5_2_route.odb` file.
- Purpose: This Verilog netlist is the output after placement and routing, representing the final connectivity of all standard cells along with any routing-driven buffer insertions or optimizations done by OpenROAD. It reflects the “as-routed” logical structure of the chip.
- Steps to generate the `5_2_route.v` file:
  ```
  #Invoke Openroad in terminal
  openroad

  #Read the 5_2_route.odb file
  read_db /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.odb

  #Generate the 5_2_route.v file
  write_verilog /home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.v

  #Exit
  exit
  ```
  
   <div align="center">
     <img src="Images/84.png" alt="Alt Text" width="1000"/>
   </div>
- File saved in this path: `/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.v`

### <ins>2. SPEF File (Extracted Parasitics).</ins>
- File name: `vsdbabysoc.spef`
- Generated in: Week 7 – Parasitic Extraction. [Check here](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-7.git)
- Purpose: This file contains all extracted parasitics from the routed layout — wire resistance, net capacitances, coupling values, pin capacitances. Annotating this into OpenSTA enables real, RC-aware timing analysis instead of ideal wires.
- Path: `/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/vsdbabysoc.spef`

### <ins>3. Liberty Files for PVT Corners.</ins>
- File name:
  * `sky130_fd_sc_hd__tt_025C_1v80.lib`
  * `sky130_fd_sc_hd__ff_100C_1v65.lib`
  * `sky130_fd_sc_hd__ff_100C_1v95.lib`
  * `sky130_fd_sc_hd__ff_n40C_1v56.lib`
  * `sky130_fd_sc_hd__ff_n40C_1v65.lib`
  * `sky130_fd_sc_hd__ff_n40C_1v76.lib`
  * `sky130_fd_sc_hd__ss_100C_1v40.lib`
  * `sky130_fd_sc_hd__ss_100C_1v60.lib`
  * `sky130_fd_sc_hd__ss_n40C_1v28.lib`
  * `sky130_fd_sc_hd__ss_n40C_1v35.lib`
  * `sky130_fd_sc_hd__ss_n40C_1v40.lib`
  * `sky130_fd_sc_hd__ss_n40C_1v44.lib`
  * `sky130_fd_sc_hd__ss_n40C_1v76.lib`
- Generated in: Provided within the PDK.
- Purpose: Each `.lib` contains cell delays, transition tables, and setup/hold constraints for a specific Process-Voltage-Temperature (PVT) corner. OpenSTA loads these libraries to compute timing under worst, best, and typical operating conditions.
- Path: `/home/bitopan/VSD_Tapeout_Program/open_pdks/sources/sky130_fd_sc_hd/timing/`

### <ins>4. Timing Constraints (SDC File).</ins>
- File name: `4_cts.sdc`
- Generated in: Week 7 – During Clock Tree Synthesis. [Check here](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-7.git)
- Purpose: Defines the design’s timing intent: clock definitions, I/O delays, false paths, multicycle paths, and all other STA constraints. This same SDC must be used consistently across synthesis, post-route STA, and multi-corner analysis.
- Path: `/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/4_cts.sdc`

### <ins>5. Custom IP Liberty Files (PLL & DAC).</ins>
- File name:
  * `avsddac.lib`
  * `avsdpll.lib`
- Generated in: Provided by the VSDBabySoC design package
- Purpose: Models the timing of the DAC macro’s digital pins so that its interactions with the SoC’s digital logic are correctly timing-checked.
- Path: `/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib`

### <ins>6. Multi-Corner OpenSTA TCL Script.</ins>
- File name: `sta_across_pvt_route_custom.tcl`
- Generated in: Need to make it now.
- Purpose: Automates the entire STA flow — loading netlist, libraries, constraints, SPEF, defining corners, and running `report_checks` for both setup and hold across all corners. This script ensures repeatable, consistent analysis.
- Use the following content (modify the paths of files wherever required):
  ```
  ### ============================================================
  ###               VSDBabySoC  –  Post-Route STA
  ### ============================================================

  puts "\n▶ Starting Post-Route STA for VSDBabySoC...\n"

  # --------------------------------------------------------------
  # Units
  # --------------------------------------------------------------
  puts "→ Setting units..."
  set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
  set_units      -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um


  # --------------------------------------------------------------
  # Liberty file list
  # --------------------------------------------------------------
  puts "\n→ Loading corner list..."

  set list_of_lib_files(1)  "sky130_fd_sc_hd__tt_025C_1v80.lib"
  set list_of_lib_files(2)  "sky130_fd_sc_hd__ff_100C_1v65.lib"
  set list_of_lib_files(3)  "sky130_fd_sc_hd__ff_100C_1v95.lib"
  set list_of_lib_files(4)  "sky130_fd_sc_hd__ff_n40C_1v56.lib"
  set list_of_lib_files(5)  "sky130_fd_sc_hd__ff_n40C_1v65.lib"
  set list_of_lib_files(6)  "sky130_fd_sc_hd__ff_n40C_1v76.lib"
  set list_of_lib_files(7)  "sky130_fd_sc_hd__ss_100C_1v40.lib"
  set list_of_lib_files(8)  "sky130_fd_sc_hd__ss_100C_1v60.lib"
  set list_of_lib_files(9)  "sky130_fd_sc_hd__ss_n40C_1v28.lib"
  set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
  set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
  set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
  set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"


  # --------------------------------------------------------------
  # Load analog macro Liberty files once
  # --------------------------------------------------------------
  puts "\n→ Loading DAC/PLL Liberty models..."
  read_liberty /data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib
  read_liberty /data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib


  # --------------------------------------------------------------
  # Begin corner loop
  # --------------------------------------------------------------
  puts "\n============================================================"
  puts "              BEGIN PVT CORNER SWEEP"
  puts "============================================================\n"

  for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
      puts "\n------------------------------------------------------------"
      puts " Running Corner $i  →  $list_of_lib_files($i)"
      puts "------------------------------------------------------------"

      # Load Liberty for this corner
      read_liberty /data/VSD_Tapeout_Program/open_pdks/sources/sky130_fd_sc_hd/timing/$list_of_lib_files($i)

      # Reload design
      read_verilog /data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.v
      link_design vsdbabysoc
      current_design

      # Constraints & parasitics
      read_sdc  /data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/4_cts.sdc
      read_spef /data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/vsdbabysoc.spef
    
      puts " → Timing graph built."
      puts " → Running setup/hold analysis..."

      # Full checks
      report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} \
          > /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/min_max_$list_of_lib_files($i).txt

      # WNS & TNS summary logs
      exec echo "\n$list_of_lib_files($i)" >> /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_worst_max_slack.txt
      report_worst_slack -max -digits {4} >> /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_worst_max_slack.txt

      exec echo "\n$list_of_lib_files($i)" >> /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_worst_min_slack.txt
      report_worst_slack -min -digits {4} >> /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_worst_min_slack.txt

      exec echo "\n$list_of_lib_files($i)" >> /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_tns.txt
      report_tns -digits {4} >> /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_tns.txt

      exec echo "\n$list_of_lib_files($i)" >> /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_wns.txt
      report_wns -digits {4} >> /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_wns.txt

      puts " → Corner $i complete."
  }

  puts "\n============================================================"
  puts "              END OF MULTI-CORNER STA"
  puts "============================================================\n"

  exit
  ```
- File saved in this path: `/home/bitopan/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/sta_across_pvt_route_custom.tcl`

---

## 15. Procedure for Post-SPEF STA
To perform post-route STA within the Docker environment, launch a container with the project directory mounted and execute the `sta_across_pvt_route_custom.tcl` script inside it. The script will generate all relevant timing reports—including setup and hold analyses, WNS, and TNS—directly within the mounted `/data` directory. Running the flow through Docker ensures a consistent, isolated, and fully reproducible analysis environment.
```
docker run -it -v /home/bitopan:/data opensta /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/sta_across_pvt_route_custom.tcl
```

<div align="center">
  <img src="Images/85.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
  <img src="Images/86.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
  <img src="Images/87.png" alt="Alt Text" width="1000"/>
</div>

After executing the STA script, all generated timing reports can be found in the directory:
`/home/bitopan/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/`.

This folder contains the complete set of post-route analysis outputs, including detailed path-delay reports for each PVT corner (`min_max_*.txt`), worst setup and hold slack summaries (`sta_worst_max_slack.txt` and `sta_worst_min_slack.txt`), and overall timing metrics such as total negative slack (`sta_tns.txt`) and worst negative slack (`sta_wns.txt`). Together, these reports provide a comprehensive view of the BabySoC design’s timing behavior after routing.

<div align="center">
  <img src="Images/88.png" alt="Alt Text" width="1000"/>
</div>

---

## 16. Results of Post-SPEF STA
To present the post-route STA output in a clear and structured manner, a Python-based parsing script was used to automatically extract key timing metrics from the generated reports. This script processes all setup and hold analysis files produced by the STA flow and compiles the essential values—Worst Setup Slack, Worst Hold Slack, WNS, and TNS—into consolidated summary formats.<br><br>
The script is named `analysis_postSPEF_STA.py` and is saved in the following path: `/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/scripts/analysis_postSPEF_STA.py`.<br><br>
The contents of the script is as follows:
```
import re
import pandas as pd
import os

# ============================
# BASE INPUT AND OUTPUT PATHS
# ============================
BASE = "/home/bitopan/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route"
OUTPUT_DIR = BASE

os.makedirs(OUTPUT_DIR, exist_ok=True)

def extract_number(line):
    nums = re.findall(r"[-+]?\d*\.\d+|\d+", line)
    if nums:
        return float(nums[-1])
    return None

# Load corner names
with open(f"{BASE}/sta_worst_max_slack.txt") as f:
    corners = [line.strip() for line in f if line.strip().startswith("sky130")]

worst_setup_slack = []
worst_hold_slack = []
wns_list = []
tns_list = []

# Worst Setup Slack + correct WNS
with open(f"{BASE}/sta_worst_max_slack.txt") as f:
    for line in f:
        if line.startswith("sky130"):
            num_line = f.readline()
            value = extract_number(num_line)
            worst_setup_slack.append(value)

            # ------ CORRECT WNS FORMULA ------
            wns = value if value < 0 else 0.0
            wns_list.append(wns)

# Worst Hold Slack
with open(f"{BASE}/sta_worst_min_slack.txt") as f:
    for line in f:
        if line.startswith("sky130"):
            num_line = f.readline()
            value = extract_number(num_line)
            worst_hold_slack.append(value)

# TNS
with open(f"{BASE}/sta_tns.txt") as f:
    for line in f:
        if line.startswith("sky130"):
            num_line = f.readline()
            value = extract_number(num_line)
            tns_list.append(value)

# Create table
df = pd.DataFrame({
    "Corner": corners,
    "Worst Setup Slack": worst_setup_slack,
    "Worst Hold Slack": worst_hold_slack,
    "WNS": wns_list,
    "TNS": tns_list
})

df.to_csv(f"{OUTPUT_DIR}/sta_summary.csv", index=False)
df.to_markdown(f"{OUTPUT_DIR}/sta_summary.md", index=False)

print(df)
print("\nSaved:")
print(f" - {OUTPUT_DIR}/sta_summary.csv")
print(f" - {OUTPUT_DIR}/sta_summary.md")
```

Then, in the path: `/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/scripts/`, the script is run with the command:
```
python analysis_postSPEF_STA.py
```

<div align="center">
  <img src="Images/97.png" alt="Alt Text" width="1000"/>
</div>

The script reads the complete set of post-route timing reports and generates two output files:
- **sta_summary.csv** — a machine-readable tabulated summary suitable for further processing or analysis.
- **sta_summary.md** — a Markdown-formatted summary that can be directly included in Github, as given below:

| Corner                            |   Worst Setup Slack |   Worst Hold Slack |      WNS |        TNS |
|:----------------------------------|--------------------:|-------------------:|---------:|-----------:|
| sky130_fd_sc_hd__tt_025C_1v80.lib |              6.0318 |             0.2829 |   0      |      0     |
| sky130_fd_sc_hd__ff_100C_1v65.lib |              7.1006 |             0.2285 |   0      |      0     |
| sky130_fd_sc_hd__ff_100C_1v95.lib |              7.987  |             0.1807 |   0      |      0     |
| sky130_fd_sc_hd__ff_n40C_1v56.lib |              5.8201 |             0.2605 |   0      |      0     |
| sky130_fd_sc_hd__ff_n40C_1v65.lib |              6.5581 |             0.2302 |   0      |      0     |
| sky130_fd_sc_hd__ff_n40C_1v76.lib |              7.1948 |             0.204  |   0      |      0     |
| sky130_fd_sc_hd__ss_100C_1v40.lib |             -3.4451 |             0.8342 |  -3.4451 |   -577.608 |
| sky130_fd_sc_hd__ss_100C_1v60.lib |              1.2189 |             0.5914 |   0      |      0     |
| sky130_fd_sc_hd__ss_n40C_1v28.lib |            -30.4255 |             1.5731 | -30.4255 | -14179.2   |
| sky130_fd_sc_hd__ss_n40C_1v35.lib |            -16.9016 |             1.1762 | -16.9016 |  -6609.19  |
| sky130_fd_sc_hd__ss_n40C_1v40.lib |            -11.2889 |             0.9878 | -11.2889 |  -3470.77  |
| sky130_fd_sc_hd__ss_n40C_1v44.lib |             -8.167  |             0.8726 |  -8.167  |  -2072.12  |
| sky130_fd_sc_hd__ss_n40C_1v76.lib |              2.5504 |             0.4465 |   0      |      0     |

Both files are saved automatically at the designated output directory: `/home/bitopan/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/`

---

## 17. Comparison of Post-SPEF STA With Post-Synthesis STA Reports
To perform a meaningful comparison between post-synthesis and post-route timing, it was necessary to revisit the STA performed in [Week 3.](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-3/blob/main/README.md#4-static-timing-analysis-of-vsdbabysoc) The original Week 3 analysis had been executed only for the TT corner (sky130_fd_sc_hd__tt_025C_1v80.lib), which was insufficient for multi-corner comparison against the Week 8 post-route results.<br>
To address this, a new post-synthesis STA run was executed across the same PVT corners used for post-route analysis. This ensured that timing metrics at the synthesis stage and the routing stage were evaluated under identical corner conditions.

### <ins>1. Re-Running Post-Synthesis STA Across All Corners.</ins>
A dedicated TCL script was prepared to load the synthesized netlist, read all required `.lib` files corresponding to the PVT corners, apply the SDC constraints, and run setup/hold timing for each corner. The TCL script was named `sta_post_synth_across_pvt.tcl` and saved in the path: `/home/bitopan/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/sta_post_synth_across_pvt.tcl`.<br><br>
The contents of the TCL script is as follows:

```
### ============================================================
###   VSDBabySoC  –  Post-Synthesis STA  (Standalone OpenSTA)
### ============================================================

puts "\n▶ Starting Post-Synthesis STA for VSDBabySoC...\n"

# --------------------------------------------------------------
# Units
# --------------------------------------------------------------
puts "→ Setting units..."
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
set_units      -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um


# --------------------------------------------------------------
# Liberty Corners
# --------------------------------------------------------------
puts "\n→ Loading corner list..."

set list_of_lib_files(1)  "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2)  "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(3)  "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(4)  "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(5)  "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(6)  "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(7)  "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(8)  "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(9)  "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"


# --------------------------------------------------------------
# Load analog macros
# --------------------------------------------------------------
puts "\n→ Loading DAC/PLL Liberty models..."

read_liberty /data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib
read_liberty /data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib


# --------------------------------------------------------------
# Paths
# --------------------------------------------------------------
set SYN_NETLIST "/data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/1_synth.v"
set SYN_SDC     "/data/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/1_synth.sdc"

set OUT "/data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/synthesis"
exec mkdir -p $OUT

# ----------- THIS FIXES DUPLICATION ----------------
exec echo -n > $OUT/sta_worst_max_slack.txt
exec echo -n > $OUT/sta_worst_min_slack.txt
exec echo -n > $OUT/sta_tns.txt
exec echo -n > $OUT/sta_wns.txt
# ----------------------------------------------------


# --------------------------------------------------------------
# Begin STA loop
# --------------------------------------------------------------
puts "\n============================================================"
puts "              BEGIN POST-SYNTH MULTI-CORNER STA"
puts "============================================================\n"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {

    puts "\n------------------------------------------------------------"
    puts " Running Corner $i  →  $list_of_lib_files($i)"
    puts "------------------------------------------------------------"

    read_liberty /data/VSD_Tapeout_Program/open_pdks/sources/sky130_fd_sc_hd/timing/$list_of_lib_files($i)
    read_verilog $SYN_NETLIST

    link_design vsdbabysoc
    current_design

    read_sdc $SYN_SDC

    report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} \
        > $OUT/min_max_$list_of_lib_files($i).txt

    exec echo "\n$list_of_lib_files($i)" >> $OUT/sta_worst_max_slack.txt
    report_worst_slack -max -digits {4} >> $OUT/sta_worst_max_slack.txt

    exec echo "\n$list_of_lib_files($i)" >> $OUT/sta_worst_min_slack.txt
    report_worst_slack -min -digits {4} >> $OUT/sta_worst_min_slack.txt

    exec echo "\n$list_of_lib_files($i)" >> $OUT/sta_tns.txt
    report_tns -digits {4} >> $OUT/sta_tns.txt

    exec echo "\n$list_of_lib_files($i)" >> $OUT/sta_wns.txt
    report_wns -digits {4} >> $OUT/sta_wns.txt

    puts " → Corner $i complete."
}

puts "\n============================================================"
puts "            END OF POST-SYNTHESIS MULTI-CORNER STA"
puts "============================================================\n"

exit
```

This updated synthesis-level STA generated the complete set of reports for all corners, which were stored at: `/home/bitopan/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/synthesis
`. These reports mirror the structure of the post-route outputs, making them suitable for one-to-one comparison.

The `sta_post_synth_across_pvt.tcl` file is run as follows:
```
docker run -it -v /home/bitopan:/data opensta /data/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/sta_post_synth_across_pvt.tcl
```

<div align="center">
  <img src="Images/89.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
  <img src="Images/90.png" alt="Alt Text" width="1000"/>
</div>
<div align="center">
  <img src="Images/91.png" alt="Alt Text" width="1000"/>
</div>

### <ins>2. Automated Extraction of Synthesis-Level Timing Metrics.</ins>
To process the newly generated synthesis reports, a Python script—similar to the one used in the post-route stage—was used to extract:
- Worst Setup Slack
- Worst Hold Slack
- Worst Negative Slack (WNS)
- Total Negative Slack (TNS)

The Python script is named as `analysis_postSYNTH_STA.py` and stored in the path: `/home/bitopan/VSD_Tapeout_Program/OpenROAD-flow-scripts/flow/scripts/`.<br><br>
The Python script used is as follows:
```
import re
import pandas as pd
import os

# ============================
# BASE INPUT AND OUTPUT PATHS
# ============================
BASE = "/home/bitopan/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/synthesis"
OUTPUT_DIR = BASE

os.makedirs(OUTPUT_DIR, exist_ok=True)


# -----------------------------------------------------
# Helper: extract LAST floating number in a line
# -----------------------------------------------------
def extract_number(line):
    nums = re.findall(r"[-+]?\d*\.\d+|\d+", line)
    if nums:
        return float(nums[-1])
    return None


# -----------------------------------------------------
# Required input files
# -----------------------------------------------------
MAX_FILE = f"{BASE}/sta_worst_max_slack.txt"
MIN_FILE = f"{BASE}/sta_worst_min_slack.txt"
TNS_FILE = f"{BASE}/sta_tns.txt"

for file in [MAX_FILE, MIN_FILE, TNS_FILE]:
    if not os.path.exists(file):
        raise FileNotFoundError(f"\n❌ Missing required STA file:\n{file}\n")


# -----------------------------------------------------
# Load corner names
# -----------------------------------------------------
with open(MAX_FILE) as f:
    corners = [line.strip() for line in f if line.strip().startswith("sky130")]


# Arrays to fill
worst_setup_slack = []
worst_hold_slack = []
wns_list = []
tns_list = []


# -----------------------------------------------------
# Worst Setup Slack + WNS (always ≤ 0)
# -----------------------------------------------------
with open(MAX_FILE) as f:
    for line in f:
        if line.startswith("sky130"):
            val = extract_number(f.readline())

            # Setup slack can be positive, but WNS must be ≤ 0
            worst_setup_slack.append(val)

            # Compute real WNS: min(val, 0)
            wns_list.append(min(val, 0))


# -----------------------------------------------------
# Worst Hold Slack (WHS)
# -----------------------------------------------------
with open(MIN_FILE) as f:
    for line in f:
        if line.startswith("sky130"):
            val = extract_number(f.readline())
            worst_hold_slack.append(val)


# -----------------------------------------------------
# Total Negative Slack (TNS)
# -----------------------------------------------------
with open(TNS_FILE) as f:
    for line in f:
        if line.startswith("sky130"):
            val = extract_number(f.readline())
            tns_list.append(val)


# -----------------------------------------------------
# Create DataFrame
# -----------------------------------------------------
df = pd.DataFrame({
    "Corner": corners,
    "Worst Setup Slack": worst_setup_slack,
    "Worst Hold Slack": worst_hold_slack,
    "WNS": wns_list,
    "TNS": tns_list
})


# -----------------------------------------------------
# Save the outputs
# -----------------------------------------------------
df.to_csv(f"{OUTPUT_DIR}/sta_summary_synth.csv", index=False)
df.to_markdown(f"{OUTPUT_DIR}/sta_summary_synth.md", index=False)

print(df)
print("\nSaved:")
print(f" - {OUTPUT_DIR}/sta_summary_synth.csv")
print(f" - {OUTPUT_DIR}/sta_summary_synth.md")
```

The script is run with the following command:
```
python analysis_postSYNTH_STA.py
```

<div align="center">
  <img src="Images/92.png" alt="Alt Text" width="1000"/>
</div>

The script parsed all synthesis STA outputs and generated two summary files:
- **sta_summary_synth.csv**
- **sta_summary_synth.md**

The Post-Synthesis STA reports are as follows:
| Corner                            |   Worst Setup Slack |   Worst Hold Slack |      WNS |       TNS |
|:----------------------------------|--------------------:|-------------------:|---------:|----------:|
| sky130_fd_sc_hd__tt_025C_1v80.lib |              7.5324 |             0.3096 |   0      |     0     |
| sky130_fd_sc_hd__ff_100C_1v65.lib |              8.3122 |             0.2491 |   0      |     0     |
| sky130_fd_sc_hd__ff_100C_1v95.lib |              8.8805 |             0.196  |   0      |     0     |
| sky130_fd_sc_hd__ff_n40C_1v56.lib |              7.5135 |             0.2915 |   0      |     0     |
| sky130_fd_sc_hd__ff_n40C_1v65.lib |              7.981  |             0.2551 |   0      |     0     |
| sky130_fd_sc_hd__ff_n40C_1v76.lib |              8.3959 |             0.2243 |   0      |     0     |
| sky130_fd_sc_hd__ss_100C_1v40.lib |              0.9453 |             0.9053 |   0      |     0     |
| sky130_fd_sc_hd__ss_100C_1v60.lib |              4.1401 |             0.642  |   0      |     0     |
| sky130_fd_sc_hd__ss_n40C_1v28.lib |            -17.1518 |             1.8296 | -17.1518 | -5778.63  |
| sky130_fd_sc_hd__ss_n40C_1v35.lib |             -7.9686 |             1.3475 |  -7.9686 | -1871.7   |
| sky130_fd_sc_hd__ss_n40C_1v40.lib |             -4.1543 |             1.1249 |  -4.1543 |  -650.898 |
| sky130_fd_sc_hd__ss_n40C_1v44.lib |             -1.8958 |             0.9909 |  -1.8958 |  -121.73  |
| sky130_fd_sc_hd__ss_n40C_1v76.lib |              5.2885 |             0.5038 |   0      |     0     |

Both files were saved in the same synthesis STA directory: `/home/bitopan/VSD_Tapeout_Program/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/synthesis/`. These summaries provided a structured view of timing behavior at the synthesis stage across all PVT corners.

### <ins>3. Post-Synthesis vs Post-Route Comparison.</ins>
Using the CSV summaries from:
- **post-route STA** (`sta_summary.csv`)
- **post-synthesis STA** (`sta_summary_synth.csv`)

a consolidated comparison was performed across all PVT corners. For clarity, each of the four key metrics was visualized separately:

- Worst Setup Slack comparison:
  <div align="center">
    <img src="Images/93.png" alt="Alt Text" width="1000"/>
  </div>

- Worst Hold Slack comparison:
  <div align="center">
    <img src="Images/94.png" alt="Alt Text" width="1000"/>
  </div>

- Worst Negative Slack (WNS) comparison:
  <div align="center">
    <img src="Images/95.png" alt="Alt Text" width="1000"/>
  </div>

- Total Negative Slack (TNS) comparison:
  <div align="center">
    <img src="Images/96.png" alt="Alt Text" width="1000"/>
  </div>

Each metric was plotted in a dedicated graph, providing a corner-by-corner visualization of how timing behavior changes from synthesis to layout-aware analysis. These graphs highlight the impact of parasitics, routing delays, and physical effects introduced after synthesis—offering a complete understanding of timing evolution between Week 3 and Week 8.

---

## 18. Interpretations and Discussions 
The comparison between post-synthesis and post-route STA across all PVT corners offers a clear view into how routing parasitics reshape timing behavior once real interconnect effects are introduced. The trends observed in the four comparison plots highlight consistent physical-design phenomena that emerge after placement, routing, and SPEF extraction.

### <ins>1. Worst Setup Slack (WSS) Trends.</ins>
From the setup slack comparison, several observations stand out:
1. At **typical and fast corners**, the post-route slack closely follows the post-synthesis values, with only minor degradation. This is expected—interconnect delays increase slightly due to added wire resistance and capacitance, but the overall timing margin remains healthy.
2. A sharp setup degradation appears at slow PVT corners, most notably at `sky130_fd_sc_hd__ss_n40C_1v28.lib`, where the post-route Worst Setup Slack drops significantly (≈ −30 ns). The post-synthesis slack at the same corner shows a much smaller violation.<br>
   This divergence is rooted in slow-corner physics:
   - Slow process + low voltage + low temperature → cells switch slower, wire RC becomes dominant, and path delays stretch heavily.
   - Post-synthesis STA does not account for routed interconnect parasitics, so the delay increase is underestimated.
   - SPEF annotation introduces the actual RC load, causing a large drop in setup slack.

### <ins>2. Worst Hold Slack (WHS) Trends.</ins>
The hold slack comparison shows the opposite behavior:
1. Across most corners, post-route hold slack is slightly lower than the post-synthesis values, but the deviation is small.
2. At faster corners, such as FF corners, hold slack tightens noticeably due to reduced intrinsic cell delay.
3. No extreme outliers or catastrophic hold failures appear after routing, which indicates:
   - Balanced routing,
   - Limited insertion of short, ultra-fast paths, and
   - Reasonable clock tree behavior.

Unlike setup, hold timing is impacted more by cell delay than wire delay, and thus shows less variation between synthesis and routed STA.

### <ins>3. Worst Negative Slack (WNS) Comparison.</ins>
The WNS plot reinforces the setup trend:
1. WNS is zero (or very close to zero) at several corners in both synthesis and post-route STA.
2. The dominant violation appears again at the SS low-voltage corner, where post-route WNS drops dramatically (~ −33 ns).
3. The corresponding post-synthesis WNS dip is much smaller, confirming that routing parasitics are the primary contributor.

This emphasizes that design closure must always consider slow process–weak voltage corners, because the parasitic impact becomes disproportionately large there.

### <ins>4. Total Negative Slack (TNS) Comparison.</ins>
TNS amplifies the WNS trend:
1. For most corners, TNS remains zero or near-zero in both analyses.
2. At the slow SS corner, however, post-route TNS spikes sharply, reaching large negative values due to multiple violating paths.
3. The post-synthesis TNS remains relatively small, showing that many near-critical paths collapsed only after interconnect parasitics were applied.

---

## My Unique Contributions
Throughout the development of VSDBabySoC, several challenges emerged that required deeper investigation and custom problem-solving beyond the provided flow. The following points highlight the key interventions I carried out — each involving targeted modifications, clear reasoning, and a demonstrable impact on the physical design results.

### <ins>1. Correcting Macro Placement to Resolve Routing Congestion.</ins> [(Check Here)](#routing-congestion-and-debugging-efforts)

#### <ins>What I changed:</ins>
I modified the `macro_place_util.tcl` script to ensure that macro placement was driven directly by the `macro.cfg` file, rather than the default `rtl_macro_placer` routine that the script was using.

#### <ins>Why I changed it:</ins>
During floorplanning, I noticed that the PLL and DAC macros were not placed at the specified coordinates despite being correctly defined in `macro.cfg`. This mismatch produced severe congestion patterns, including 24 routing overflows, which signaled a deeper issue in the placement utility itself.

#### <ins>How it affected my design flow:</ins>
After correcting the script to properly parse and apply the `macro.cfg` instructions, macro placements aligned correctly with the intended floorplan. As a direct result, routing congestion eased significantly, with overflow count dropping from 24 to 7 — a substantial improvement that stabilized the downstream routing process.

### <ins>2. Fixing DAC LEF Blockage Errors Preventing Successful Routing.</ins> [(Check Here)](#routing-congestion-and-debugging-efforts)

#### <ins>What I changed:</ins>
I inspected and manually edited the `avsddac.lef` file to remove two overlapping OBS (obstruction) blocks that were incorrectly positioned over the DAC's output pin.

#### <ins>Why I changed it:</ins>
Repeated routing failures and persistent congestion around the DAC region led me to suspect a geometry-level issue. Peer discussion with peers pointed me toward the LEF, where I discovered these obstructive geometries directly blocking pin access and causing forced detours in the routing engine.

#### <ins>How it affected my design flow:</ins>
Removing the two problematic OBS blocks immediately resolved the routing deadlock. Upon re-running the routing stage, the tool completed successfully without congestion-triggered failures, allowing the physical design process to progress uninterrupted.

### <ins>3. Developing a Custom Post-SPEF STA Script for Multi-Corner Timing.</ins> [(Check Here)](#6-multi-corner-opensta-tcl-script)

#### <ins>What I changed:</ins>
I reworked the provided `sta_across_pvt_route.tcl` script — which contained execution issues — and produced a refined version titled `sta_across_pvt_route_custom.tcl`.

#### <ins>Why I changed it:</ins>
The original script failed to execute reliably across multiple corners, preventing automated post-SPEF STA for the full set of 13 PVT corners. Accurate timing closure required a consistent and reproducible automated STA run.

#### <ins>How it affected my design flow:</ins>
The custom script enabled seamless post-SPEF STA execution across all 13 corners, generating complete timing reports for each. This automation not only saved time but also ensured uniform analysis conditions necessary for robust timing comparison and corner-based evaluation.

### <ins>4. Automating Multi-Corner Post-SPEF STA Analysis Using Python.</ins> [(Check Here)](#16-results-of-post-spef-sta)

#### <ins>What I changed:</ins>
I developed a Python script to parse all post-SPEF STA reports and extract key timing metrics — Worst Setup Slack, Worst Hold Slack, WNS, and TNS — across all 13 corners, generating two summary files: `sta_summary.md` and `sta_summary.csv`.

#### <ins>Why I changed it:</ins>
Manually parsing STA reports for each corner was error-prone and time-consuming. I needed a structured, automated method to consolidate timing metrics and spot trends efficiently.

#### <ins>How it affected my design flow:</ins>
This automation provided a clean, tabulated comparison of cornerwise timing behavior, making it significantly easier to understand PVT sensitivity, identify critical corners, and verify post-SPEF timing closure in a consistent, analytical manner.

### <ins>5. Creating a Multi-Corner Post-Synthesis STA Script for Accurate Comparison.</ins> [(Check Here)](#1-re-running-post-synthesis-sta-across-all-corners)

#### <ins>What I changed:</ins>
I wrote a new TCL script to perform post-synthesis STA across the same 13 PVT corners, replicating the coverage achieved in the post-SPEF stage.

#### <ins>Why I changed it:</ins>
I discovered that the initial post-synthesis STA (performed in Week 3) covered only one corner. To allow meaningful, one-to-one comparison between post-synthesis and post-SPEF timing, multi-corner synthesis-level STA was essential.

#### <ins>How it affected my design flow:</ins>
This script enabled uniform coverage across all corners, allowing precise evaluation of how parasitics from SPEF altered timing behavior for each operating condition. It made the final timing comparison robust, complete, and technically defensible.

### <ins>6. Automating Post-Synthesis STA Analysis to Match Post-SPEF Workflow.</ins> [(Check Here)](#2-automated-extraction-of-synthesis-level-timing-metrics)

#### <ins>What I changed:</ins>
Mirroring the earlier post-SPEF analysis tool, I developed another Python script to parse the new multilayer synthesis-level STA reports. This script generated `sta_summary_synth.csv` and `sta_summary_synth.md`.

#### <ins>Why I changed it:</ins>
To compare synthesis-level and post-SPEF timing accurately, both datasets needed to be processed through identical analysis pipelines. Manual extraction would have introduced inconsistencies and slowed the evaluation considerably.

#### <ins>How it affected my design flow:</ins>
With automated extraction for both datasets, comparisons became seamless. I could align worst-case metrics cornerwise and study how SPEF-induced parasitic effects shifted timing margins — enabling a clear, data-driven understanding of the design’s timing evolution.

---

## ⚠️ Challenges

### <ins>1. Week 1</ins>
 
 1. **Multiple OUT Signals in GTKWave**:
    While analyzing the waveform, you may notice several `OUT` signals in the signal list. Only the `OUT` signal belonging to the `dac` module should be used for observation. You can find it under the hierarchy: <br>`vsdbabysoc_tb → UUT → DAC → OUT`. <br>See below for reference:
    <div align="center">
      <img src="Images/4.png" alt="Alt Text" width="1000"/>
    </div>

 2. **Analog Waveform Not Visible Initially**:
    The` OUT` signal, even when correctly selected, will initially not appear as Analog signal. To visualize the analog-equivalent output, you need to change its data representation:
    * Right-click on the `OUT` signal in the waveform list.
    * Navigate to Data Format → Analog → Step.

    After this conversion, the waveform will resemble a continuous analog signal corresponding to the digital input values. See below for reference:
    <div align="center">
      <img src="Images/5.png" alt="Alt Text" width="1000"/>
    </div>

### <ins>2. Week 7</ins>

### <ins>Routing Congestion and Debugging Efforts</ins>
The primary challenge encountered this week was severe congestion during the routing stage, which repeatedly caused the flow to halt or produce incomplete routing results. Resolving this issue required a systematic trial-and-error approach—testing multiple hypotheses, modifying flow parameters, and analyzing congestion maps to understand the underlying cause. This section documents each approach that was attempted, the outcome of those attempts, and finally the method that successfully eliminated the congestion and enabled the routing to complete.

1. **Attempt 1: Initial Routing Run – 24 Overflows Observed:**<br>
   The first indication of a routing problem appeared during the initial execution of the routing stage. Without any modifications to the default ORFS configuration, the router reported 24 overflows, signaling that several routing resources were insufficient to accommodate the required connections. This confirmed that congestion was present early in the flow and that further investigation and parameter adjustments were necessary before achieving a clean, fully routed design.

   <div align="center">
   <img src="Images/64.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
   <img src="Images/65.png" alt="Alt Text" width="1000"/>
   </div>

2. **Attempt 2: Correcting Macro Placement to Reduce Congestion**<br>
   After reviewing the congestion map from the initial routing run, it became clear that macro placement was contributing significantly to routing overflow. Using the DRC Viewer GUI, I noticed that the PLL and DAC macros were not positioned at the coordinates specified in the `macro.cfg` file:
   ```
   pll 200 950 N  
   dac 150 250 N
   ```

   However, the GUI showed both macros placed at entirely different locations. Investigating the terminal logs revealed that the flow was not using `macro.cfg` at all—instead, the floorplan step was being driven by `macro_place.tcl`, which invoked `macro_place_util.tcl`.
   <div align="center">
   <img src="Images/66.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
   <img src="Images/67.png" alt="Alt Text" width="1000"/>
   </div>

   Upon inspecting `macro_place_util.tcl`, I found that it was calling the `rtl_macro_placer` instead of reading the coordinates from `macro.cfg`.
   <div align="center">
   <img src="Images/68.png" alt="Alt Text" width="1000"/>
   </div>

   To enforce manual macro placement, I modified `macro_place_util.tcl` by removing the `rtl_macro_placer` call and adding logic to explicitly parse and apply the coordinates from `macro.cfg`. The updated block read the `macro.cfg` file, extracted the macro name, coordinates, and orientation, and placed the macros accordingly using place_macro. In the `macro_place_util.tcl` file, I replaced:
   ```
   log_cmd rtl_macro_placer {*}$all_args
   ```

   with:
   ```
     # Manual macro placement using macro.cfg
   if { [env_var_exists_and_non_empty MACRO_PLACEMENT_CFG] } {

   set fp [open $::env(MACRO_PLACEMENT_CFG) r]
   while {[gets $fp line] >= 0} {
   # skip empty and comment lines
   if {[regexp {^\s*$} $line]} continue
   if {[regexp {^#} $line]} continue

   # Parse: <macro> <x> <y> <orient>
   scan $line "%s %f %f %s" macro x y orient

   puts "Placing macro $macro at ($x, $y) orient $orient"
   place_macro -macro_name $macro -location "$x $y" -orientation $orient
   }
   close $fp
   }
   ```

   After applying this fix and re-running the routing stage, the macros appeared at their correct intended positions. This adjustment yielded an immediate improvement—the number of routing overflows dropped from 24 to 7, indicating that improper macro placement was a significant source of congestion.
   <div align="center">
   <img src="Images/69.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
   <img src="Images/70.png" alt="Alt Text" width="1000"/>
   </div>

3. **Attempt 3: Adjusting DAC Orientation (dac 150 250 MY)**<br>
   To further reduce congestion around the DAC region, I began experimenting with different macro orientations and slight coordinate shifts. The first significant test involved mirroring the DAC macro along the Y-axis while keeping its original coordinates.

   **Result:** The number of routing overflows dropped to 2, indicating a major improvement and suggesting that local pin access and routing channels were more favorable in this orientation.

   <div align="center">
   <img src="Images/71.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
   <img src="Images/72.png" alt="Alt Text" width="1000"/>
   </div>

4. **Attempt 4: Rotating DAC by 180 Degrees (dac 150 250 R180)**<br>
   Next, I tested a 180-degree rotation of the DAC macro at the same coordinates. Although rotations can sometimes open cleaner routing paths, this particular configuration proved counterproductive.

   **Result:** Overflows increased to 13, showing that this rotation created more routing blockages or difficult pin-access situations around the DAC.
   <div align="center">
   <img src="Images/73.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
   <img src="Images/74.png" alt="Alt Text" width="1000"/>
   </div>

5. **Attempt 5: Slight Coordinate Shift with MY Orientation (dac 160 250 MY)**<br>
   Finally, I attempted a small horizontal shift while keeping the improved MY orientation. Minor adjustments like this can help redistribute local congestion or improve via access.
   **Result:** The overflow count stabilized at 3, better than many other trials but still not as effective as Attempt 3.
   <div align="center">
   <img src="Images/75.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
   <img src="Images/76.png" alt="Alt Text" width="1000"/>
   </div>

6. **Attempt 6: Modifying Macro Halos with the MY Orientation**<br>
   Since the MY orientation yielded the best results in earlier trials, the next approach focused on adjusting the halo (keep-out margin) around the DAC macro to potentially relieve local routing pressure. By increasing the spacing around the macro, the aim was to create wider routing channels and reduce congestion in the surrounding region.

   **Result:** Despite these adjustments, the overflow count remained at 3, showing no meaningful improvement over previous attempts. This indicated that halo modifications alone were insufficient to resolve the remaining routing bottlenecks.
   <div align="center">
   <img src="Images/77.png" alt="Alt Text" width="1000"/>
   </div>
   <div align="center">
   <img src="Images/78.png" alt="Alt Text" width="1000"/>
   </div>

7. **Final Attempt: Identifying and Removing Problematic OBS Blocks in `avsddac.lef`**<br>
   After multiple unsuccessful trials involving macro repositioning, orientation changes, and halo adjustments, I consulted peers for additional insight into the routing bottleneck. Following their suggestions, I inspected the `avsddac.lef` file more closely and discovered that the macro contained two OBS (obstruction) entries on the met4 layer positioned directly above the DAC’s output pin. These obstructions were unintentionally blocking routing resources at a critical point, preventing the router from accessing the DAC pin cleanly and causing persistent congestion around the macro.

   <div align="center">
   <img src="Images/79.png" alt="Alt Text" width="1000"/>
   </div>

   To eliminate this blockage, I removed the two offending OBS sections from the LEF file, ensuring that the routing tool had a clear path to the macro’s output.

   **Result:** After making this correction and re-running the routing stage, the flow completed successfully with zero overflows, confirming that the met4 obstructions were the root cause of the routing failure.

   <div align="center">
   <img src="Images/80.png" alt="Alt Text" width="1000"/>
   </div>

   The routed layout was inspected in the OpenROAD GUI:
   <div align="center">
   <img src="Images/81.png" alt="Alt Text" width="1000"/>
   </div>   
   <div align="center">
   <img src="Images/82.png" alt="Alt Text" width="1000"/>
   </div>   
   <div align="center">
   <img src="Images/83.png" alt="Alt Text" width="1000"/>
   </div>   

---

## 🏁 Final Remarks
Week 9 brought the entire VSDBabySoC journey full circle. This was the moment where every stage — from RTL and synthesis to routing, SPEF extraction, and STA — was consolidated into a single, coherent GitHub repository. The documentation now captures not just the standard flow, but also the custom fixes, debugging efforts, and automated analysis tools that shaped the final design.

By organizing all visuals, reports, and scripts, this week transformed months of work into a clear, review-ready record. With multi-corner timing results aligned for both post-synthesis and post-SPEF stages, and all unique contributions formally highlighted, the project now stands complete, transparent, and ready for evaluation or future extension. Week 9, therefore, marks the formal closing of the design journey and the handoff of a fully documented SoC implementation.

>[!IMPORTANT]
> For easy navigation to all the weeks of the program, please visit the [Master Repository](https://github.com/BitopanBaishya/VSD-Tapeout-Program-2025.git).

