"Task 1 – Week 1: Research the fundamentals of SRAM design using open-source tools and study the reference GitHub repo https://github.com/ShonTaware/SRAM_SKY130, which focuses on designing a 1024x32 SRAM, 32 Kbits / 4 KB, using OpenRAM and SKY130 PDK with 1.8 V operation and target access time below 2.5 ns. Your Week 1 goal is not to fully reproduce the SRAM compiler output immediately, but to understand how similar results can be approached using AI-assisted workflows. Break the problem into small blocks such as SRAM architecture, 6T SRAM cell operation, read/write/precharge/sense amplifier/write driver blocks, SKY130 PDK setup, OpenRAM installation, OpenRAM technology configuration, custom cell files such as GDS/SPICE/LEF/LIB, SRAM compiler configuration, pre-layout simulations, post-layout expectations, possible tool errors and debugging strategy. You may use ChatGPT, Codex or any AI tool of your choice to understand the repo, generate step-by-step prompts, prepare setup/debug commands, study scripts and create the report content, but every point written in the report must be understood and verified by you. As a hint, start by asking an AI tool to summarize the repo and convert it into smaller executable research tasks; then ask it to create Codex-style prompts for exploring OpenRAM, SKY130 PDK integration, SRAM cell simulations and compiler output generation. Document what inputs are required, what outputs are expected, what may fail during setup, and how you would fix the flow block by block. For this week, you are required to submit only a compact 1-page IEEE two-column report. No working SRAM layout, OpenRAM run, simulation waveform, final GDS, screenshots or tool output is mandatory in Week 1."

Yes. The current tracker is still **module-level**. In industry, each module is decomposed into **micro-tasks** that can be completed in 15–60 minutes and objectively marked as:

* Not Started
* Reading
* Understood
* Practiced
* Verified
* Documented
* Reviewed

For this SRAM internship, I would break the entire plan into the following hierarchy:

```text
Program
 ├── Module
 │    ├── Topic
 │    │    ├── Task
 │    │    │    ├── Subtask
 │    │    │    │    ├── Evidence
 │    │    │    │    └── Review
```

---

# PHASE 0 – ENVIRONMENT UNDERSTANDING

## Step 0.1

What is SRAM?

Deliverable:

```text
1-page summary
```

---

## Step 0.2

Why SRAM?

Compare:

* SRAM
* DRAM
* Register File

Deliverable:

Comparison table

---

## Step 0.3

What is 4 KB SRAM?

Calculate:

```text
1024 × 32
```

Deliverable:

Manual derivation

---

## Step 0.4

Derive address width

Calculate:

```text
log2(1024)
```

Deliverable:

10-bit explanation

---

# PHASE 1 – REPOSITORY STUDY

## Step 1.1

Clone repo

Evidence:

```bash
git clone
```

---

## Step 1.2

Read README

Deliverable:

1-page notes

---

## Step 1.3

Draw repository structure

Understand:

```text
compiler/
technology/
characterizer/
```

Deliverable:

Directory tree

---

## Step 1.4

Identify inputs

Document:

* Config
* PDK
* Custom Cells

---

## Step 1.5

Identify outputs

Document:

* GDS
* LEF
* LIB
* SPICE
* Verilog

---

# PHASE 2 – SRAM ARCHITECTURE

## Step 2.1

Draw complete SRAM block diagram

Include:

* Decoder
* Wordline
* Array
* Sense Amp
* Write Driver

---

## Step 2.2

Understand decoder

Subtasks:

### What is decoding?

### Why 1-of-1024?

### Delay source?

### Area source?

---

## Step 2.3

Understand wordline

Subtasks:

### Function

### Driver sizing

### Delay contribution

---

## Step 2.4

Understand bitlines

Subtasks:

### BL

### BLB

### Differential operation

### Capacitance

---

## Step 2.5

Understand column mux

Subtasks:

### Why needed

### Area tradeoff

### Speed tradeoff

---

# PHASE 3 – 6T SRAM CELL

Most important phase.

---

## Step 3.1

Draw 6T cell

Label:

* PU1
* PU2
* PD1
* PD2
* AX1
* AX2

---

## Step 3.2

Study hold operation

Questions:

* Current path?
* Stable nodes?
* Failure modes?

---

## Step 3.3

Study read operation

Questions:

* BL behavior?
* BLB behavior?
* Read disturb?

---

## Step 3.4

Study write operation

Questions:

* Overwrite mechanism?
* Failure mechanism?

---

## Step 3.5

Study transistor sizing

Why:

```text
Pull-down > Access > Pull-up
```

---

# PHASE 4 – MEMORY STABILITY

Industry-important.

---

## Step 4.1

Understand SNM

Deliverable:

Definition

---

## Step 4.2

Read SNM

Deliverable:

Notes

---

## Step 4.3

Write SNM

Deliverable:

Notes

---

## Step 4.4

Hold SNM

Deliverable:

Notes

---

## Step 4.5

Butterfly Curve

Understand purpose

---

## Step 4.6

Cell Ratio

Study:

```text
PD/AX
```

---

## Step 4.7

Pull-Up Ratio

Study:

```text
PU/AX
```

---

## Step 4.8

Read Disturb

Understand mechanism

---

## Step 4.9

Half Select Disturb

Understand mechanism

---

## Step 4.10

Write Margin

Understand significance

---

# PHASE 5 – PERIPHERAL CIRCUITS

---

## Step 5.1

Precharge Circuit

Subtasks:

* Purpose
* Inputs
* Outputs

---

## Step 5.2

Equalization Circuit

Subtasks:

* Why equalize?
* Timing

---

## Step 5.3

Decoder Circuit

Subtasks:

* NAND implementation
* Hierarchical decoder

---

## Step 5.4

Wordline Driver

Subtasks:

* Buffer chain
* Fanout

---

## Step 5.5

Sense Amplifier

Study:

* Voltage sense
* Current sense
* Latch sense

Comparison matrix.

---

## Step 5.6

Write Driver

Study:

* Drive strength
* Write failure

---

# PHASE 6 – OPENRAM

---

## Step 6.1

What is OpenRAM?

Deliverable:

Architecture diagram

---

## Step 6.2

Study compiler flow

```text
Config
↓
Compiler
↓
Generated SRAM
```

---

## Step 6.3

Study sram.py

Questions:

* Inputs?
* Outputs?

---

## Step 6.4

Study config file

Parameters:

* word_size
* num_words
* tech_name

---

## Step 6.5

Study bitcell.py

Purpose

---

## Step 6.6

Study characterizer

Purpose

---

# PHASE 7 – SKY130

---

## Step 7.1

Understand PDK

Deliverable:

PDK notes

---

## Step 7.2

Study devices

* NMOS
* PMOS

---

## Step 7.3

Study models

* BSIM

---

## Step 7.4

Study layers

* Poly
* LI
* M1
* M2
* M3
* M4
* M5

---

## Step 7.5

Study corners

* TT
* SS
* FF
* SF
* FS

---

# PHASE 8 – GENERATED FILES

---

## Step 8.1

Verilog

Purpose

---

## Step 8.2

SPICE

Purpose

---

## Step 8.3

GDS

Purpose

---

## Step 8.4

LEF

Purpose

---

## Step 8.5

LIB

Purpose

---

# PHASE 9 – VERIFICATION

---

## Step 9.1

Functional

Read

Write

Hold

---

## Step 9.2

DRC

What checked?

---

## Step 9.3

LVS

What checked?

---

## Step 9.4

PEX

What extracted?

---

# PHASE 10 – CHARACTERIZATION

---

## Step 10.1

Access Time

Definition

---

## Step 10.2

Setup

Definition

---

## Step 10.3

Hold

Definition

---

## Step 10.4

Read Power

Definition

---

## Step 10.5

Write Power

Definition

---

## Step 10.6

Leakage

Definition

---

## Step 10.7

NLDM

Awareness

---

## Step 10.8

CCS

Awareness

---

## Step 10.9

LVF

Awareness

---

# PHASE 11 – VARIATION

---

## Step 11.1

Process Variation

---

## Step 11.2

Voltage Variation

---

## Step 11.3

Temperature Variation

---

## Step 11.4

Monte Carlo

Purpose

---

## Step 11.5

Yield

Purpose

---

# PHASE 12 – DEBUG

---

## Step 12.1

Missing PDK

---

## Step 12.2

Wrong Environment Variables

---

## Step 12.3

Missing Dependencies

---

## Step 12.4

DRC Errors

---

## Step 12.5

LVS Errors

---

## Step 12.6

SPICE Convergence Errors

---

## Step 12.7

Timing Failures

Decoder

Wordline

Bitline

Sense Amp

---

# PHASE 13 – AI WORKFLOW

---

## Step 13.1

Repository Summarization

---

## Step 13.2

Code Explanation

---

## Step 13.3

Command Generation

---

## Step 13.4

Debug Assistance

---

## Step 13.5

Documentation Generation

---

# PHASE 14 – REPORT SUBMISSION

---

## Step 14.1

Abstract

---

## Step 14.2

Architecture Section

---

## Step 14.3

Methodology Section

---

## Step 14.4

Debug Strategy Section

---

## Step 14.5

Conclusion

---

## Step 14.6

Future Work

* RISC-V Integration
* FPGA Correlation
* ASIC Memory Subsystem

This breakdown results in roughly **120–150 measurable micro-tasks**, each with a clear deliverable, and is much closer to how a senior SRAM engineer or research fellow would decompose the Week-1 research objective.

