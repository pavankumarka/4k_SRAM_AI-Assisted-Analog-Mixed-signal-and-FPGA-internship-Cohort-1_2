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


Excellent. From this point onward, we move into the **actual circuit operation**. This chapter is one of the most overlooked in SRAM learning, yet it is fundamental: **if the cell cannot reliably hold data, nothing else matters**. Before we attempt reads, writes, or noise margin analysis, we must prove that the cross-coupled inverters can retain a stored bit under nominal conditions.

---

# Week 2 – Chapter 4

# Hold Operation & Data Retention in a 6T SRAM Cell

> **Objective:** Understand, recreate, simulate, and verify the hold state of a 6T SRAM cell using AI-assisted prompts, xschem, ngspice, and the SKY130 PDK.

---

# Learning Objectives

After this chapter, you should be able to:

* Explain the hold state of a 6T SRAM cell.
* Identify the conduction state of each transistor.
* Trace current flow during hold.
* Explain why data is retained without refresh.
* Simulate hold mode using ngspice.
* Verify AI-generated netlists.
* Identify common hold failures.
* Document the experiment in GitHub.

---

# 1. Why Study Hold Mode First?

The SRAM cell has three primary operating modes:

```text
           6T SRAM Cell
                │
     ┌──────────┼──────────┐
     │          │          │
     ▼          ▼          ▼
   Hold       Read       Write
```

**Hold Mode** is the simplest because:

* Wordline is inactive.
* Bitlines do not influence the cell.
* The cross-coupled inverters operate in isolation.

If the cell cannot retain data in hold mode, it will certainly fail during read or write.

---

# 2. Definition of Hold Mode

A cell is in hold mode when:

* **WL = LOW (0 V)**.
* Access transistors (A1 and A2) are OFF.
* BL and BLB are disconnected from the storage nodes.
* The cross-coupled inverters alone maintain the stored state.

This is the idle state of almost every SRAM cell in an array.

---

# 3. Initial Conditions

Assume the cell stores:

```text
Q  = 1
QB = 0
```

This means:

| Node | Voltage               |
| ---- | --------------------- |
| Q    | ≈ VDD                 |
| QB   | ≈ 0 V                 |
| WL   | 0 V                   |
| BL   | Don't Care (isolated) |
| BLB  | Don't Care (isolated) |

---

# 4. Transistor States

Using the above assumption:

| Transistor | State                | Reason                                                               |
| ---------- | -------------------- | -------------------------------------------------------------------- |
| P1         | OFF                  | Gate at QB = 0 V? Actually gate depends on QB; verify with schematic |
| N1         | ON/OFF based on gate | Depends on QB                                                        |
| P2         | ON/OFF based on gate | Depends on Q                                                         |
| N2         | ON/OFF based on gate | Depends on Q                                                         |
| A1         | OFF                  | WL = LOW                                                             |
| A2         | OFF                  | WL = LOW                                                             |

### Engineering Exercise

Rather than memorizing this table, derive each state from the actual schematic:

1. Identify the gate connection of each transistor.
2. Determine the gate voltage from Q or QB.
3. Decide whether the device is ON or OFF.
4. Compare your result with the simulation operating point.

This habit is essential in industry reviews.

---

# 5. Positive Feedback

The heart of SRAM is the regenerative feedback loop.

```text
Q  ───► Inverter 2 ───► QB
▲                       │
│                       ▼
└──── Inverter 1 ◄──────┘
```

If Q is HIGH:

* It forces QB LOW.
* QB LOW reinforces Q HIGH.

This positive feedback continuously restores the stored state against small disturbances.

---

# 6. Why No Refresh Is Required

Unlike DRAM, SRAM stores information using a **stable feedback loop**, not a capacitor.

As long as:

* VDD is present.
* Device leakage is within acceptable limits.

The cross-coupled inverters regenerate the stored value indefinitely.

This is why SRAM is called **Static Random Access Memory**.

---

# 7. Current Flow During Hold

![Image](https://images.openai.com/static-rsc-4/oERj2W2CpeKgXYCzMkRsIu5F6KO46VW8nSVXd-IoAVxn_vNOog0Ou3CahMeANSzql8ttP9kRPgNkbuJNp46A20drblqg-FYaa3VFbWVEIfHDCdm8K8hpTkx1n8jliqYeIDGzdURRUiVFd8ycwWfrWoiUCyoDjQY_1s9Z5iF1szXbG6M-SB9iaQ0iC8lspzCL?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/TwauNfJdR3pFLMXIP49f-JYKsr5tIIDvkr2VoHWpC4HKHpm73qYGRGNc29Fy5B6hRfZWwcenE_0eJ8sXjSGAdbD90OvV-zPq_xdP68NxsLz4Tr9Q63uGai2vwQjR37cwG2vBMcCMRnshM8Vnhqvh4DvqqSXp3v-qXtsdoAIcHxRbakvdeAsK7nniVC0gqtOb?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/M8-XsDmcHEiARGkBWQlFr0NEXhi79RxLzRFABrSV4NGG3AHuvGOcADIuHHudDAaZNMRse0GJZKl53eyhl43QgYoSEijRqeNnIe1xsYIWhctzDyl8vFOATMBKvILcwqICOrN0hl28LDJOgYMzHDzcVZ2ggP4Kidq6jiHNrThSckhayAsq3QP5j5Sr4XPE-MI3?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Hc4pxoedaZMFEiwXMqrlP8g2Ytlnr3OBxrcKVebz2wor4jU3UFipl2iXnDrYG-8U_IvKBy2AjQnG73u8cMC0r5ynltoI0Hi0mmeD7Hu4Ds8FKDqt9rME1JredQePW_STEO4Tnu5elJ2aE4IYbcM7inNT6ADpvRm5yW4G8HHNp2_SmiIpOMF5TrczVRYvMsDv?purpose=fullsize)

In an ideal cell:

* No current flows through the access transistors.
* No current flows to the bitlines.
* Only tiny leakage currents exist.

Important leakage mechanisms include:

* Subthreshold leakage.
* Gate oxide leakage.
* Junction leakage.

In modern technologies, leakage can dominate standby power.

---

# 8. Voltage Stability

During hold:

| Node | Expected Voltage |
| ---- | ---------------- |
| Q    | Close to VDD     |
| QB   | Close to GND     |

In practice:

* Process variation.
* Temperature.
* Supply voltage.
* Device mismatch.

can slightly shift these voltages.

This motivates later chapters on:

* Static Noise Margin (SNM).
* Monte Carlo analysis.
* PVT variation.

---

# 9. Hold Stability

A stable hold state means:

* The stored value does not change spontaneously.
* Small electrical noise is rejected.
* Leakage does not flip the cell.

Hold stability depends on:

* Cross-coupled inverter gain.
* Transistor sizing.
* Supply voltage.
* Temperature.

---

# 10. AI-Assisted Prompt Sequence

### Prompt 1 – Theory

> Explain why a 6T SRAM cell retains data when the wordline is LOW.

### Prompt 2 – Current Flow

> Describe current flow through each transistor during hold mode.

### Prompt 3 – SPICE

> Generate an ngspice testbench for a 6T SRAM cell in hold mode using SKY130 models.

### Prompt 4 – Verification

> Review this hold-mode netlist and identify possible connectivity or initialization issues.

### Prompt 5 – Debugging

> Why does my SRAM cell lose data in hold mode?

---

# 11. xschem Exercise

Open the 6T SRAM schematic created in Chapter 3.

Tasks:

1. Keep WL tied LOW.
2. Disconnect any active write stimulus.
3. Set an initial state (Q = 1, QB = 0) using appropriate initial conditions or a pre-write sequence.
4. Run a transient simulation.
5. Observe Q and QB over time.

Expected result:

* Q remains HIGH.
* QB remains LOW.

---

# 12. ngspice Simulation

### Example Control Block

```spice
.control
tran 100p 20n
plot v(Q) v(QB)
.endc
```

### Suggested Checks

* Do Q and QB remain complementary?
* Are there unexpected oscillations?
* Does either node drift significantly?

If so, investigate initialization and connectivity before proceeding.

---

# 13. Common Debugging Issues

| Observation                | Possible Cause                     | Suggested Action                                    |
| -------------------------- | ---------------------------------- | --------------------------------------------------- |
| Q and QB both HIGH         | Cross-coupling error               | Recheck inverter wiring                             |
| Q drifts slowly            | Incorrect initial condition        | Initialize or pre-write the cell                    |
| Cell oscillates            | Netlist error or convergence issue | Review feedback connections and simulation settings |
| Access transistor conducts | WL not LOW                         | Verify WL source                                    |
| Simulation fails           | Missing SKY130 model include       | Check include paths                                 |

---

# 14. Industry Verification Checklist

Before moving to read-mode simulations:

* ✔ Cell holds logic '1' for the simulation duration.
* ✔ Cell holds logic '0' when initialized oppositely.
* ✔ WL remains LOW.
* ✔ No unintended bitline interaction.
* ✔ No convergence warnings affecting results.
* ✔ Waveforms match expected theory.

This "hold sanity check" is a standard prerequisite before characterization.

---

# 15. GitHub Deliverables

```text
Week2/
└── Chapter04_Hold/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── hold_mode.sp
    ├── xschem/
    │   └── hold_mode.sch
    ├── waveforms/
    ├── screenshots/
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Document:

* AI prompts used.
* Hold-mode waveform.
* Simulation settings.
* Any errors and fixes.
* Personal observations.

---

# 16. Industry Gap

Many engineers stop after confirming that Q remains HIGH and QB remains LOW.

Professional SRAM designers ask deeper questions:

* How much leakage current flows?
* What is the minimum retention voltage?
* Does the cell remain stable across process, voltage, and temperature corners?
* How sensitive is the cell to device mismatch?

Although those analyses come later, this chapter establishes the baseline needed for them.

---

# 17. Chapter Summary

This chapter focused exclusively on the **hold state** of the 6T SRAM cell. We examined the role of the access transistors, the regenerative action of the cross-coupled inverters, expected node voltages, leakage paths, and verification methodology. By recreating the circuit, simulating hold mode, and validating stable data retention, we established a trusted starting point for the more challenging operations that follow.

---

# Preview of Chapter 5 – Read Operation

The next chapter introduces the **read operation**, where the cell is intentionally connected to the bitlines. We will study precharged bitlines, access transistor conduction, differential bitline discharge, current paths, and the origin of the small voltage difference later amplified by the sense amplifier. We will also begin to see why reading an SRAM cell is **not a passive operation** and how it can potentially disturb the stored data, setting the stage for the following chapter on **Read Disturb**.

---

## Quality Note

From this point onward, **each subsequent chapter will build directly on the previous one**. For example:

* Chapter 5: Read operation (normal behavior).
* Chapter 6: Read disturb (failure mechanism).
* Chapter 7: Write operation.
* Chapter 8: Write margin.
* Chapter 9: Butterfly curve and SNM.

This progression mirrors how SRAM circuit engineers typically characterize a new bitcell in industry and keeps the handbook tightly aligned with your Week 2 and Week 3 objectives.


Excellent. We are now entering the **core of SRAM circuit design**. This chapter is where an SRAM cell first interacts with the outside world. Everything up to now has focused on the isolated bitcell; from this point onward, we begin studying the interactions between the bitcell and the peripheral circuitry.

One improvement I will make from this chapter onwards is to include **"Engineering Thinking"** sections. These explain *why* each design choice exists, which is often missing from textbooks and is expected in industry design reviews.

---

# Week 2 – Chapter 5

# SRAM Read Operation – Theory, Circuit Analysis, Simulation & Verification

> **Objective:** Understand how a 6T SRAM cell performs a read operation, analyze the electrical behavior of each transistor and node, recreate the circuit using AI-assisted prompts, simulate the read operation with xschem/ngspice using SKY130 models, and verify correct functionality.

---

# Learning Objectives

After completing this chapter, you will be able to:

* Explain the complete SRAM read sequence.
* Understand why bitlines are precharged.
* Trace current flow during a read.
* Explain differential sensing.
* Understand why SRAM read is destructive if poorly designed.
* Generate AI-assisted SPICE simulations.
* Verify read operation using ngspice.
* Document waveforms and observations.

---

# 1. Why Reading SRAM is Difficult

A common misconception is:

> "Reading simply means checking whether Q is 0 or 1."

In reality, **reading an SRAM cell is an analog operation**.

During a read:

* The bitcell is connected to large bitline capacitances.
* Charge is redistributed.
* Internal node voltages shift slightly.
* The cell must survive this disturbance while producing enough bitline voltage difference for the sense amplifier.

This balance between **readability** and **stability** is one of the central challenges of SRAM design.

---

# 2. Read Operation Sequence

The read process consists of several ordered steps.

```text
1. Cell stores data
        │
        ▼
2. Precharge BL and BLB to VDD
        │
        ▼
3. Equalize BL and BLB
        │
        ▼
4. Assert WL
        │
        ▼
5. Access transistors turn ON
        │
        ▼
6. One bitline begins to discharge
        │
        ▼
7. Small differential voltage develops
        │
        ▼
8. Sense amplifier detects the difference
        │
        ▼
9. WL goes LOW
        │
        ▼
10. Bitlines are precharged again
```

---

# 3. Initial Conditions

Assume the cell stores:

```text
Q  = 1
QB = 0
```

Before asserting the wordline:

| Signal | Value |
| ------ | ----- |
| WL     | LOW   |
| BL     | VDD   |
| BLB    | VDD   |
| Q      | VDD   |
| QB     | GND   |

The bitlines are intentionally charged to the same voltage.

---

# 4. Why Are Both Bitlines Precharged?

![Image](https://images.openai.com/static-rsc-4/QPKTL8bE0EopkYo_z1IZZbW_5WB7nQX3Q0qD_RZcbyRXpJowCSYnjpTQEz-hzFFwgoddggORbZ-FynkdgHgBqz-zyrFDRx4eWfFysW6Lqu_79OJaOFwXuAhke2EUTI03ZUWseLq0TwCB4knD9U_gp9crrrPKHMnpqo5IHOc4l-DIyxqpa5MuYCywr6StqVeG?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gzMO1wOjJl2MmdeSeZO365mRkkLg5irRa4Q_JtnuMbMO3xIVP-MosxpAmXM10f9F44gwqiF3kN91H6zlDKONv0Io21xWtxq18zt6lA4YnN5ndJqTCvEuaVdibdcAce6ESU3ycmtMRazNz7vkdZKPJrcE_MRqIPArWYXLX3JW5AwV8_QKFMhzZ10KZp77DFtH?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/RrZhjMWZxTZVvE5t0mcyPId-rFIJKd4kVHsTrqQ5ERDY_GoWqKewIviKHykljibq9f3zaZbPllBE_nHy4otdd1NXja4Lrke-bq21rBbq3jihzfYdz866mVNGY6Y4WVMAwmElUvZeJ1aQmI3brU3tYjJzbio4bAZDAVmOoslpyzK45hpY9te-1ZFINoCNsLTM?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/MtTWP4PSHTxKi4Q0cpYTG3t_tqsvZUEN0Uj0JtCesgxUMi6ZcWwey3hHxN65wzQfHrClXnP4hHrUURury7TLIW2Z7SfsYQPWxd5P5mknLoJQsPbqBPhzt2qy-MiNxFr5xucK5ge1lgRw3Y8WB__RGK_jNDxbcimedLyChLqGcADgjvtDqi0yqb1-CAJZPsqs?purpose=fullsize)

### Engineering Thinking

This is one of the most important concepts in SRAM.

The bitlines are **not** initialized to:

* BL = 1
* BLB = 0

Instead:

```text
BL  = VDD
BLB = VDD
```

Why?

Because the sense amplifier detects a **difference**, not an absolute voltage.

Starting both lines at the same voltage:

* Removes initial bias.
* Improves speed.
* Reduces power.
* Enables differential sensing.

---

# 5. Wordline Activation

The row decoder selects one row.

It drives:

```text
WL = HIGH
```

This turns ON:

* Access transistor A1
* Access transistor A2

The storage nodes are now connected to the bitlines.

---

# 6. Current Flow During Read

Assume:

```text
Q  = 1
QB = 0
```

### Left Side

Q is HIGH.

BL is already HIGH.

Almost no current flows.

---

### Right Side

BLB is HIGH.

QB is LOW.

Current flows:

```text
BLB

↓

Access transistor

↓

Pull-down NMOS

↓

Ground
```

BLB begins to discharge.

---

# 7. Bitline Behavior

Initially:

```text
BL  = 1.8 V

BLB = 1.8 V
```

After WL rises:

```text
BL  ≈ 1.80 V

BLB ≈ 1.75 V
```

Later:

```text
BL  ≈ 1.80 V

BLB ≈ 1.65 V
```

Only a **small voltage difference** (often tens to hundreds of millivolts) is needed before the sense amplifier makes a decision.

---

# 8. Differential Read

The sense amplifier receives:

| BL   | BLB            |
| ---- | -------------- |
| High | Slightly lower |

It does **not** wait for BLB to reach 0 V.

This is critical for speed.

Typical differential voltage:

* 50–200 mV (technology and design dependent)

---

# 9. Why Not Fully Discharge the Bitline?

### Engineering Thinking

A bitline may have capacitance hundreds of times larger than the storage node.

Fully discharging it would:

* Increase delay.
* Waste energy.
* Slow the memory.

Instead:

* Generate a small differential.
* Let the sense amplifier amplify it.

This is far more efficient.

---

# 10. Read Current Path

```text
VDD

↓

Bitline

↓

Access NMOS

↓

Pull-down NMOS

↓

Ground
```

Notice:

The PMOS pull-up device is not the primary discharge path.

The pull-down NMOS strength is therefore crucial for read performance.

---

# 11. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the complete read operation of a 6T SRAM cell.

### Prompt 2 – Circuit

> Generate a read-mode testbench for a 6T SRAM cell using SKY130 MOSFET models.

### Prompt 3 – SPICE

> Create an ngspice transient simulation for an SRAM read operation.

### Prompt 4 – Waveform

> Describe the expected BL, BLB, WL, Q, and QB waveforms during a successful read.

### Prompt 5 – Verification

> Review this read-mode netlist and identify potential functional errors.

---

# 12. xschem Exercise

Starting from the Chapter 3 schematic:

1. Add precharged BL and BLB sources.
2. Drive WL with a pulse source.
3. Initialize the cell to:

   * Q = 1
   * QB = 0
4. Run a transient simulation.
5. Plot:

   * V(Q)
   * V(QB)
   * V(BL)
   * V(BLB)
   * V(WL)

Observe:

* BL remains close to VDD.
* BLB discharges slightly.
* Q and QB remain stable.

---

# 13. Example ngspice Control Block

```spice
.control
tran 50p 20n
plot v(BL) v(BLB)
plot v(Q) v(QB)
plot v(WL)
.endc
```

After simulation, verify that:

* Only one bitline discharges.
* The differential develops after WL is asserted.
* The storage nodes are not flipped.

---

# 14. Common Debugging Issues

| Symptom                  | Possible Cause                         | Check                                            |
| ------------------------ | -------------------------------------- | ------------------------------------------------ |
| Both bitlines discharge  | Cell wiring error                      | Verify stored data and cross-coupling            |
| Neither bitline changes  | WL not asserted                        | Check pulse source                               |
| Q flips during read      | Weak pull-down or strong access device | Review transistor sizing (explored in Chapter 6) |
| BL and BLB start unequal | Missing precharge                      | Verify initialization                            |
| No differential develops | Incorrect bitline connection           | Inspect BL/BLB labeling                          |

---

# 15. Industry Verification Flow

A memory design engineer typically performs:

1. Verify precharge.
2. Verify WL timing.
3. Measure bitline differential.
4. Measure read delay.
5. Confirm data retention.
6. Sweep supply voltage.
7. Sweep temperature.
8. Sweep process corners (later characterization).

At the Week 2 stage, focus on steps 1–5.

---

# 16. GitHub Deliverables

```text
Week2/
└── Chapter05_Read/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── read_operation.sp
    ├── xschem/
    │   └── read_operation.sch
    ├── waveforms/
    ├── screenshots/
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompts.
* Read-mode netlist.
* Waveforms for WL, BL, BLB, Q, QB.
* Notes on any simulation issues.
* Personal interpretation of the results.

---

# 17. Industry Gap

Many newcomers believe the read operation is complete when they see BLB discharge.

Experienced SRAM engineers ask additional questions:

* How fast does the differential develop?
* Is the voltage difference sufficient for the target sense amplifier?
* Does the read disturb the internal node?
* What happens at lower supply voltages?
* How does transistor sizing affect the read current?

These questions naturally lead to the next topic.

---

# 18. Chapter Summary

This chapter analyzed the complete read operation of a 6T SRAM cell. We examined precharged bitlines, wordline activation, access transistor conduction, current paths, differential bitline discharge, and the role of the sense amplifier. We also recreated the read-mode testbench, prepared ngspice simulations, and established verification steps for ensuring correct read functionality.

---

# Preview of Chapter 6 – Read Disturb (One of the Most Important Chapters)

Reading an SRAM cell is **not** a passive operation. When the access transistors connect the storage nodes to the precharged bitlines, the internal voltages are disturbed. If the access transistors are too strong, or the pull-down devices are too weak, the cell can accidentally flip during a read.

In Chapter 6, we will perform a **transistor-by-transistor analysis** of this phenomenon, covering:

* Charge sharing.
* Voltage division.
* Read disturb mechanism.
* Cell Ratio (CR).
* Why read failures occur.
* SPICE simulations demonstrating stable and unstable cases.
* Debugging methodology and industry characterization practices.

> **Engineering Note:** Chapter 6 is considered one of the most important chapters in SRAM design. A deep understanding of read disturb is expected in memory circuit design interviews and forms the basis for later discussions on Static Noise Margin (SNM) and transistor sizing.


Excellent. We have now reached what many SRAM designers consider **the most important circuit topic in SRAM bitcell design**.

If someone understands **Read Disturb**, they are no longer just learning SRAM—they are beginning to think like a memory circuit designer.

This chapter intentionally goes deeper than most tutorials because it is the foundation for:

* Static Noise Margin (SNM)
* Cell Ratio (CR)
* Read Stability
* Monte Carlo Analysis
* Low-voltage SRAM Design

---

# Week 2 – Chapter 6

# Read Disturb in a 6T SRAM Cell

> **Objective:** Understand why a read operation can unintentionally change the stored data, analyze the transistor-level mechanism behind read disturb, verify the effect using AI-assisted SPICE simulations, and learn the design techniques used to prevent read failures.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain why an SRAM read is **not** a passive operation.
* Identify the transistor-level cause of read disturb.
* Understand charge sharing and voltage division.
* Explain why the internal storage node rises during a read.
* Understand the concept of **Read Stability**.
* Learn the importance of the **Cell Ratio (CR)** (detailed calculations in Chapter 10).
* Simulate stable and unstable read operations using ngspice.
* Debug common read failures.
* Document observations in GitHub.

---

# 1. What is Read Disturb?

A read disturb occurs when the act of reading a stored value **changes the internal node voltage enough to risk flipping the cell**.

The bitcell should behave like this:

```text id="rd001"
Stored Data

↓

Read

↓

Same Stored Data
```

A read failure behaves like:

```text id="rd002"
Stored Data

↓

Read

↓

Data Changes

↓

Read Failure
```

Even though no write operation was requested, the cell is unintentionally modified.

---

# 2. Why Does Read Disturb Occur?

Let's assume the stored state is:

```text id="rd003"
Q  = 1

QB = 0
```

Before the read:

| Signal | Value |
| ------ | ----- |
| WL     | LOW   |
| BL     | VDD   |
| BLB    | VDD   |
| Q      | VDD   |
| QB     | GND   |

Everything is stable.

---

# 3. What Changes During Read?

When:

```text id="rd004"
WL = HIGH
```

The access transistors connect:

```text id="rd005"
Q

↓

BL

QB

↓

BLB
```

The problem is:

BLB starts at **VDD**, while QB is at **0 V**.

These two nodes are suddenly connected.

---

# 4. Charge Sharing

![Image](https://images.openai.com/static-rsc-4/BhoVOsaXLFE5ZECo9la4CySUOADdpuy39hp3UdTCEaL_NCZyMTZO8RthwPM37TQypOCvGha6vUeDISm_7yhVmgj5h5ZhQQfLrtPC5PjCYf194Ch43PuvH9fVD4cuyNbaFEH7tXwks-Zn1etnEDTzvg2fRPzgpuKhVc90w3BDo4lqb5zBczHvQPg641Hg46ZA?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/qC4Ul8kRVdGJa7DnyLLuexh6BQSHzgILwniLZk9y0_owz3IB17OVdWQ_3fwjGmwaCsWbyseStFV1CqIb9jwTsIFOclvpDlHqSWYDU7zNDpzzl1C4A62DL9x5sL6SvUuis9iWydY8IiBHGYSHeGBJbVoeTWUPwYE7MI7TpPMFkGA4zQdjnnUEpdaZEdSpWsQx?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/hv3BhWcJviUhXfPgximqopyjkozTbJwmdu2Vj_jXsSpWYivSQI0sagpQ0CYyoAtjY62k9Iyf1A0Vr2uF3_eGLhGcc8dWheZ9A65pXQvQJ5ekGTZPe06f1fm6FKUN4exdzo50_4Vu3OeeWF--nnewsLntudhQpTKOpUlnS4g3gpWqha2BQ2NBriD6zW12kMtW?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/H1ANmJ5AmnXALqbuprryYUeGQJalEZiJymZGm9xVEPBBPbfC54p0VouzSzkEoTqPN7TqU2sp749i4jlFWPraILeOqhpSMzeyCWZ_6GnFcaUwbRwQe0V-r5_uZpP-vupETKVHx3zBIqG8nYjk1P3cs59E5O9QeVXlm8aVlf_uEJxDmD9Dlhyz9U7qaswvPnJt?purpose=fullsize)

Charge sharing occurs because:

* BLB has a **large capacitance** charged to VDD.
* QB has a **small internal storage capacitance** at 0 V.

When connected:

* Charge flows from BLB toward QB.
* QB rises above 0 V.

This voltage rise is called **read disturb**.

---

# 5. Why Doesn't the Cell Flip Every Time?

The pull-down NMOS (N2) is simultaneously trying to keep QB at ground.

Two devices now compete:

```text id="rd006"
Bitline

↓

Access NMOS

↓

QB

↓

Pull-down NMOS

↓

Ground
```

The final QB voltage depends on:

* Access transistor strength.
* Pull-down transistor strength.
* Bitline capacitance.
* Supply voltage.
* Process variation.

---

# 6. Engineering Thinking

This is the central design trade-off:

A **strong access transistor**:

* Improves write ability.
* Increases read disturb.

A **strong pull-down transistor**:

* Improves read stability.
* Makes writing more difficult.

There is no perfect sizing. SRAM design is an optimization problem.

---

# 7. Voltage Evolution During Read

Ideally:

```text id="rd007"
QB = 0 V
```

During read:

```text id="rd008"
QB ≈ 50–300 mV
```

If QB rises too high:

The opposite inverter may interpret it as a logic transition, initiating positive feedback and flipping the stored state.

---

# 8. Read Stability

A read is considered stable if:

* Q remains HIGH.
* QB remains LOW (with only a small temporary rise).
* After WL goes LOW, QB returns to its original value.

An unstable read occurs if:

* QB crosses the inverter switching threshold.
* Positive feedback takes over.
* The stored data changes.

---

# 9. Read Failure Mechanism

```text id="rd009"
Precharge

↓

WL HIGH

↓

Charge Sharing

↓

QB Rises

↓

Crosses Switching Threshold?

↓

Yes → Cell Flips

↓

No → Cell Survives
```

This is why **switching threshold** and **noise margin** matter so much.

---

# 10. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the read disturb mechanism in a 6T SRAM cell with charge sharing.

### Prompt 2 – Circuit

> Generate a transistor-level testbench that demonstrates read disturb in a 6T SRAM cell using SKY130 models.

### Prompt 3 – Simulation

> Create an ngspice transient simulation showing the internal node voltage rise during a read operation.

### Prompt 4 – Verification

> Review this read-disturb netlist and identify transistor sizing or connectivity issues that could cause instability.

### Prompt 5 – Debugging

> Why does my SRAM cell flip during a read operation?

---

# 11. xschem Exercise

Starting from the read-mode schematic:

1. Precharge BL and BLB.
2. Initialize:

   * Q = HIGH
   * QB = LOW
3. Assert WL.
4. Plot:

   * Q
   * QB
   * BL
   * BLB
5. Zoom into QB.

Observe:

* A small voltage rise is expected.
* The node should recover after WL returns LOW.

---

# 12. ngspice Simulation

### Suggested Control Block

```spice
.control
tran 20p 10n
plot v(Q)
plot v(QB)
plot v(BL)
plot v(BLB)
.endc
```

### Additional Measurement

Use `.measure` to capture the peak disturbed voltage:

```spice
.measure tran QB_PEAK MAX v(QB)
```

Record this value in your observations.

---

# 13. Common Debugging Issues

| Symptom                          | Possible Cause                                   | Suggested Investigation                           |
| -------------------------------- | ------------------------------------------------ | ------------------------------------------------- |
| QB immediately flips HIGH        | Access transistor too strong or incorrect wiring | Check transistor sizes and connections            |
| No disturbance visible           | WL pulse too short or weak                       | Verify stimulus timing                            |
| Both storage nodes move together | Cross-coupling error                             | Inspect inverter feedback                         |
| BLB discharges excessively       | Incorrect load or missing precharge              | Verify bitline initialization                     |
| Simulation convergence warnings  | Initial conditions not well defined              | Add initial conditions or review simulation setup |

---

# 14. Industry Verification Method

Before characterizing read stability, engineers typically verify:

* Correct precharge.
* Proper WL pulse width.
* Internal node disturbance.
* Peak disturbed voltage.
* Recovery after WL is deasserted.
* No unintended state change.

Later characterization adds:

* Voltage sweeps.
* Temperature sweeps.
* Process corner analysis.
* Monte Carlo mismatch.

---

# 15. GitHub Deliverables

```text
Week2/
└── Chapter06_Read_Disturb/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── read_disturb.sp
    ├── xschem/
    │   └── read_disturb.sch
    ├── waveforms/
    ├── screenshots/
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Document:

* AI prompts.
* Peak QB disturbance.
* Waveforms.
* Debugging notes.
* Personal explanation of why the cell remained stable (or failed).

---

# 16. Industry Gap

Many engineers believe that a successful read simply means obtaining the correct output.

Memory designers ask deeper questions:

* **How much did the internal node move?**
* **What is the maximum safe disturbance?**
* **How close is the cell to failure?**
* **Will this still work at lower VDD or under process variation?**

These questions drive transistor sizing and robustness analysis.

---

# 17. Connection to Future Chapters

This chapter provides the foundation for several upcoming topics:

* **Chapter 7 – Write Operation:** Another transistor-strength trade-off, but in the opposite direction.
* **Chapter 8 – Write Margin:** How easily can new data overwrite the existing state?
* **Chapter 9 – Static Noise Margin (SNM):** Quantifying stability using butterfly curves.
* **Chapter 10 – Cell Ratio (CR):** Mathematical relationship between pull-down and access transistor strengths that governs read stability.

Understanding read disturb makes these topics much more intuitive.

---

# 18. Chapter Summary

This chapter explained why reading an SRAM cell is an active analog process rather than a passive observation. We studied charge sharing between the precharged bitline and the internal storage node, analyzed the competition between the access and pull-down transistors, and introduced the concept of read stability. Through AI-assisted prompts, xschem reconstruction, and ngspice simulations, we established a practical methodology for observing and debugging read disturb.

---

# Preview of Chapter 7 – Write Operation

Reading attempts to **observe** the stored data while preserving it. Writing does the opposite—it deliberately **overpowers** the existing state and forces the cell into a new one.

In Chapter 7, we will analyze:

* Differential write operation.
* Writing logic '0' and logic '1'.
* Current paths during write.
* Why writing is often harder than expected.
* The competition between access transistors and pull-up PMOS devices.
* AI-assisted generation of write-mode testbenches.
* ngspice verification of successful and failed writes.

> **Engineering Note:** The write operation is the mirror image of read stability. During a read, the pull-down NMOS must dominate the access transistor. During a write, the access transistor must overcome the pull-up PMOS. Understanding this opposing balance is the key to SRAM transistor sizing and is a recurring theme throughout the remaining chapters.


Excellent. We have now completed the **read path** (Hold → Read → Read Disturb). The next logical step is the **write path**.

This chapter is where many first-time SRAM learners realize something surprising:

> **Writing into an SRAM cell is not guaranteed.**

Unlike a flip-flop driven by strong logic gates, an SRAM bitcell actively **resists being overwritten** because its cross-coupled inverters are designed to preserve the current state. A successful write therefore becomes a carefully balanced analog competition between the write driver and the storage cell.

---

# Week 2 – Chapter 7

# SRAM Write Operation – Theory, Circuit Analysis, Simulation & Verification

> **Objective:** Understand how new data is written into a 6T SRAM cell, analyze the transistor-level switching mechanism, recreate the write operation using AI-assisted prompts, simulate successful and failed writes using xschem/ngspice with SKY130 models, and verify the results.

---

# Learning Objectives

After this chapter, you will be able to:

* Explain the complete write sequence.
* Understand how the write driver forces new data into the cell.
* Trace current flow during write.
* Explain why writing is an analog competition.
* Differentiate between **Write '0'** and **Write '1'**.
* Recognize write failures.
* Simulate write operations using ngspice.
* Document results in GitHub.

---

# 1. Purpose of the Write Operation

The goal of the write operation is simple:

```text id="wr001"
Old Data

↓

Overwrite

↓

New Data
```

However, electrically it is one of the most challenging operations because the SRAM cell is **designed to preserve its existing state**.

---

# 2. Initial Conditions

Assume the cell currently stores:

```text id="wr002"
Q = 1

QB = 0
```

We now want to write:

```text id="wr003"
Q = 0

QB = 1
```

The write driver prepares the bitlines accordingly.

---

# 3. Bitline Preparation

Before WL is asserted:

| Signal | Value |
| ------ | ----- |
| BL     | 0 V   |
| BLB    | VDD   |
| WL     | LOW   |

Unlike a read operation, the bitlines are **not both precharged**.

Instead, the write driver actively forces complementary values:

```text id="wr004"
BL  = 0

BLB = 1
```

---

# 4. Write Operation Sequence

```text id="wr005"
Old Data Stored
        │
        ▼
Write Driver Forces BL / BLB
        │
        ▼
Wordline Goes HIGH
        │
        ▼
Access Transistors Turn ON
        │
        ▼
Internal Node Begins Changing
        │
        ▼
Cross-Coupled Inverters Switch
        │
        ▼
New Data Stored
        │
        ▼
Wordline LOW
```

---

# 5. Current Flow During Write '0'

![Image](https://images.openai.com/static-rsc-4/gidtSj7_qHJJy7rlitkp9gVIsxpQnckqfVJtjPvAP9MXo204ukQuJJKK83F6f-WufHMrvI0SqKFs_PX_Le2HsG0oRaV7C_myCO0L2L8ROkWgAXp0Hj_qA-Uhw6kfhfjr2Zfwk4W5qOYc-W_Y76jUfsi2qf760ObAqCWlsJLB1AIUimOaG9tu_BV118s3gxPb?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/4T_bo9e-PPOv0gykIn6fLKhprFsjpjRBC-YNGWVx_k4nIzmghrDg9ngao6LVxDJClq1zotmPZ91EQ1Os3dechKOACI0VnjNKa302HUQS-4RKSIg7w4bQ-knHqG2ymRi55w7o_wjRNZzl1deN03c6GF-VrkBrx6xSjFeQzXJDBiveozjRDyD2QFSGZU7wyFzV?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/-YTcph_r0F40z_nequLKZ-Vdquq3w1sLJqygopCcE0RilsdJLu04qVJPB9cpeH4Pw1cIaTlEfaYhzAVvVpldRDSpV1QaQXuf243RPslfN5mSOlM6RuhYBrCJ2hSj_rh4HOjND72W0gH9jqdqJaes3gL7iBxLCX79aDSCorvCS5SLIUJeuPnvnQKAOfn5kqFy?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/b1pCzuKuviNVFe__r9colC9E7iAp2I6yz0wacubQskK1DekfZAFPvLLOOyImMtBat9uBzVIx9blkyi_TLrqShEMBM_aFsBpEM8t0NLvOMWWqp1XwOxzQtCMa2qixrgIq5_47uaAxDXJ7KzjWo-tyWYiTv58w3v9p8HszoyIxKefWfT3Ha5zv5CKTuV59Y5cX?purpose=fullsize)

Assume:

```text id="wr006"
BL = 0

BLB = VDD
```

When WL becomes HIGH:

* Access transistors turn ON.
* BL pulls **Q downward**.
* BLB pushes **QB upward**.
* The cross-coupled inverters begin to lose their previous stable state.

---

# 6. The Analog Competition

This is the key engineering concept.

To write successfully:

The **access transistor + write driver** must overcome the **pull-up PMOS** that is trying to keep Q at VDD.

The competition is:

```text id="wr007"
Write Driver

↓

Access NMOS

↓

Storage Node

↑

Pull-up PMOS
```

If the write driver wins:

The cell flips.

If the pull-up PMOS wins:

The write fails.

---

# 7. Positive Feedback During Switching

Initially:

```text id="wr008"
Q = 1

QB = 0
```

As Q decreases:

* The opposite inverter begins responding.
* QB starts increasing.
* Once the inverter switching threshold is crossed, regenerative feedback rapidly completes the transition.

The cell then settles to:

```text id="wr009"
Q = 0

QB = 1
```

This regeneration makes the write transition very fast once it begins.

---

# 8. Writing Logic '1'

Writing a logic '1' is the mirror image.

Prepare:

```text id="wr010"
BL = VDD

BLB = 0
```

After WL is asserted:

* BL raises Q.
* BLB lowers QB.
* The inverters regenerate to the new stable state.

---

# 9. Why Write Can Fail

Common causes include:

* Pull-up PMOS too strong.
* Access NMOS too weak.
* Low supply voltage.
* Insufficient WL pulse width.
* Weak write driver.
* Process variation.

The cell may partially change but then recover to the original value once WL goes LOW.

---

# 10. Engineering Thinking

There is a fundamental trade-off:

A stronger pull-up PMOS:

* Better hold stability.
* Harder to overwrite.

A stronger access transistor:

* Easier writing.
* Greater read disturb risk (Chapter 6).

SRAM design is therefore an optimization between read stability and write ability.

---

# 11. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the write operation of a 6T SRAM cell and describe the role of the write driver.

### Prompt 2 – Circuit

> Generate a transistor-level write-mode testbench for a 6T SRAM cell using SKY130 devices.

### Prompt 3 – SPICE

> Create an ngspice transient simulation for writing a logic '0' into a 6T SRAM cell.

### Prompt 4 – Verification

> Review this write-mode netlist and identify any transistor sizing or connectivity issues that may prevent a successful write.

### Prompt 5 – Debugging

> Why does my SRAM cell fail to change state during a write operation?

---

# 12. xschem Exercise

Starting from the Chapter 3 schematic:

1. Add complementary write-driver voltage sources:

   * BL = LOW
   * BLB = HIGH
2. Apply a WL pulse.
3. Initialize the cell to:

   * Q = HIGH
   * QB = LOW
4. Run a transient simulation.
5. Plot:

   * V(Q)
   * V(QB)
   * V(BL)
   * V(BLB)
   * V(WL)

Expected result:

* Q transitions from HIGH to LOW.
* QB transitions from LOW to HIGH.
* The cell remains in the new state after WL returns LOW.

Repeat the experiment for writing logic '1'.

---

# 13. ngspice Control Example

```spice
.control
tran 20p 10n

plot v(Q)
plot v(QB)
plot v(BL)
plot v(BLB)
plot v(WL)

.endc
```

### Suggested Measurements

```spice
.measure tran WRITE_DELAY \
TRIG v(WL) VAL='0.9' RISE=1 \
TARG v(Q) VAL='0.9' FALL=1
```

Record the measured delay and compare it across transistor sizing experiments.

---

# 14. Common Debugging Issues

| Symptom                                  | Possible Cause                                | Suggested Check                       |
| ---------------------------------------- | --------------------------------------------- | ------------------------------------- |
| Q never changes                          | Weak access transistor or incorrect BL values | Verify BL/BLB stimulus                |
| Q changes briefly then returns           | Pull-up PMOS dominates                        | Review transistor sizing              |
| Both nodes become undefined              | Incorrect initialization or feedback wiring   | Inspect cross-coupled inverters       |
| BL and BLB equal                         | Write driver not producing complementary data | Check write stimulus                  |
| Write only succeeds with a long WL pulse | Drive strength too low                        | Measure write delay and review sizing |

---

# 15. Industry Verification Flow

Before declaring a write successful, memory designers verify:

1. Correct complementary bitline values.
2. WL timing and pulse width.
3. Internal node transition.
4. Regenerative switching.
5. Final stored state after WL returns LOW.
6. Write delay.
7. Operation across different supply voltages.

Later characterization adds process and temperature variation.

---

# 16. GitHub Deliverables

```text
Week2/
└── Chapter07_Write/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   ├── write0.sp
    │   └── write1.sp
    ├── xschem/
    │   └── write_operation.sch
    ├── waveforms/
    ├── screenshots/
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompt history.
* Write '0' waveform.
* Write '1' waveform.
* Measured write delay.
* Notes on any failed write attempts.
* Explanation of the final results.

---

# 17. Industry Gap

A beginner typically checks only whether the cell eventually stores the new value.

An SRAM designer asks:

* What is the minimum WL pulse width required?
* What is the maximum pull-up strength that still permits writing?
* How much write margin exists?
* Does writing still succeed at reduced VDD?
* What happens under worst-case process corners?

These questions lead directly to the next chapter.

---

# 18. Chapter Summary

This chapter explained the complete write operation of a 6T SRAM cell. We examined how the write driver prepares complementary bitlines, how the access transistors connect the cell to the write path, and how the internal storage nodes transition through regenerative feedback. We also analyzed the analog competition between the write driver and the pull-up PMOS devices, recreated the write-mode testbench, prepared ngspice simulations, and established a verification methodology for successful writes.

---

# Preview of Chapter 8 – Write Margin

A successful write in one simulation does **not** mean the cell is easy to write.

The next chapter introduces **Write Margin**, a quantitative measure of how much effort is required to overwrite the existing state.

We will cover:

* Definition of write margin.
* Write Trip Point (WTP).
* Write failure mechanisms.
* Measuring write margin in ngspice.
* Impact of transistor sizing.
* Supply voltage effects.
* AI-assisted generation of write-margin testbenches.
* Industry characterization methodology.

> **Engineering Note:** Chapters 6 (Read Disturb), 8 (Write Margin), and 9 (Static Noise Margin) form the **three core stability metrics** used to evaluate SRAM bitcells. Mastering these chapters provides the analytical foundation expected of an SRAM circuit designer and aligns directly with the objectives of your Week 2 assignment.

