Perfect. I reviewed the reference repository. The good news is that it already contains many of the exact circuit blocks your assignment mentions (6T SRAM cell, precharge, sense amplifier, write driver, SNM, etc.), but it presents them mainly as completed designs and simulation outputs.([GitHub][1])

**Our handbook will fill the missing gap:** instead of simply showing the circuits, it will explain **how to understand, recreate, verify, and debug each block using AI tools**, exactly as your Week 2 & Week 3 task requires.

---

# Chapter 1

# Development Environment & AI-Assisted Workflow Setup

**Week 2 – Foundation**

---

# Chapter Objective

Before designing SRAM circuits, we need a reproducible engineering environment. By the end of this chapter, you will have:

* A working environment for circuit simulation.
* A GitHub repository organized for AI-assisted development.
* A workflow for using ChatGPT/Codex effectively.
* A repeatable method to document prompts, simulations, errors, and observations.
* A baseline verification that the tools and SKY130 models are working correctly.

**Expected Deliverable:** A GitHub repository ready to document every circuit block explored in Weeks 2 and 3.

---

# 1. Understanding the Assignment

The assignment does **not** ask you to generate a complete SRAM macro.

Instead, it expects you to:

* Understand each SRAM circuit block.
* Recreate it using AI assistance.
* Simulate it with open-source tools.
* Verify the results.
* Document the process in GitHub.

Think of yourself as a **circuit verification engineer** rather than an SRAM compiler developer.

---

# 2. Reference Repository Analysis

The reference repository demonstrates a 1024×32 SRAM design using the SKY130 PDK and includes schematic-level circuit blocks, ngspice simulations, stability analysis (including butterfly curves), and supporting circuits such as precharge, write driver, sense amplifier, tri-state buffer, and D flip-flop. It also provides simulation commands for these blocks.([GitHub][1])

Our work **extends** that repository by adding:

| Repository Contains | Our Handbook Adds        |
| ------------------- | ------------------------ |
| Circuit schematic   | Circuit explanation      |
| Netlist             | Netlist derivation       |
| Simulation          | Simulation methodology   |
| Waveform            | Waveform interpretation  |
| Result              | Debugging process        |
| Circuit             | AI prompt history        |
| SNM graph           | Mathematical explanation |
| Working design      | Engineering observations |

---

# 3. Engineering Workflow

Instead of randomly opening circuits, follow the same workflow for every chapter.

```text
Understand Theory
        │
        ▼
Ask AI
        │
        ▼
Generate Circuit
        │
        ▼
Verify Circuit
        │
        ▼
Simulate
        │
        ▼
Debug
        │
        ▼
Document
        │
        ▼
Commit to GitHub
```

This workflow will be repeated throughout the handbook.

---

# 4. Required Software

The following tools are sufficient for Week 2 and Week 3:

| Tool               | Purpose                           |
| ------------------ | --------------------------------- |
| Git                | Version control                   |
| xschem             | Schematic capture                 |
| ngspice            | Circuit simulation                |
| SKY130 PDK         | Device models                     |
| ChatGPT/Codex      | AI-assisted circuit understanding |
| Image capture tool | Waveform documentation            |

No layout or OpenRAM flow is required for this assignment.

---

# 5. GitHub Repository Structure

```text
SRAM_SKY130_AI/
│
├── README.md
│
├── Week2/
│   ├── Chapter01_Environment/
│   ├── Chapter02_AI_Workflow/
│   ├── Chapter03_6T_SRAM/
│   ├── Chapter04_Hold/
│   ├── Chapter05_Read/
│   ├── Chapter06_Read_Disturb/
│   ├── Chapter07_Write/
│   ├── Chapter08_Write_Margin/
│   ├── Chapter09_SNM/
│   ├── Chapter10_Cell_Ratio/
│   └── Chapter11_Pullup_Ratio/
│
├── Week3/
│   ├── Chapter12_Bitlines/
│   ├── Chapter13_Precharge/
│   ├── Chapter14_Wordline/
│   ├── Chapter15_Write_Driver/
│   ├── Chapter16_Sense_Amp/
│   ├── Chapter17_Row_Decoder/
│   ├── Chapter18_Column_Decoder/
│   ├── Chapter19_Timing/
│   ├── Chapter20_Simulation/
│   ├── Chapter21_AI_Prompts/
│   ├── Chapter22_Debugging/
│   └── Chapter23_Final_Report/
```

---

# 6. Standard Chapter Folder

Every chapter follows the same structure.

```text
ChapterXX/

README.md

prompts.md

observations.md

debug_notes.md

simulation.md

images/

waveforms/

spice/

xschem/

references.md
```

---

# 7. AI Prompt Logging

The assignment specifically asks you to document AI usage.

For every interaction, record:

| Field        | Example                  |
| ------------ | ------------------------ |
| Date         | 2026-07-05               |
| AI Tool      | ChatGPT GPT-5.5          |
| Objective    | Generate 6T SRAM netlist |
| Prompt       | Original prompt          |
| AI Output    | Generated circuit        |
| Verification | Passed/Failed            |
| Corrections  | Manual edits             |
| Observations | Notes                    |

This demonstrates responsible AI-assisted engineering rather than blind reliance on AI.

---

# 8. Prompt Design Guidelines

Use concise, task-focused prompts.

### Good Example

> Generate a SKY130 ngspice netlist for a 6T SRAM cell with labeled pull-up, pull-down, and access transistors.

### Poor Example

> Explain everything about SRAM.

Short prompts generally produce more focused and verifiable results.

---

# 9. Documentation Strategy

Each circuit should include:

* Circuit purpose.
* AI prompt.
* Generated netlist.
* Manual review.
* Simulation command.
* Expected waveform.
* Actual waveform.
* Errors encountered.
* Fixes applied.
* Lessons learned.

This structure makes the repository reproducible.

---

# 10. First Verification Test

Before studying SRAM, confirm the environment is working.

Perform a simple CMOS inverter simulation.

Verify:

* xschem opens correctly.
* SKY130 devices load.
* ngspice runs without errors.
* A voltage transfer characteristic or transient response is produced.

Only after this sanity check should you proceed to SRAM circuits.

---

# 11. Common Installation Problems

| Problem                | Likely Cause                       | Typical Fix                            |
| ---------------------- | ---------------------------------- | -------------------------------------- |
| Device model not found | Incorrect SKY130 path              | Check `.include` statements            |
| Unknown subcircuit     | Missing library file               | Verify PDK installation                |
| Floating node warning  | Unconnected net                    | Inspect schematic connectivity         |
| Convergence error      | Poor initial conditions            | Add `.ic` or adjust simulation options |
| Flat waveform          | Incorrect power supply or stimulus | Verify VDD and input sources           |

Keep a record of each issue and its resolution.

---

# 12. Industry Practice

Circuit designers rarely trust the first simulation.

A common review process is:

1. Review the schematic.
2. Review the generated netlist.
3. Run DC simulation.
4. Run transient simulation.
5. Check expected operating points.
6. Investigate anomalies.
7. Document findings.

Developing this discipline early will improve both your report and your engineering practice.

---

# 13. Chapter Deliverables

By the end of Chapter 1, your repository should contain:

* Environment setup notes.
* Tool versions.
* Repository structure.
* AI prompt log template.
* Initial inverter verification.
* First simulation screenshot.
* Troubleshooting notes.
* Personal observations.

---

# 14. Industry Gap

Many engineers install the tools and immediately begin designing circuits.

Experienced circuit designers first verify:

* The PDK is correctly referenced.
* Device models behave as expected.
* Simulation outputs match theory.
* Documentation is reproducible.

This verification-first mindset saves significant debugging time later.

---

# 15. Summary

Chapter 1 established the engineering foundation for the project. Rather than focusing on SRAM circuits immediately, we created a reproducible workflow for AI-assisted circuit development using xschem, ngspice, and the SKY130 PDK. We analyzed the reference repository, defined a consistent GitHub structure, established prompt logging practices, and introduced an industry-style verification process. This ensures that every circuit studied in the following chapters can be recreated, simulated, validated, and documented in a consistent manner.

---

# Chapter 2

# AI-Assisted Circuit Engineering Workflow for SRAM Design

**Week 2 – Foundation**

> *"The objective is not to let AI design SRAM for you. The objective is to use AI as a junior engineer that helps you understand, generate, verify, debug, and document SRAM circuit blocks."*

---

# Chapter Objective

By the end of this chapter, you will be able to:

✓ Use ChatGPT/Codex effectively for SRAM circuit study.

✓ Generate meaningful low-token prompts.

✓ Generate SPICE netlists.

✓ Generate xschem reconstruction steps.

✓ Debug simulation failures.

✓ Verify AI-generated outputs.

✓ Maintain AI prompt logs required by the assignment.

✓ Build a reusable SRAM prompt library.

---

# 1. Why AI is Part of the Assignment

The assignment specifically states:

> Use ChatGPT, Codex or similar AI tools to understand and recreate SRAM circuit blocks.

The goal is **not automation**.

The goal is:

```text
Engineer
   +
AI Assistant
   =
Faster Learning
```

The expected workflow is:

```text
Theory
   ↓
AI Prompt
   ↓
Generated Circuit
   ↓
Verification
   ↓
Simulation
   ↓
Debugging
   ↓
Documentation
```

---

# 2. Common Mistake

Most beginners use prompts like:

> Explain SRAM.

This usually produces:

* Huge responses.
* Generic information.
* Little engineering value.

Instead:

Break the problem into tiny blocks.

Example:

```text
6T Cell

↓

Hold Operation

↓

Read Operation

↓

Read Disturb

↓

Write Operation

↓

SNM

↓

Precharge
```

This produces focused outputs.

---

# 3. AI Usage Strategy

For every SRAM block, use AI in 5 phases.

---

## Phase 1

Theory Understanding

Prompt:

```text
Explain the purpose of the access transistors in a 6T SRAM cell in less than 200 words.
```

Goal:

Understand concept.

---

## Phase 2

Circuit Generation

Prompt:

```text
Generate a transistor-level 6T SRAM schematic using SKY130 naming conventions.
```

Goal:

Create circuit.

---

## Phase 3

SPICE Generation

Prompt:

```text
Generate an ngspice netlist for the above circuit.
```

Goal:

Create simulation.

---

## Phase 4

Debugging

Prompt:

```text
ngspice reports a floating node at Q. Suggest possible causes.
```

Goal:

Fix simulation.

---

## Phase 5

Verification

Prompt:

```text
Review this netlist and identify potential SRAM functionality issues.
```

Goal:

Validate output.

---

# 4. Low-Token Prompt Library

The assignment specifically mentions low-token prompts.

The following format works well.

---

## Theory Prompt

```text
Explain read disturb in a 6T SRAM cell.
```

---

## Circuit Prompt

```text
Generate a 6T SRAM transistor schematic.
```

---

## Netlist Prompt

```text
Generate ngspice netlist for 6T SRAM.
```

---

## Waveform Prompt

```text
Expected waveforms during SRAM read?
```

---

## Debug Prompt

```text
Why does BL not discharge?
```

---

## Verification Prompt

```text
Review this SRAM netlist for errors.
```

---

Industry observation:

Short prompts often outperform very long prompts.

---

# 5. Prompt Template for Every Chapter

Create:

```text
Goal

Context

Output Required
```

Example:

```text
Goal:
Understand write margin.

Context:
6T SRAM using SKY130.

Output:
Explanation and ngspice setup.
```

This structure improves AI consistency.

---

# 6. AI Prompt Categories

For this project we need only six categories.

---

## Category 1

Theory

Example:

```text
Explain butterfly curve generation.
```

---

## Category 2

Circuit Creation

Example:

```text
Generate SRAM precharge circuit.
```

---

## Category 3

Simulation

Example:

```text
Generate transient simulation setup.
```

---

## Category 4

Waveform Analysis

Example:

```text
Interpret this SRAM read waveform.
```

---

## Category 5

Debugging

Example:

```text
Why is SNM calculation incorrect?
```

---

## Category 6

Documentation

Example:

```text
Summarize observations from this simulation.
```

---

# 7. AI Hallucination Examples

AI occasionally generates:

* Wrong transistor connections.
* Missing power rails.
* Wrong node names.
* Invalid SKY130 model names.
* Impossible timing sequences.

Example:

AI says:

```text
PMOS source connected to ground.
```

Immediate red flag.

Always verify.

---

# 8. Verification Checklist

Before accepting any AI-generated circuit:

### Check 1

Power connections

```text
VDD correct?
GND correct?
```

---

### Check 2

Transistor orientation

```text
Source correct?
Drain correct?
```

---

### Check 3

Control signals

```text
WL connected?
BL connected?
BLB connected?
```

---

### Check 4

Cross-coupling

```text
Q drives QB inverter?
QB drives Q inverter?
```

---

### Check 5

Simulation completeness

```text
Power source?
Input source?
.tran command?
.end present?
```

---

# 9. AI Workflow for Week 2

For every chapter:

```text
Theory Prompt

↓

Circuit Prompt

↓

Netlist Prompt

↓

Simulation Prompt

↓

Debug Prompt

↓

Observation Prompt
```

Repeat.

---

# 10. AI Workflow for Week 3

For peripherals:

```text
Precharge

↓

Sense Amplifier

↓

Write Driver

↓

Decoder

↓

Timing
```

Same methodology.

---

# 11. Example Complete Workflow

Topic:

Read Disturb

---

Prompt 1

```text
Explain read disturb in a 6T SRAM cell.
```

---

Prompt 2

```text
Generate transistor-level read-disturb testbench.
```

---

Prompt 3

```text
Generate ngspice deck.
```

---

Prompt 4

```text
Expected waveforms during read disturb?
```

---

Prompt 5

```text
Review netlist for read-stability issues.
```

---

Prompt 6

```text
Summarize observations from simulation.
```

---

Result:

Complete documentation.

---

# 12. GitHub Prompt Log Format

Create:

```text
prompts.md
```

Template:

```markdown
## Prompt ID: RD01

Tool:
ChatGPT GPT-5.5

Objective:
Understand read disturb

Prompt:
Explain read disturb in a 6T SRAM cell.

Result:
Generated explanation.

Verification:
Verified against simulation.

Observation:
Read disturb increases when access transistor becomes stronger.
```

Repeat for every prompt.

---

# 13. Recommended AI Tools

For this assignment:

| Tool           | Usage                       |
| -------------- | --------------------------- |
| ChatGPT        | Theory, circuits, debugging |
| GitHub Copilot | SPICE editing               |
| Codex          | Netlist generation          |
| Claude         | Alternative explanations    |
| Gemini         | Cross-checking              |

Do not trust any tool blindly.

---

# 14. Industry Practice

At companies such as Intel Corporation, AMD, NVIDIA and Qualcomm:

AI is becoming a productivity tool.

However:

```text
Engineer remains responsible.
```

Simulation results always override AI output.

The industry mindset is:

```text
Trust Simulation

Not AI
```

---

# 15. Industry Gap

Many engineers use AI as:

```text
Question

↓

Copy Answer

↓

Done
```

Professional engineers use:

```text
Question

↓

AI Output

↓

Review

↓

Simulation

↓

Verification

↓

Documentation
```

The second workflow is what this assignment expects.

---

# 16. Chapter Deliverables

By end of Chapter 2:

```text
Week2/
 └── Chapter02_AI_Workflow/
      ├── prompts.md
      ├── verification_checklist.md
      ├── workflow.md
      ├── observations.md
      └── report.md
```

Contents:

* Prompt library.
* Verification checklist.
* Example workflows.
* AI hallucination examples.
* Observations.

---

# 17. Interview Questions

1. Why should AI-generated circuits be verified?
2. What is a good low-token prompt?
3. How would you debug an AI-generated SRAM netlist?
4. Why is simulation more trustworthy than AI output?
5. What information should be recorded in a prompt log?
6. What are common AI mistakes in circuit generation?

---

# 18. Summary

This chapter established a disciplined AI-assisted engineering workflow for the entire Week 2 and Week 3 project. We defined prompt categories, low-token prompt patterns, verification procedures, debugging strategies, and GitHub documentation practices. The key takeaway is that AI should accelerate understanding and circuit creation, but every generated circuit, netlist, waveform, and conclusion must be validated through engineering review and simulation.

---

# Preview of Chapter 3

**Chapter 3 – 6T SRAM Bitcell Architecture**

We now begin the actual SRAM study. Using the reference repository and AI-assisted reconstruction, we will dissect the complete 6T SRAM cell transistor by transistor, identify the role of each device, understand why exactly six transistors are used, recreate the circuit in xschem, generate an ngspice netlist, and prepare the foundation for hold, read, write, SNM, and stability analysis in subsequent chapters. This is where the real circuit-level learning begins.


Excellent. From **Chapter 3**, we start the actual SRAM circuit study. This chapter is the **foundation of the entire Week 2 & Week 3 work**. Every later topic (hold, read, write, SNM, read disturb, sense amplifier, precharge, etc.) depends on mastering this chapter first.

---

# Week 2 – Chapter 3

# 6T SRAM Bitcell Architecture & Circuit Understanding

> **Reference:** Continue from the reference repository and **recreate** the 6T SRAM bitcell using AI-assisted prompts, xschem, ngspice, and SKY130 models. The goal is understanding and verification—not layout or compiler flow.

---

# Chapter Objectives

After completing this chapter, you will be able to:

* Explain why the industry standard SRAM cell uses **6 transistors**.
* Identify the purpose of each transistor.
* Understand every node inside the bitcell.
* Recreate the 6T SRAM schematic in xschem.
* Generate an ngspice netlist using AI.
* Verify AI-generated circuits.
* Simulate the cell in **Hold Mode** (the simplest operating state).
* Document the work in GitHub.

---

# 1. Where Does the 6T SRAM Cell Fit?

The complete SRAM hierarchy is:

```text
SRAM Array
    │
    ├── Row Decoder
    ├── Column Decoder
    ├── Sense Amplifier
    ├── Write Driver
    ├── Precharge Circuit
    │
    ▼
6T SRAM Bitcell
```

The **6T SRAM Bitcell** is the smallest storage element.

Everything else exists only to access or control these cells.

> **Industry Note:** A modern CPU cache may contain **millions** of identical 6T SRAM cells. Improving one transistor width by even a few nanometers can significantly affect total chip area, power, and yield.

---

# 2. What is a 6T SRAM Cell?

A 6T SRAM cell stores **one bit** using:

* Two cross-coupled CMOS inverters.
* Two NMOS access transistors.

It stores:

* Logic '1'
* Logic '0'

as **stable analog voltage states**, not abstract digital values.

---

# 3. Circuit Diagram

![Image](https://images.openai.com/static-rsc-4/OFUsToA3QbTCSQPH7GYRemyHCUsyhEu6i6A8PgIcdVjRWLOoLdHWR4Y_MRLgeLsQRSHN58cR_OPhpSiEtxyCZrl4v8CMOKypuDMNjSFr0ss6lZdOCZWOJxaI-SVD-NGL1FZQ6jUSVroxWewH3GuXIFvVXbzV7x2QctKwkfveP_IGNY7Ekk7V0MW--v9sttAJ?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/OVNIWuF0cV9QcxDilUN-DtHV0mnlWYAW9WkS-H_yg2ApDpOVuO9di25OT83XfAaKCXBYvB5mkiVMwHAurPlIlt-g6FencQ_1zkr9jeDSyYAudOay2lfG8ANTMuzjvmNcDNQiTQB49P05z-0FtwZBJed15R6OS4hSQSOZNjwsJX_kBM3vlWhMflr9D5yvqDos?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/oERj2W2CpeKgXYCzMkRsIu5F6KO46VW8nSVXd-IoAVxn_vNOog0Ou3CahMeANSzql8ttP9kRPgNkbuJNp46A20drblqg-FYaa3VFbWVEIfHDCdm8K8hpTkx1n8jliqYeIDGzdURRUiVFd8ycwWfrWoiUCyoDjQY_1s9Z5iF1szXbG6M-SB9iaQ0iC8lspzCL?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gidtSj7_qHJJy7rlitkp9gVIsxpQnckqfVJtjPvAP9MXo204ukQuJJKK83F6f-WufHMrvI0SqKFs_PX_Le2HsG0oRaV7C_myCO0L2L8ROkWgAXp0Hj_qA-Uhw6kfhfjr2Zfwk4W5qOYc-W_Y76jUfsi2qf760ObAqCWlsJLB1AIUimOaG9tu_BV118s3gxPb?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/BhoVOsaXLFE5ZECo9la4CySUOADdpuy39hp3UdTCEaL_NCZyMTZO8RthwPM37TQypOCvGha6vUeDISm_7yhVmgj5h5ZhQQfLrtPC5PjCYf194Ch43PuvH9fVD4cuyNbaFEH7tXwks-Zn1etnEDTzvg2fRPzgpuKhVc90w3BDo4lqb5zBczHvQPg641Hg46ZA?purpose=fullsize)

A typical transistor naming convention:

| Transistor | Type | Function                   |
| ---------- | ---- | -------------------------- |
| P1         | PMOS | Pull-up (left inverter)    |
| N1         | NMOS | Pull-down (left inverter)  |
| P2         | PMOS | Pull-up (right inverter)   |
| N2         | NMOS | Pull-down (right inverter) |
| A1         | NMOS | Left access transistor     |
| A2         | NMOS | Right access transistor    |

---

# 4. Important Nodes

The cell has five critical electrical nodes.

| Node | Purpose                   |
| ---- | ------------------------- |
| Q    | Stored data               |
| QB   | Complement of Q           |
| BL   | Bitline                   |
| BLB  | Complementary bitline     |
| WL   | Wordline (access control) |

---

# 5. Understanding Each Transistor

## P1 and P2 (Pull-Up PMOS)

Purpose:

* Charge the internal storage node to VDD.
* Maintain logic '1'.

If the stored value is HIGH, the corresponding PMOS keeps that node near VDD.

---

## N1 and N2 (Pull-Down NMOS)

Purpose:

* Discharge the storage node to ground.
* Maintain logic '0'.

These transistors are intentionally stronger than the pull-up PMOS to improve read stability (explored in later chapters).

---

## A1 and A2 (Access NMOS)

Purpose:

* Connect the storage nodes to BL and BLB.
* Controlled solely by the Wordline (WL).

If WL = LOW:

* Access transistors OFF.
* Cell isolated.

If WL = HIGH:

* Access transistors ON.
* Cell connected to bitlines.

---

# 6. Why Exactly Six Transistors?

A common interview question is: **Why six?**

The answer lies in balancing:

* Stability.
* Speed.
* Area.
* Power.

A 6T cell provides:

* Static data retention.
* Differential read.
* Differential write.
* Good density.

Alternative cells exist:

* 4T: Smaller but less stable.
* 8T: Better read stability, larger area.
* 10T: Improved robustness, higher cost.

The 6T architecture remains the mainstream choice because of this balance.

---

# 7. Internal Data Storage

Assume the cell stores:

```text
Q  = 1
QB = 0
```

Internally:

* Left inverter output is HIGH.
* Right inverter output is LOW.

Each inverter reinforces the other through positive feedback.

This regenerative action keeps the data stable indefinitely while power is applied.

---

# 8. Current Flow in Hold Mode

When:

```text
WL = LOW
```

Then:

* A1 = OFF
* A2 = OFF

There is **no current path to BL or BLB**.

The only currents present are tiny leakage currents.

The cross-coupled inverters continuously reinforce the stored state.

---

# 9. Why is SRAM Called "Static"?

Unlike DRAM:

* No capacitor stores charge.
* No periodic refresh is required.

As long as:

* VDD is present.
* Leakage does not dominate.

The positive feedback of the cross-coupled inverters maintains the stored value.

---

# 10. Voltage Levels

Ideal operating points:

| Node | Logic 1 | Logic 0 |
| ---- | ------- | ------- |
| Q    | ≈ VDD   | ≈ 0 V   |
| QB   | ≈ 0 V   | ≈ VDD   |

In reality, process variation, temperature, leakage, and loading introduce slight deviations. Later chapters will quantify these effects.

---

# 11. Recreating the Cell in xschem

### Objective

Rebuild the 6T SRAM bitcell manually rather than importing an existing schematic.

### Steps

1. Create a new schematic.
2. Place:

   * 2 PMOS devices.
   * 4 NMOS devices.
3. Connect P1/N1 as the left inverter.
4. Connect P2/N2 as the right inverter.
5. Cross-couple the inverter outputs.
6. Add A1 and A2 between Q/QB and BL/BLB.
7. Connect the gates of A1 and A2 to WL.
8. Add VDD and GND supplies.
9. Label all nodes clearly (Q, QB, BL, BLB, WL).

**Verification Checklist**

* Two inverters are correctly cross-coupled.
* Access transistors connect only when WL is asserted.
* No floating nodes remain.

---

# 12. AI-Assisted Prompt Sequence

### Prompt 1 – Architecture

> Generate a labeled transistor-level 6T SRAM bitcell schematic using SKY130 MOSFET naming conventions.

### Prompt 2 – Explanation

> Explain the function of each transistor in the generated 6T SRAM cell.

### Prompt 3 – xschem

> Provide step-by-step instructions to recreate the 6T SRAM bitcell in xschem.

### Prompt 4 – SPICE

> Generate an ngspice netlist for a 6T SRAM bitcell in hold mode using SKY130 models.

### Prompt 5 – Verification

> Review this 6T SRAM netlist and identify any incorrect transistor connections or missing power rails.

---

# 13. Example ngspice Skeleton

```spice
* 6T SRAM Cell Skeleton

VDD VDD 0 1.8
VWL WL 0 0
VBL BL 0 1.8
VBLB BLB 0 1.8

* PMOS
XM1 ...
XM2 ...

* NMOS
XM3 ...
XM4 ...

* Access NMOS
XM5 ...
XM6 ...

.tran 100p 20n

.end
```

> **Assignment Note:** In the repository, replace the placeholder transistor lines with the correct SKY130 device instances and verify that the model include paths match your local PDK installation.

---

# 14. Simulation Goal (Chapter 3)

Do **not** attempt read or write yet.

Only verify:

* The schematic is electrically correct.
* The netlist runs.
* No connectivity errors occur.
* The cell can maintain an initialized state in hold mode.

This establishes a known-good starting point for later experiments.

---

# 15. Common Debugging Issues

| Problem                  | Likely Cause              | Suggested Check                      |
| ------------------------ | ------------------------- | ------------------------------------ |
| Floating Q/QB            | Missing cross-coupling    | Verify inverter feedback connections |
| Cell cannot retain data  | Incorrect inverter wiring | Check PMOS/NMOS gate connections     |
| Access always ON         | WL tied HIGH              | Verify WL stimulus                   |
| ngspice "unknown subckt" | Wrong SKY130 include path | Confirm model libraries              |
| BL and BLB shorted       | Net labeling error        | Inspect node names carefully         |

---

# 16. Industry Practice

Before evaluating read stability or write margin, SRAM designers first perform a **cell integrity review**:

* Schematic review.
* Connectivity review.
* Device sizing review.
* ERC (Electrical Rule Check).
* Basic DC/transient sanity simulations.

Only after the bitcell is validated do they proceed to characterization.

---

# 17. GitHub Deliverables

```text
Week2/
└── Chapter03_6T_SRAM/
    ├── README.md
    ├── architecture.md
    ├── prompts.md
    ├── spice/
    │   └── sram6t_hold.sp
    ├── xschem/
    │   └── sram6t.sch
    ├── waveforms/
    ├── screenshots/
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Commit:

* xschem schematic.
* ngspice netlist.
* Hold-mode simulation.
* AI prompt history.
* Screenshots.
* Debugging log.
* Personal observations.

---

# 18. Industry Gap

A frequent mistake among engineers new to SRAM is treating the 6T cell as a purely digital latch. In reality, it is an **analog regenerative circuit**. Designers evaluate transistor strengths, node voltages, leakage, and process variation—not just logic levels. Developing this analog perspective is essential before studying read disturb, write margin, or static noise margin.

---

# 19. Chapter Summary

In this chapter, we established the architecture of the 6T SRAM bitcell, identified the role of every transistor and node, recreated the schematic, and prepared a verified foundation for simulation. We intentionally limited ourselves to **hold-mode verification**, because a correct and stable bitcell is a prerequisite for all subsequent analyses.

---

# Preview of Chapter 4 – Hold Operation

The next chapter focuses exclusively on **Hold Mode**, examining transistor operating regions, leakage paths, regenerative feedback, data retention mechanisms, retention failures, retention voltage, and hold stability. We will perform the first detailed ngspice simulations and begin correlating theoretical operation with waveform observations. This chapter is the first step toward mastering SRAM stability before introducing read and write operations.


