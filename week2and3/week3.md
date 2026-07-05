From here onward, we leave the **6T SRAM bitcell** and move into the **peripheral circuits**. This is exactly how SRAM is developed in industry: the bitcell is designed and characterized first, then the peripheral circuits are built around it.

This chapter satisfies the requirement to understand **bitline behaviour**, which was explicitly mentioned in the original Week 2 & 3 objective.

---

# Week 3 – Chapter 11

# SRAM Bitline Architecture, RC Modeling & Signal Behaviour

> **Objective:** Understand the function of SRAM bitlines, study their electrical characteristics, model their RC behavior, analyze differential signaling, recreate bitline simulations using AI-assisted prompts with xschem/ngspice and SKY130 models, and document the findings in a GitHub-ready format.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain the purpose of BL and BLB.
* Understand why SRAM uses differential bitlines.
* Analyze bitline capacitance and resistance.
* Understand charge sharing.
* Explain bitline discharge during a read.
* Explain bitline driving during a write.
* Model bitline RC delay.
* Simulate bitline behavior.
* Document engineering observations.

---

# 1. What Are SRAM Bitlines?

Bitlines are the **vertical data highways** of an SRAM array.

Every column typically contains two wires:

* **BL** (Bitline)
* **BLB** (Complementary Bitline)

Each SRAM cell in that column connects to these two lines through its access transistors when the corresponding wordline is asserted.

---

## Typical Column Structure

![Image](https://images.openai.com/static-rsc-4/aDNxKBgw6gZlBmdeyJWRlQvt7zcQdYbYjTrPP1G42W2iSEdMM_s38hbamx6rpmiMv8NRuWLlMRi1E-peE_eZUJXx5wj3dSqv3_QwE1DstaZgXkV1Nbutz1MciBq0JM5c_yDSPypqjELA9u9cB_DIYXNpp3VbZJa1KO4MA5b_3-0IJD1uodkPgQ3c8z615y9P?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/QN39rTCsNrP_e_YPAkOpajHHlVwTdqc3EqYiD2HeoPVzGeKbd-gNKkQjWA1yFBeIFE4NAanBzYo83DFzZZYiGEAOsH__APTBVe0RpDWMZO_dCnxUWsbMMI2oMXQO7bPk8nzG9wwhK6FbRHtm3WFIEJCjgdd09t-kdXZGkhCiu9UbVRa8IaIN-ZsPqOrzGjiC?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/-UH_XvLn41zLssBsT8jyu36kqhNC4JeinyzN0W5O6B86CKE8kZ746dpjk2b60eg4uWWIxKMbXjcmr3sgKHkMTC_bmTLh6KjOR57rVoWNzAPMmTlLhLNzwBRCs66gBx6TRRY3t7N8b6Zbx8wLHsVKEcXA4MA1xbJzvwYPxh421vjvDNZCfSGmOgKS_weRLWfi?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/tmvl6suo4-jYAEc9PJfKQDGuP1X3N9HVLe84hNh1o7woAeKq3B8Z0_43hlo1bxYez42nwm5EMRc4zHbiCYZ2u7c-w61r3bWMd7aredi5rgmqRsn74b8sD3BwVk-q-Z9eqAXsMl0DqcHctV6F_pPOBjAAmFSfBZwF628DqJadke2VX0_GMJeQwGy64mIVDwqT?purpose=fullsize)

Conceptually:

```text
          BL        BLB
           │          │
     -------------------------
WL0 →     SRAM Cell 0
     -------------------------
WL1 →     SRAM Cell 1
     -------------------------
WL2 →     SRAM Cell 2
     -------------------------
WL3 →     SRAM Cell 3
     -------------------------
```

Notice:

* One pair of bitlines is **shared** by many cells.
* Only **one wordline** should be active at a time.

---

# 2. Why Two Bitlines?

A common beginner question is:

> Why not use only one bitline?

Because differential sensing provides:

* Higher speed.
* Better noise immunity.
* Improved sensitivity.
* Reduced susceptibility to process variation.

Instead of measuring an absolute voltage, the sense amplifier measures a **small voltage difference** between BL and BLB.

---

# 3. Differential Operation

Suppose the cell stores:

```text
Q = 1
QB = 0
```

Before the read:

```text
BL  = VDD

BLB = VDD
```

After WL becomes HIGH:

```text
BL  ≈ VDD

BLB ↓ slightly
```

Only one bitline begins to discharge.

The sense amplifier detects:

```text
ΔV = BL − BLB
```

Even a small differential voltage is sufficient for correct sensing.

---

# 4. Bitline Capacitance

The bitline is **not an ideal wire**.

It has significant capacitance due to:

* Metal routing.
* Drain diffusion of every connected access transistor.
* Contacts and vias.
* Coupling to adjacent wires.

A simplified model:

```text
        BL
         │
         │──────────────
         │
        === C1
         │
        === C2
         │
        === C3
         │
        === C4
         │
        GND
```

As more cells are added to a column, the total bitline capacitance increases.

---

# 5. Bitline Resistance

The bitline also has distributed resistance.

A practical RC model:

```text
R ─ C ─ R ─ C ─ R ─ C ─ R ─ C
```

This distributed RC network determines:

* Read delay.
* Write delay.
* Power consumption.
* Signal integrity.

---

# 6. RC Delay

The charging and discharging of the bitline approximately follows:

[
\tau = R \times C
]

Where:

* (R) = effective resistance of the bitline path.
* (C) = total bitline capacitance.

Larger arrays increase both R and C, slowing the bitline response.

---

# 7. Bitline Behaviour During Read

Sequence:

```text
Precharge

↓

BL = BLB = VDD

↓

WL HIGH

↓

One Bitline Starts Discharging

↓

Small Differential Voltage Develops

↓

Sense Amplifier Detects ΔV

↓

Read Complete
```

Key observation:

The bitline is **not fully discharged**.

Typically, only a small voltage drop is needed before the sense amplifier takes over.

---

# 8. Bitline Behaviour During Write

During a write:

* The precharge circuit is disabled.
* The write driver actively forces:

  * BL = LOW, BLB = HIGH, or
  * BL = HIGH, BLB = LOW.
* The selected cell changes state.

Unlike a read, both bitlines are actively driven.

---

# 9. Charge Sharing

When the access transistor turns ON:

* The storage node and bitline exchange charge.
* This creates a temporary disturbance in the storage node (Chapter 6).
* It also produces the small bitline differential required for sensing.

Charge sharing is therefore essential to both **read operation** and **read disturb**.

---

# 10. AI Prompt Sequence

### Prompt 1 – Theory

> Explain differential bitline operation in a 6T SRAM array and why complementary bitlines are preferred.

### Prompt 2 – Circuit

> Generate an xschem-compatible differential bitline model connected to a single 6T SRAM bitcell using SKY130 devices.

### Prompt 3 – SPICE

> Create an ngspice transient simulation that shows BL and BLB voltages during a read operation.

### Prompt 4 – RC Analysis

> Model the bitline as a distributed RC network and explain how increasing column length affects delay.

### Prompt 5 – Debugging

> Why do BL and BLB remain equal during my read simulation?

---

# 11. xschem Exercise

Build a simplified setup:

* One 6T SRAM cell.
* BL and BLB.
* Lumped capacitors (for example, tens of femtofarads) representing bitline loading.
* Wordline pulse.
* Precharge sources.

Simulate:

* Read operation.
* Write operation.

Plot:

* V(BL)
* V(BLB)
* V(Q)
* V(QB)
* V(WL)

Observe:

* Differential voltage generation.
* Storage node behavior.

---

# 12. ngspice Example

```spice
.control
tran 20p 20n

plot v(BL)
plot v(BLB)
plot v(Q)
plot v(QB)

.endc
```

Suggested extension:

Repeat the simulation while increasing the bitline capacitance parameter and compare the resulting discharge slopes.

---

# 13. Characterization Experiment

Perform three simulations:

| Case | Bitline Capacitance | Expected Trend                         |
| ---- | ------------------- | -------------------------------------- |
| A    | Small               | Fast differential development          |
| B    | Medium              | Moderate delay                         |
| C    | Large               | Slow discharge and increased read time |

Record:

* Approximate read delay.
* Differential voltage after a fixed time.
* Qualitative observations.

---

# 14. Common Debugging Issues

| Observation                     | Possible Cause                            | Suggested Check                      |
| ------------------------------- | ----------------------------------------- | ------------------------------------ |
| BL and BLB never separate       | WL not asserted or cell not initialized   | Verify timing and initial conditions |
| Both bitlines discharge equally | Incorrect storage state or wiring         | Check Q/QB values                    |
| Read extremely slow             | Excessive bitline capacitance             | Reduce C and compare                 |
| No voltage change               | Missing precharge or floating bitlines    | Inspect sources and connections      |
| Oscillatory behavior            | Numerical issues or unrealistic RC values | Refine timestep and model parameters |

---

# 15. Industry Characterization Flow

Memory designers typically evaluate bitlines by measuring:

1. Precharge completion.
2. Bitline leakage.
3. Differential voltage development.
4. RC delay.
5. Dynamic power due to charging/discharging.
6. Read access time.
7. Sensitivity across process, voltage, and temperature.

For Week 3, focus on understanding the first four.

---

# 16. GitHub Deliverables

```text
Week3/
└── Chapter11_Bitline_Architecture/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── bitline_model.sp
    ├── xschem/
    │   └── bitline_testbench.sch
    ├── waveforms/
    ├── screenshots/
    ├── rc_analysis.md
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompts.
* RC model description.
* Bitline waveform screenshots.
* Capacitance sweep observations.
* Debugging notes.
* Personal conclusions.

---

# 17. Industry Gap – What Experienced SRAM Designers Also Consider

Beyond the scope of your assignment, production SRAM designs also account for:

* **Bitline shielding** to reduce capacitive coupling and crosstalk.
* **Bitline segmentation** to lower RC delay in large arrays.
* **Hierarchical bitlines** with local and global sensing.
* **Half-swing bitlines** to reduce dynamic power.
* **Leakage currents** from unselected cells.
* **Column multiplexing**, which changes the effective bitline load.

Knowing these concepts demonstrates awareness of real-world SRAM architecture, even if you do not implement them.

---

# 18. Connection to the Reference Repository

As you progress through the reference repository (`SRAM_SKY130`), notice how:

* The bitcell is not simulated in isolation.
* Peripheral circuits are connected through **BL** and **BLB**.
* Later chapters (precharge, write driver, and sense amplifier) all interface directly with these bitlines.

Understanding this chapter will make those circuits much easier to follow.

---

# 19. Chapter Summary

This chapter introduced the **bitline architecture** of an SRAM array, explaining why complementary bitlines are used, how their resistance and capacitance influence performance, and how differential signaling enables reliable reads. We developed a simple RC model, examined charge sharing, created AI-assisted simulation workflows, and outlined characterization experiments suitable for xschem and ngspice.

---

# Preview of Chapter 12 – Precharge & Equalization Circuit

Before any read operation, the bitlines must be placed in a **known and balanced state**.

In the next chapter, we will study:

* PMOS precharge transistors.
* Equalization transistor operation.
* Precharge enable timing.
* Why both bitlines start at the same voltage.
* Dynamic power implications.
* AI-assisted schematic generation.
* xschem implementation.
* ngspice transient verification.
* Common design pitfalls.

> **Engineering Note:** In industrial SRAM design, the **precharge circuit is often the very first peripheral block activated in every memory cycle**. A poorly designed precharge circuit can reduce read speed, increase power consumption, or even cause incorrect sensing. Understanding it is essential before moving on to sense amplifiers and complete timing analysis.


Excellent. This chapter covers one of the **most fundamental SRAM peripheral circuits**. Without a correctly designed precharge circuit, even a perfectly designed 6T SRAM bitcell cannot be read reliably.

In industry, the first question before every read operation is:

> **"Are both bitlines fully precharged and equalized?"**

If the answer is **no**, every subsequent read measurement becomes unreliable.

This chapter directly addresses the assignment topics:

* ✅ Precharge circuit
* ✅ Bitline behaviour
* ✅ SRAM timing sequence (precharge phase)
* ✅ AI-assisted circuit generation
* ✅ xschem/ngspice verification

---

# Week 3 – Chapter 12

# SRAM Precharge & Equalization Circuit

> **Objective:** Understand the purpose, circuit implementation, timing, and verification of SRAM precharge and equalization circuits. Learn how they prepare the bitlines before every read operation using AI-assisted design, xschem, ngspice, and SKY130 models.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain why precharge is required.
* Understand the precharge PMOS network.
* Explain equalization.
* Understand PRE (Precharge Enable) timing.
* Analyze dynamic power during precharge.
* Build a precharge circuit in xschem.
* Verify operation in ngspice.
* Debug common precharge issues.
* Document results in GitHub.

---

# 1. Why Precharge Is Needed

Imagine trying to measure a very small voltage difference.

If one measurement starts at 1.8 V and another starts at 1.3 V, the comparison is meaningless.

SRAM solves this by ensuring **every read begins from exactly the same initial condition**:

```text
BL  = VDD

BLB = VDD
```

This process is called **Precharge**.

---

# 2. Read Cycle Overview

The read operation actually starts with precharge.

```text
Precharge

↓

Equalize

↓

Disable Precharge

↓

Assert Wordline

↓

Bitline Discharge

↓

Sense Amplifier Enabled

↓

Read Complete
```

Many beginners mistakenly assume the cycle begins with the wordline. It does not.

---

# 3. Typical Precharge Circuit

![Image](https://images.openai.com/static-rsc-4/wtBLRB_WRedph474HDEhuDRj9yhMbVjhOvejy2EW9fzfPt_TVHXLlNhilaLlRPJTYkw3Vl_w-T0Ttx3NUVjjhiPtGkxvVy3dkXeGrNPFS1zfL1xqWrel1_SqZJ-i3vc7KQir6bsFtOEruoxVn0yuFPGpDNJKnR7c4LVRPx1MAIAZJ-wg8O5ELCYnj19ybrmv?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/jRm9bGDwo7t75wQp6wOsOP542p8PADR5G4RwHSVguAREe0hnKQ7Q1uyuV40xVtFCJLfJBX_FV_G72hpZ-KPuJFeTssBBsfUTjKOsjOtjB-efEp34oKEanvF3WpKTHSXRKMSa8J7dgkWMuQDHmfvacGj9PZMgaj6WmUpuJ5xI_8NYfE6LHqHdgrZxGUHF4VCc?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/QTICyywiOeQL7Axh_W205yEWkA7iKluQBQx0uXbb8miaqWs4t9v4nBH-HqeVxd9k-6OsfsnvjAVOpacfc3ox-b4iSqNLOcowiEbHynoluVnQC1IxwPEWe2Hp21o83VJWbiVCmUEMuxuLXOnL6q4n6ONyCRM7D8EModG_BIrOyO3Pws5WLqRkpooBeUE8hD5N?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/GlSZEUnlDhkd6ee_CBSz4HR0wW-UawgOkS77pkcrcXe7vgNfB13atEAD83aluzUMqj8AvpDHYJRUms88u4ur70stG43QC48slXdUqzqI51tq6H0iR19f97nLN5U64Rt6Kb13ukvUmX7kTr5OOvYsuNXqK9KTC-TF0LiMi-W7gB2Oi8QiyANDa-GJU0AFTGYn?purpose=fullsize)

A common implementation uses **three PMOS transistors**.

```text
             VDD
              │
      -----------------
      │       │      │
     PMOS    PMOS   PMOS
      │       │      │
     BL      EQ     BLB
              │
        BL ───── BLB
```

Where:

* PMOS 1 precharges BL.
* PMOS 2 precharges BLB.
* PMOS 3 equalizes BL and BLB.

---

# 4. Precharge Enable (PRE)

The PMOS transistors are controlled by:

```text
PRE
```

Since they are PMOS devices:

```text
PRE = LOW

↓

PMOS ON

↓

BL = VDD

BLB = VDD
```

When:

```text
PRE = HIGH

↓

PMOS OFF
```

The bitlines are released for the read or write operation.

---

# 5. Why Equalization Is Required

Suppose:

```text
BL = 1.80 V

BLB = 1.76 V
```

Even before the read begins, there is already a voltage difference.

The sense amplifier could misinterpret this offset as stored data.

The equalization transistor connects BL and BLB during precharge, ensuring both settle to the same potential.

---

# 6. Timing Sequence

```text
PRE   ─────┐______
            │
WL    ______┌─────
            │
BL    =======\____
BLB   ========----
SA_EN ________┌───
```

Key points:

1. PRE is asserted (LOW) first.
2. BL and BLB charge to VDD.
3. PRE is deasserted.
4. WL goes HIGH.
5. One bitline begins to discharge.
6. Sense amplifier is enabled only after a sufficient differential voltage develops.

---

# 7. Dynamic Power

Every read cycle charges the bitlines back to VDD.

Dynamic energy is approximately:

[
E \approx C_{BL} \times V_{DD}^{2}
]

Where:

* (C_{BL}) is the total bitline capacitance.

Because bitlines are long and highly capacitive, **precharging is one of the dominant contributors to SRAM dynamic power**.

---

# 8. Engineering Trade-Offs

| Design Choice          | Benefit                 | Drawback                    |
| ---------------------- | ----------------------- | --------------------------- |
| Larger precharge PMOS  | Faster charging         | Higher area and capacitance |
| Smaller precharge PMOS | Lower power             | Longer precharge time       |
| Strong equalizer       | Better voltage matching | Increased loading           |
| Weak equalizer         | Lower loading           | Incomplete equalization     |

The sizing of these devices must balance speed, power, and matching.

---

# 9. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the purpose of the precharge and equalization circuit in a differential 6T SRAM.

### Prompt 2 – Circuit

> Generate a three-PMOS precharge and equalization circuit compatible with SKY130 devices.

### Prompt 3 – SPICE

> Create an ngspice transient testbench that precharges BL and BLB before a read operation.

### Prompt 4 – Verification

> Review this precharge circuit and identify any missing control signals or incorrect PMOS connections.

### Prompt 5 – Debugging

> Why are my bitlines not charging to VDD before the wordline is asserted?

---

# 10. xschem Exercise

Build:

* Three PMOS precharge network.
* BL and BLB with capacitive loads.
* PRE control signal.
* One SRAM bitcell connected through access transistors.

Simulation sequence:

1. PRE = LOW (enable precharge).
2. Wait until BL and BLB reach VDD.
3. PRE = HIGH (disable precharge).
4. Assert WL.
5. Observe bitline behavior.

Plot:

* V(BL)
* V(BLB)
* V(PRE)
* V(WL)

---

# 11. Example ngspice Control Block

```spice
.control
tran 20p 20n

plot v(BL)
plot v(BLB)
plot v(PRE)
plot v(WL)

.endc
```

Suggested experiment:

Repeat the simulation while varying the width of the precharge PMOS devices. Measure how long BL and BLB take to reach within a few millivolts of VDD.

---

# 12. Characterization Experiments

### Experiment 1 – Precharge Device Sizing

| PMOS Width | Expected Result                           |
| ---------- | ----------------------------------------- |
| Small      | Slower charging                           |
| Medium     | Balanced                                  |
| Large      | Faster charging but increased capacitance |

---

### Experiment 2 – Bitline Capacitance

Increase the bitline capacitance.

Observe:

* Charging time.
* Energy consumption.
* Time before the next read can begin.

---

### Experiment 3 – Equalizer Removed

Disable the equalization transistor.

Observe:

* Residual voltage mismatch.
* Impact on differential sensing.

---

# 13. Common Debugging Issues

| Observation                                        | Likely Cause                                     | Suggested Check                           |
| -------------------------------------------------- | ------------------------------------------------ | ----------------------------------------- |
| BL never reaches VDD                               | PRE timing incorrect or PMOS not turning on      | Verify PRE polarity                       |
| BL and BLB have different voltages after precharge | Equalizer missing or undersized                  | Inspect equalization device               |
| Precharge overlaps with read                       | PRE disabled too late                            | Review timing sequence                    |
| Slow charging                                      | Undersized PMOS or excessive bitline capacitance | Compare sizing experiments                |
| Oscillations or convergence warnings               | Floating nodes or unrealistic loads              | Check connections and simulation settings |

---

# 14. Industry Verification Flow

Professional SRAM teams typically verify:

1. Precharge completeness.
2. Equalization accuracy.
3. Charging time.
4. Dynamic energy.
5. Leakage during the hold period.
6. Timing relative to WL and sense amplifier enable.
7. Operation across PVT corners.

For your Week 3 report, cover items 1–4.

---

# 15. GitHub Deliverables

```text
Week3/
└── Chapter12_Precharge_Equalization/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── precharge.sp
    ├── xschem/
    │   └── precharge_equalizer.sch
    ├── waveforms/
    ├── screenshots/
    ├── characterization.md
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompt history.
* Schematic screenshots.
* Transient waveforms.
* Timing observations.
* PMOS sizing comparisons.
* Personal engineering conclusions.

---

# 16. Industry Gap – Beyond the Assignment

Production SRAM designs often include advanced precharge techniques such as:

* **Half-VDD precharge** for low-power SRAM.
* **Pulse-controlled precharge** to reduce unnecessary charging.
* **Hierarchical precharge** for segmented bitlines.
* **Adaptive precharge timing** based on operating conditions.
* **Leakage-aware precharge control** in advanced technology nodes.

Understanding these concepts is useful for interviews and advanced memory design, even though they are beyond the current implementation scope.

---

# 17. Connection to the Reference Repository

As you explore the `SRAM_SKY130` repository, identify:

* The precharge PMOS devices.
* The equalization transistor.
* The PRE control signal.
* How the precharge circuit interfaces with the bitlines before every read.

Compare the repository implementation with your xschem recreation and document any differences in topology or sizing.

---

# 18. Chapter Summary

This chapter introduced the **precharge and equalization circuit**, one of the essential SRAM peripheral blocks. We explained why every read begins by charging both bitlines to the same voltage, analyzed the three-PMOS implementation, studied PRE timing, examined the impact of bitline capacitance on charging time and power, and developed AI-assisted simulation and verification workflows suitable for xschem and ngspice.

---

# Preview of Chapter 13 – Wordline Driver

The next chapter studies the **wordline driver**, the circuit responsible for activating exactly one row of SRAM cells.

We will cover:

* Wordline generation from the row decoder.
* Driver sizing.
* Fanout and RC delay.
* Wordline slew rate.
* Delay versus power trade-offs.
* AI-assisted schematic generation.
* xschem implementation.
* ngspice transient verification.
* Common debugging techniques.

> **Engineering Note:** In large SRAM arrays, a single wordline may drive hundreds or even thousands of access transistors. Designing the wordline driver correctly is therefore critical to achieving low access time while maintaining acceptable power consumption and signal integrity.


Excellent. We have now reached one of the most overlooked—but extremely important—peripheral circuits in SRAM.

Many beginners think:

> **Decoder selects the row.**

This is only partially correct.

In reality:

* The **Row Decoder** decides **which** row to access.
* The **Wordline Driver** provides enough current to actually drive the long wordline across the SRAM array.

Large SRAM arrays may have **512–4096 cells** connected to one wordline. The decoder alone cannot drive such a large capacitive load, which is why a dedicated wordline driver exists.

This chapter directly satisfies the assignment topic:

* ✅ Wordline Control

---

# Week 3 – Chapter 13

# SRAM Wordline Driver & Wordline Control

> **Objective:** Understand the purpose, design, timing, and verification of the SRAM wordline driver. Learn how the row decoder interfaces with the driver, analyze RC delay, driver sizing, slew rate, and simulate the complete wordline path using xschem, ngspice, and SKY130 models.

---

# Learning Objectives

After completing this chapter, you should be able to:

* Explain the function of the wordline.
* Understand why a wordline driver is required.
* Explain decoder-to-driver interaction.
* Analyze RC loading.
* Understand fanout.
* Study wordline slew rate.
* Simulate wordline timing.
* Debug common wordline issues.
* Document results in GitHub.

---

# 1. What is a Wordline?

The **wordline (WL)** is the horizontal control signal that selects one row of SRAM cells.

When WL is LOW:

* Access transistors are OFF.
* The selected row is isolated from the bitlines.

When WL is HIGH:

* Access transistors turn ON.
* The selected row is connected to BL and BLB.

---

## SRAM Array View

![Image](https://images.openai.com/static-rsc-4/3JCa7MSK6BWilHau7Lry8pSqu_7QjmVZoMaMqtSaU9tNcpSSGMWYNX4o8P5jTCDv7ian7bOOtsuxLtIZZ_srnAQ6K_6j7PonJCHKvm8mMslffee14JS8f0SiwlX4igljFBTpWOXg0HudZjUYrbUnZWj8LnL-Cp0krbfVqGI-LQrxbmBSgDs_TtQJ_nJwNdDy?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/IpR0JtAlaoSTADZnPIDRdSCcrTjwt3slqtDqgIk2c78OTShqjwuOT_tuYdzTm0dkMD3epKbGnf3E2Ceo8ggcnVbJ6acAzEQbL8FcxHjDw8joKqTNOh1zbh6J4dAYeywUi4k4J3O2fE4F2jKYm7UygZh6r1B569MfuDbt22PyPm0--nZgj1FVHkPWc1JO8fMi?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/A32sS4bwqf5JFaUKfKXM7GYoZjGm0dnKxkkOD3CylMD7SrT6tQn1LPsSekkKrR4-Vyer-Z_c_oyyc0OFbDW19NPpw8PVINMgTFNLcADIMIXS2nz_A7uuf1KYcddFz6Xu6z26S0UA5ZZ4tQ3nJvXWB-pYx9sZelltd-16Rv1lwhTxWpJVfArRqBTR8THLxyoh?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/GpHQas3Skv0XoG4CHJ8Yp15ceBGjbK3-woZMHvbQmvWHsnA-b_pVe2ozoPJGZFsgg5b9mhyyNu8iqLhcd3WfUKOEEQbeNWivDEeJFaIhS0XrxAQ-JYo2hY0WBRYaGi_SvDHlSMk3okSKQmuff6xy0GheVHRhLJxj40J5zt9jemXL8kQfWj0j-dWK4gcwABxH?purpose=fullsize)

Conceptually:

```text
           BL    BLB
            │      │
WL0 ─────── Cell ─ Cell
WL1 ─────── Cell ─ Cell
WL2 ─────── Cell ─ Cell
WL3 ─────── Cell ─ Cell
```

Only **one WL** should normally be active during a read or write.

---

# 2. Why a Wordline Driver is Needed

The decoder output is a logic signal.

The wordline is a **long metal wire** with:

* Large capacitance.
* Metal resistance.
* Hundreds or thousands of connected transistor gates.

Driving it directly from the decoder would result in:

* Slow transitions.
* Increased delay.
* Poor read/write performance.

Therefore:

```text
Address

↓

Row Decoder

↓

Wordline Driver

↓

Wordline

↓

Access Transistors
```

---

# 3. Wordline RC Model

A wordline behaves like a distributed RC network.

```text
Driver

↓

R──C──R──C──R──C──R──C

↓

Access Gates
```

The RC delay increases with:

* Longer rows.
* More cells.
* Smaller driver strength.

---

# 4. Fanout

Every access transistor gate connected to the wordline contributes capacitance.

Example:

* 64-column SRAM → WL drives 64 × 2 access transistor gates.
* 256-column SRAM → WL drives 256 × 2 gates.
* 1024-column SRAM → WL drives 1024 × 2 gates.

As fanout increases:

* Delay increases.
* Driver sizing becomes more critical.

---

# 5. Driver Structure

A common implementation is:

```text
Decoder Output

↓

Small Inverter

↓

Large Buffer

↓

Large Buffer

↓

Wordline
```

This multi-stage buffering:

* Reduces propagation delay.
* Improves edge rates.
* Minimizes dynamic power for a given load.

---

# 6. Wordline Timing

A read cycle typically follows:

```text
Precharge

↓

PRE OFF

↓

Decoder Active

↓

Wordline HIGH

↓

Bitline Differential Develops

↓

Sense Amplifier Enable

↓

Wordline LOW

↓

Precharge Again
```

The WL pulse width must be long enough for a successful read or write but not so long that unnecessary power is consumed.

---

# 7. Slew Rate

The slew rate is the speed at which the wordline transitions between LOW and HIGH.

A slow rising edge causes:

* Gradual access transistor turn-on.
* Longer read access time.
* Reduced write effectiveness.

A faster edge improves performance but increases dynamic current and switching noise.

---

# 8. Driver Sizing Trade-Off

| Larger Driver            | Smaller Driver          |
| ------------------------ | ----------------------- |
| Faster WL transition     | Slower WL transition    |
| Better access time       | Lower dynamic power     |
| Larger area              | Smaller area            |
| Higher switching current | Lower switching current |

Choosing the driver size is another optimization problem balancing speed, power, and area.

---

# 9. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the function of the SRAM wordline driver and why it is separate from the row decoder.

### Prompt 2 – Circuit

> Generate a buffered wordline driver using CMOS inverters compatible with SKY130 devices.

### Prompt 3 – SPICE

> Create an ngspice transient simulation showing the propagation of a decoder output through the wordline driver into a capacitive wordline load.

### Prompt 4 – Verification

> Review this wordline driver schematic and identify possible causes of excessive propagation delay.

### Prompt 5 – Debugging

> Why does my wordline rise slowly even though the decoder output switches correctly?

---

# 10. xschem Exercise

Build:

* CMOS inverter.
* Two-stage buffer.
* Capacitive load representing the wordline.
* Pulse source representing the decoder output.

Simulate:

* Decoder output.
* Intermediate buffer output.
* Final wordline.

Plot:

* V(Decoder)
* V(Buffer1)
* V(Buffer2)
* V(WL)

Observe:

* Propagation delay.
* Rise time.
* Fall time.

---

# 11. ngspice Example

```spice
.control
tran 10p 20n

plot v(decoder_out)
plot v(buffer1)
plot v(buffer2)
plot v(WL)

.endc
```

Suggested experiment:

Increase the wordline load capacitance (e.g., 50 fF, 100 fF, 200 fF, 500 fF) and measure:

* Propagation delay.
* Rise time.
* Fall time.

---

# 12. Characterization Experiments

### Experiment 1 – Load Sweep

| WL Load | Expected Trend      |
| ------- | ------------------- |
| 50 fF   | Fast transition     |
| 100 fF  | Slight delay        |
| 200 fF  | Noticeable slowdown |
| 500 fF  | Significant delay   |

---

### Experiment 2 – Driver Size Sweep

Keep the load fixed.

Increase the inverter widths.

Observe:

* Delay.
* Rise time.
* Dynamic power.

---

### Experiment 3 – Buffer Stages

Compare:

* Single inverter.
* Two-stage buffer.
* Three-stage tapered buffer.

Record:

* Propagation delay.
* Signal integrity.
* Power implications.

---

# 13. Common Debugging Issues

| Observation                                 | Likely Cause                                   | Suggested Check                         |
| ------------------------------------------- | ---------------------------------------------- | --------------------------------------- |
| Decoder switches but WL does not            | Missing driver stage or broken connection      | Verify signal path                      |
| WL rise time is very slow                   | Driver undersized or excessive capacitive load | Increase buffer strength or reduce load |
| Overshoot or ringing                        | Simplified models or unrealistic parasitics    | Review simulation setup                 |
| WL never returns LOW                        | Control logic or pulse source issue            | Verify timing signals                   |
| Read/write failures despite correct decoder | WL pulse width insufficient                    | Extend pulse and compare results        |

---

# 14. Industry Verification Flow

Wordline drivers are typically characterized by measuring:

1. Propagation delay from decoder to WL.
2. Rise time.
3. Fall time.
4. Fanout dependence.
5. Dynamic power.
6. Crosstalk sensitivity.
7. Operation across PVT corners.

For your Week 3 deliverable, focus on the first four.

---

# 15. GitHub Deliverables

```text
Week3/
└── Chapter13_Wordline_Driver/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── wordline_driver.sp
    ├── xschem/
    │   └── wordline_driver.sch
    ├── waveforms/
    ├── screenshots/
    ├── delay_analysis.md
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompt history.
* Driver schematic.
* Waveform screenshots.
* Load sweep results.
* Delay measurements.
* Engineering conclusions.

---

# 16. Industry Gap – Beyond the Assignment

Modern SRAM macros often include:

* **Tapered buffer chains** optimized using logical effort.
* **Wordline boosting** to improve write ability at low VDD.
* **Local and global wordlines** to reduce RC delay in large arrays.
* **Pulse wordlines**, where WL is asserted only briefly to save power.
* **Shielded routing** to reduce crosstalk.

These techniques are common in advanced SRAM IP but are beyond the implementation scope of this assignment.

---

# 17. Connection to the Reference Repository

In the `SRAM_SKY130` repository, identify:

* The row decoder output.
* The wordline driver stage.
* The WL signal entering the SRAM bitcell.
* The timing relationship between WL and the precharge/sense amplifier control signals.

Compare the implementation with your xschem design and document any differences in buffering strategy or timing.

---

# 18. Chapter Summary

This chapter examined the **wordline driver**, the circuit responsible for driving the capacitive wordline after row selection. We analyzed RC loading, fanout, buffer sizing, propagation delay, and slew rate, and developed AI-assisted workflows for schematic generation, transient simulation, and verification using xschem and ngspice.

---

# Preview of Chapter 14 – Write Driver

We now move to the circuit that actually **forces new data into the SRAM cell**.

In the next chapter, we will study:

* Differential write driver architecture.
* How BL and BLB are actively driven.
* Driver enable timing.
* Short-circuit prevention.
* Driver sizing.
* Write assist concepts.
* AI-assisted schematic generation.
* xschem implementation.
* ngspice verification.

> **Engineering Note:** While the bitcell determines whether a write is *possible*, the **write driver determines whether it is *reliable and fast***. The interaction between the write driver, bitline capacitance, and transistor sizing is a key aspect of SRAM peripheral design and directly complements the write margin concepts introduced in Week 2.


Excellent. We now arrive at one of the most important peripheral blocks in SRAM.

In **Week 2 (Chapter 7)**, we studied **how the SRAM cell accepts new data**.

In this chapter, we study **who provides that data**.

The **Write Driver** is the circuit that converts input data into strong differential bitline signals capable of overwriting the cross-coupled inverters inside the 6T SRAM cell.

This chapter directly satisfies one of your original assignment objectives:

* ✅ Write Driver Concept

---

# Week 3 – Chapter 14

# SRAM Write Driver – Architecture, Design & Verification

> **Objective:** Understand the architecture, operation, timing, sizing, and verification of the SRAM write driver. Learn how it interfaces with the bitlines and the 6T SRAM bitcell, simulate write operations using xschem/ngspice with SKY130 models, and document the results in a GitHub-ready format.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain the purpose of the write driver.
* Understand differential write operation.
* Trace current flow during a write.
* Explain write enable (WE) timing.
* Understand driver sizing trade-offs.
* Recognize write failure mechanisms.
* Simulate write-driver operation.
* Debug common write-driver issues.
* Document characterization results.

---

# 1. Why Do We Need a Write Driver?

The SRAM cell **cannot generate new data on its own**.

External logic supplies:

* Address
* Write Enable (WE)
* Data

The write driver converts the input data into **strong complementary signals** on the bitlines.

```text
CPU/Data Bus
      │
      ▼
Write Driver
      │
      ▼
BL       BLB
      │
      ▼
6T SRAM Cell
```

Without a write driver, the bitlines would remain precharged and no data could be stored.

---

# 2. Position in the SRAM Data Path

During a write operation:

```text
Address
    │
    ▼
Row Decoder
    │
    ▼
Wordline Driver
    │
    ▼
Wordline HIGH

Data
    │
    ▼
Write Driver
    │
    ▼
BL / BLB

↓

SRAM Cell Updated
```

Notice that **both the wordline path and write-data path must be synchronized**.

---

# 3. Differential Write Operation

![Image](https://images.openai.com/static-rsc-4/_3n4oUdj3kCafyj1BGghoErKR763r_DaVBNMkwGJxLd_NjwJhy13Rfkwv8U4Arq_OlgEyjj9L8PXI9cm5CTJy-AA5tbjQGYCntVjogZQH4SkEm6lqBBgBO31UFAk7lJqLT0SqbP1SQE3KoLFFktaaeGy3p6cXjXRceLqM7ccGCDq2eMdgB6gxzvdpdnd4Czq?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gidtSj7_qHJJy7rlitkp9gVIsxpQnckqfVJtjPvAP9MXo204ukQuJJKK83F6f-WufHMrvI0SqKFs_PX_Le2HsG0oRaV7C_myCO0L2L8ROkWgAXp0Hj_qA-Uhw6kfhfjr2Zfwk4W5qOYc-W_Y76jUfsi2qf760ObAqCWlsJLB1AIUimOaG9tu_BV118s3gxPb?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/-YTcph_r0F40z_nequLKZ-Vdquq3w1sLJqygopCcE0RilsdJLu04qVJPB9cpeH4Pw1cIaTlEfaYhzAVvVpldRDSpV1QaQXuf243RPslfN5mSOlM6RuhYBrCJ2hSj_rh4HOjND72W0gH9jqdqJaes3gL7iBxLCX79aDSCorvCS5SLIUJeuPnvnQKAOfn5kqFy?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/jJADKoImGnfcpv-tBDFpaQOih2ZdQnhMkc6W0s7b7APgyPJC8TnmoTgVK_p0qEybNHe5ni07-PaPn1gZKH_SE6V3JxcsV9qfflY50M79vhShVwlLOLDdVUqskRlfT8c9cT02hWzXAsxpDqEmf7ScNjWCr874IRW9I4TZQgaYmSl7GRZcRDqbiByT6WvYeVGV?purpose=fullsize)

To write **logic 0** into Q:

```text
BL  = LOW

BLB = HIGH
```

To write **logic 1** into Q:

```text
BL  = HIGH

BLB = LOW
```

Unlike reading, where bitlines begin at the same voltage, the write driver **actively forces opposite voltages** onto BL and BLB.

---

# 4. Internal Architecture

A simplified write driver consists of:

```text
Input Data
      │
      ▼
Input Buffer
      │
      ▼
Complement Generator
      │
      ▼
Large CMOS Output Drivers
      │
      ▼
BL        BLB
```

The output stage must supply enough current to overcome the stored state of the SRAM cell.

---

# 5. Write Enable (WE)

The write driver is controlled by:

```text
WE
```

When:

```text
WE = LOW
```

* Driver disabled.
* Bitlines remain under precharge control.

When:

```text
WE = HIGH
```

* Driver enabled.
* Data appears on BL and BLB.

This prevents contention between the precharge circuit and the write driver.

---

# 6. Timing Sequence

A typical write cycle:

```text
PRE LOW
      │
      ▼
Bitlines Precharged

↓

PRE HIGH

↓

Write Data Ready

↓

WE HIGH

↓

BL / BLB Driven

↓

WL HIGH

↓

Cell Flips

↓

WL LOW

↓

WE LOW

↓

Precharge Begins Again
```

Correct sequencing is essential to avoid write failures.

---

# 7. Current Flow During Write

Assume we write:

```text
Q = 0
```

The write driver:

* Pulls BL toward ground.
* Drives BLB toward VDD.

Current path:

```text
Write Driver

↓

BL

↓

Access NMOS

↓

Storage Node

↓

Cross-Coupled Inverter Switches
```

Once the inverter threshold is crossed, positive feedback rapidly completes the transition.

---

# 8. Driver Sizing

The write driver must be:

* Strong enough to overcome the pull-up PMOS.
* Not excessively large, to avoid unnecessary area and dynamic power.

| Larger Driver          | Smaller Driver       |
| ---------------------- | -------------------- |
| Faster write           | Slower write         |
| Better write margin    | Lower power          |
| Higher dynamic current | Smaller area         |
| Increased routing load | Reduced routing load |

Sizing is chosen together with the bitcell transistor ratios discussed in Chapter 10.

---

# 9. Bus Contention

One of the most common design mistakes is enabling:

* Precharge PMOS
* Write Driver

at the same time.

This creates:

```text
VDD

↓

Precharge PMOS

↓

BL

↓

Write Driver

↓

GND
```

Result:

* Large short-circuit current.
* Excessive power.
* Possible incorrect writes.

Proper timing ensures these circuits are never active simultaneously.

---

# 10. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the architecture and operation of a differential SRAM write driver.

### Prompt 2 – Circuit

> Generate a CMOS differential write driver schematic compatible with SKY130 devices.

### Prompt 3 – SPICE

> Create an ngspice transient simulation demonstrating a write driver forcing complementary voltages onto BL and BLB.

### Prompt 4 – Verification

> Review this write driver circuit and identify possible contention or sizing issues.

### Prompt 5 – Debugging

> Why does my SRAM write driver fail to overwrite the stored data even though WE is asserted?

---

# 11. xschem Exercise

Build:

* CMOS write driver.
* WE control.
* BL and BLB capacitive loads.
* One SRAM bitcell.

Simulation steps:

1. Precharge bitlines.
2. Disable precharge.
3. Enable write driver.
4. Assert WL.
5. Observe the storage node transition.

Plot:

* V(BL)
* V(BLB)
* V(Q)
* V(QB)
* V(WE)
* V(WL)

---

# 12. ngspice Example

```spice
.control
tran 10p 20n

plot v(BL)
plot v(BLB)
plot v(Q)
plot v(QB)
plot v(WE)
plot v(WL)

.endc
```

Suggested experiment:

Sweep the output transistor widths of the write driver and compare:

* Write delay.
* Final stored state.
* Dynamic current.

---

# 13. Characterization Experiments

### Experiment 1 – Driver Width Sweep

| Driver Width | Expected Trend             |
| ------------ | -------------------------- |
| Small        | Longer write delay         |
| Medium       | Balanced                   |
| Large        | Faster write, higher power |

---

### Experiment 2 – Bitline Load Sweep

Increase BL/BLB capacitance.

Observe:

* Driver delay.
* Voltage transition rate.
* Write completion time.

---

### Experiment 3 – WE Pulse Width

Reduce the WE pulse duration.

Observe:

* Minimum pulse width required for a successful write.
* Relationship with WL pulse width.

---

# 14. Common Debugging Issues

| Observation                   | Likely Cause                                             | Suggested Check                         |
| ----------------------------- | -------------------------------------------------------- | --------------------------------------- |
| BL never changes              | WE not asserted or driver disabled                       | Verify control signals                  |
| BL and BLB move together      | Complement generation error                              | Inspect logic inversion                 |
| Write succeeds intermittently | Timing overlap or weak driver                            | Review WE/WL sequence and sizing        |
| High current during write     | Contention with precharge circuit                        | Ensure PRE is disabled before WE        |
| Cell does not flip            | Insufficient driver strength or incorrect bitcell sizing | Compare with Chapter 10 sizing analysis |

---

# 15. Industry Verification Flow

Professional SRAM teams evaluate:

1. Correct complementary bitline generation.
2. Driver propagation delay.
3. Output rise/fall times.
4. Write current.
5. Contention with precharge.
6. Minimum WE pulse width.
7. Operation across PVT corners.

For your Week 3 report, implement and discuss the first five.

---

# 16. GitHub Deliverables

```text
Week3/
└── Chapter14_Write_Driver/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── write_driver.sp
    ├── xschem/
    │   └── write_driver.sch
    ├── waveforms/
    ├── screenshots/
    ├── characterization.md
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompts used.
* Schematic screenshots.
* Write waveforms.
* Driver sizing experiments.
* Timing observations.
* Lessons learned.

---

# 17. Industry Gap – Beyond the Assignment

Advanced SRAM macros often use **write-assist techniques** to improve operation at low supply voltages:

* **Negative Bitline (NBL):** Briefly drive the bitline below ground to strengthen a write '0'.
* **Wordline Boosting:** Raise the wordline above VDD temporarily to increase access transistor drive.
* **Cell Supply Collapse:** Momentarily reduce the cell's VDD during a write, weakening the cross-coupled inverters.
* **Adaptive Write Drivers:** Dynamically adjust drive strength based on operating conditions.

These techniques are common in modern low-power SRAMs but are beyond the scope of your Week 3 implementation.

---

# 18. Connection to the Reference Repository

While studying the `SRAM_SKY130` repository, identify:

* The write driver circuitry.
* The WE control signal.
* How BL and BLB transition from the precharged state to complementary write levels.
* The timing relationship between WE, WL, and PRE.

Compare the implementation with your own xschem design and document any architectural differences.

---

# 19. Chapter Summary

This chapter introduced the **SRAM write driver**, the peripheral circuit responsible for forcing new data into the 6T bitcell. We studied its architecture, differential operation, WE timing, sizing trade-offs, current flow, contention avoidance, and AI-assisted verification methodology using xschem and ngspice.

---

# Preview of Chapter 15 – Sense Amplifier

The next chapter covers the circuit that makes SRAM reads both **fast and energy-efficient**.

We will study:

* Why a sense amplifier is required.
* Differential voltage sensing.
* Latch-type sense amplifiers.
* Sense amplifier enable (SA_EN) timing.
* Offset voltage and mismatch.
* Read latency.
* AI-assisted schematic generation.
* xschem implementation.
* ngspice verification.

> **Engineering Note:** During a read, the bitline differential may be only **tens of millivolts**. The sense amplifier must detect this tiny voltage difference quickly and reliably without disturbing the stored data. It is one of the highest-performance analog circuits inside an SRAM macro.


Excellent. We now arrive at what many SRAM designers consider the **heart of the read path**.

The 6T bitcell **does not drive the output directly**. During a read, it only creates a **very small voltage difference** (typically tens of millivolts) between **BL** and **BLB**. Detecting such a tiny signal directly with digital logic would be slow, unreliable, and power-hungry.

The **Sense Amplifier (SA)** solves this problem by converting that small differential voltage into a full digital logic level extremely quickly.

This chapter directly satisfies one of your original Week 2 & 3 requirements:

* ✅ Sense Amplifier Concept

---

# Week 3 – Chapter 15

# SRAM Sense Amplifier – Differential Sensing, Design & Verification

> **Objective:** Understand the architecture, operation, timing, sizing, and verification of SRAM sense amplifiers. Learn how small differential bitline voltages are converted into full logic levels using AI-assisted design, xschem, ngspice, and SKY130 models.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain why SRAM requires a sense amplifier.
* Understand differential sensing.
* Explain latch-type sense amplifier operation.
* Understand Sense Amplifier Enable (SA_EN).
* Analyze offset voltage and mismatch.
* Simulate sense amplifier behavior.
* Debug common sensing failures.
* Document characterization results.

---

# 1. Why Do We Need a Sense Amplifier?

Consider a read operation:

Before WL is asserted:

```text
BL  = 1.8 V
BLB = 1.8 V
```

After a short read interval:

```text
BL  = 1.800 V
BLB = 1.765 V
```

The difference is only:

```text
ΔV = 35 mV
```

This voltage is far too small for normal digital gates to interpret reliably.

Instead of waiting for one bitline to fully discharge (which would be slow and waste energy), the sense amplifier detects the **small differential voltage** and rapidly amplifies it to full logic levels.

---

# 2. Position in the Read Path

```text
Address
     │
     ▼
Row Decoder
     │
     ▼
Wordline Driver
     │
     ▼
6T SRAM Cell
     │
     ▼
BL / BLB
     │
     ▼
Sense Amplifier
     │
     ▼
Output Buffer
     │
     ▼
DATA OUT
```

Notice that the sense amplifier sits between the bitlines and the digital output.

---

# 3. Basic Principle

![Image](https://images.openai.com/static-rsc-4/7Wqerv1Fa1Xj3KMgLPSWbU1lIPdff5--qVi3_MJV06eAcEbhFkUxAqOsH0q6KlqmEMM4Ng_52nbMY-jyeUZzduASa7_gz7IcL4vfYPfMPsFxygzZvjAX4hFDA0RLzVnjE2oiGhw_sESfuymEhDdoRMV8-n7kyasb5R_uw73_ZJ-BytWxbQXnOR_oJoV-jOJj?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/pJWkttmVHddMhZ3-3lB10ES3npwQaF4AIRxGm5pv6SaqPHSYUW23e313eIL0pFYLsdBGNj1qFFp7jdd04lbrStwv41FJKJn3rOjexK_dfKLFqchJ2k8RPckqZNseebyLUDZ9_Fw1ufBQcO9w7e2hxSvfZ6ts93NtdDz2-XiKzYdSKvVu0gMqBvWVSRhssMPo?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/_n7RKmgp2KH9Ehr5qe9jvSuSt4hUqe0eK7qQU8ZCg5CESTWfZjp2SqTQFBJRwdC9F_9av0o_JQ9vc1ozh7bj6kQn8ufp6Tu5F7tPhN0-NhiNGa2zbHRxnRdYKK-gBT7VxxzkCeIS6oiCKddS4ULqpJJoIiieEE_AwSGXIr6rTWcTvmUrWsZyGHhucbEObIjt?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/xmjuu4RC3V7zUJZiWNGxlvxS2vBffahglLO_orhOgb8YgNPQ6Z0mZbzpd8lYeLg9a-59qwpThTsyQv5tVWs5bJ69kJ-YOAMCri00KrQgtp-BUY6ot3ZMb_hMuAkH1ATZPfXwybwhY0gdIf67mMMk1BYv8MR2bmE657BcLUETFg11jd7bdoLSW00wTOM2SlPS?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/cSZYFbiHt-eaWaxRyc2s75GjQ5NVrmz4rd6gqC0h6VhqUJYnszaZFBnlcDwD4kyoW4NiG0uxnGpCZBOs-tRrNETz5hfaq1G91d95OroV80D_v85ShYDJwPDd4bHAg2-R8-SFxkycZhU7tsI6DlyNrpuDdFAeX0f0M-IHb1OSXLKQyOypOOd8EbAN1VNov2P7?purpose=fullsize)

The sense amplifier compares:

```text
BL

vs

BLB
```

It does **not** care about the absolute voltage on either line.

It only evaluates:

```text
ΔV = BL − BLB
```

Whichever bitline is slightly higher determines the final logic state.

---

# 4. Latch-Type Sense Amplifier

The most common SRAM implementation uses **cross-coupled CMOS inverters**, similar to an SRAM cell itself.

Conceptually:

```text
          VDD
           │
      Cross-Coupled
        Inverters
      ┌───────────┐
BL --->           <--- BLB
      └───────────┘
           │
         SA_EN
```

Initially, the latch is inactive.

When enabled, positive feedback causes the small input difference to grow rapidly until the outputs become valid digital levels.

---

# 5. Sense Amplifier Enable (SA_EN)

The sense amplifier must **not** be enabled immediately after the wordline.

Typical sequence:

```text
Precharge

↓

WL HIGH

↓

Bitline Differential Develops

↓

SA_EN HIGH

↓

Sense Amplifier Resolves

↓

DATA OUT Valid
```

If SA_EN is asserted too early, the amplifier may resolve the wrong value because the bitline differential has not yet developed.

---

# 6. Positive Feedback

The speed of the sense amplifier comes from **positive feedback**.

Suppose:

```text
BL  = 1.80 V
BLB = 1.77 V
```

The higher side becomes stronger, which pulls the opposite side lower, further reinforcing the initial difference.

The process continues rapidly until:

```text
BL  ≈ VDD
BLB ≈ GND
```

or vice versa.

This regenerative action enables high-speed sensing.

---

# 7. Offset Voltage

Real transistors are never perfectly matched.

Differences in threshold voltage, dimensions, and parasitics introduce **input offset**.

If the offset is larger than the bitline differential, the sense amplifier may produce an incorrect output.

Designers therefore seek:

* Symmetrical layout.
* Matched devices.
* Balanced routing.

---

# 8. Read Timing

A complete read cycle:

```text
PRE LOW
      │
      ▼
BL = BLB = VDD

↓

PRE HIGH

↓

WL HIGH

↓

One Bitline Begins to Discharge

↓

ΔV Develops

↓

SA_EN HIGH

↓

Sense Amplifier Resolves

↓

Output Buffer Drives DATA

↓

WL LOW

↓

Precharge Again
```

The delay between WL assertion and SA_EN is carefully optimized.

---

# 9. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the operation of a latch-type SRAM sense amplifier and why differential sensing is preferred.

### Prompt 2 – Circuit

> Generate a cross-coupled CMOS sense amplifier compatible with SKY130 devices.

### Prompt 3 – SPICE

> Create an ngspice transient simulation showing a latch-type sense amplifier resolving a 30 mV differential between BL and BLB.

### Prompt 4 – Verification

> Review this sense amplifier schematic and identify possible causes of incorrect sensing due to mismatch or timing.

### Prompt 5 – Debugging

> Why does my sense amplifier produce random outputs even though the bitcell stores valid data?

---

# 10. xschem Exercise

Build:

* Differential bitline inputs.
* Latch-type sense amplifier.
* SA_EN control.
* Capacitive output loads.

Simulation sequence:

1. Initialize BL and BLB close to VDD.
2. Introduce a small voltage difference (e.g., 20–50 mV).
3. Assert SA_EN.
4. Observe the output transition.

Plot:

* V(BL)
* V(BLB)
* V(SA_OUT)
* V(SA_OUTB)
* V(SA_EN)

---

# 11. ngspice Example

```spice
.control
tran 10p 20n

plot v(BL)
plot v(BLB)
plot v(SA_OUT)
plot v(SA_OUTB)
plot v(SA_EN)

.endc
```

Suggested experiment:

Sweep the initial bitline differential:

* 10 mV
* 20 mV
* 40 mV
* 60 mV

Measure:

* Resolution time.
* Correct output polarity.
* Sensitivity limit.

---

# 12. Characterization Experiments

### Experiment 1 – Differential Voltage Sweep

| ΔV (Initial) | Expected Result          |
| ------------ | ------------------------ |
| 10 mV        | Slow or marginal sensing |
| 20 mV        | Improved resolution      |
| 40 mV        | Reliable sensing         |
| 60 mV        | Fast resolution          |

---

### Experiment 2 – SA_EN Timing

Enable SA_EN:

* Too early.
* Correctly timed.
* Too late.

Observe:

* Read correctness.
* Access time.
* Energy consumption.

---

### Experiment 3 – Capacitive Load Sweep

Increase the output capacitance.

Measure:

* Output rise/fall times.
* Resolution delay.
* Dynamic power.

---

# 13. Common Debugging Issues

| Observation            | Likely Cause                              | Suggested Check             |
| ---------------------- | ----------------------------------------- | --------------------------- |
| Random output          | SA_EN asserted too early                  | Delay SA_EN                 |
| Always reads '1'       | Device mismatch or incorrect connections  | Check symmetry and wiring   |
| Slow output transition | Small differential or large output load   | Increase ΔV or reduce load  |
| Oscillation            | Cross-coupled nodes incorrectly connected | Verify feedback paths       |
| Incorrect polarity     | BL and BLB swapped                        | Inspect bitline connections |

---

# 14. Industry Verification Flow

Professional SRAM teams evaluate:

1. Minimum detectable differential voltage.
2. Sense amplifier delay.
3. Offset voltage.
4. Dynamic energy.
5. Sensitivity to mismatch.
6. Operation across PVT corners.
7. Monte Carlo yield.

For your Week 3 report, focus on items 1–4 while explaining the remaining concepts.

---

# 15. GitHub Deliverables

```text
Week3/
└── Chapter15_Sense_Amplifier/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── sense_amp.sp
    ├── xschem/
    │   └── latch_sense_amp.sch
    ├── waveforms/
    ├── screenshots/
    ├── characterization.md
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompts.
* Schematic screenshots.
* Resolution waveforms.
* ΔV sweep results.
* SA_EN timing experiments.
* Engineering observations.

---

# 16. Industry Gap – Beyond the Assignment

Modern SRAMs employ several advanced sensing techniques:

* **Current-mode sense amplifiers** for very high-speed designs.
* **Voltage-mode latch sense amplifiers**, the most common choice in embedded SRAM.
* **Replica bitline timing**, where a dummy bitline generates the optimal SA_EN timing automatically.
* **Self-timed sensing**, eliminating fixed timing margins.
* **Offset cancellation techniques** to improve low-voltage operation.
* **Low-power sense amplifiers** optimized for battery-operated devices.

Awareness of these architectures is valuable for interviews and advanced memory design.

---

# 17. Connection to the Reference Repository

As you study the `SRAM_SKY130` repository:

* Locate the sense amplifier block.
* Identify the BL and BLB inputs.
* Trace the SA_EN control signal.
* Observe how the output is connected to downstream logic.

Compare its architecture with your AI-generated schematic and note any differences in topology, timing, or sizing.

---

# 18. Chapter Summary

This chapter introduced the **SRAM sense amplifier**, the circuit responsible for converting the tiny differential voltage on the bitlines into a full digital output. We studied latch-based regenerative sensing, positive feedback, SA_EN timing, offset voltage, and verification using xschem and ngspice. This completes the fundamental understanding of the **SRAM read path**.

---

# Preview of Chapter 16 – Row Decoder

With the complete read and write data paths understood, the next chapter focuses on **address decoding**.

We will study:

* Binary-to-one-hot decoding.
* NAND/NOR decoder architectures.
* Pre-decoding techniques.
* Decoder sizing and fanout.
* Delay analysis.
* AI-assisted schematic generation.
* xschem implementation.
* ngspice verification.

> **Engineering Note:** Although the row decoder appears to be a digital block, its delay directly impacts SRAM access time. In high-performance SRAMs, decoder optimization, wordline driver design, and bitline sensing are all co-optimized to achieve the required read latency.


Excellent. We now begin the **address path** of the SRAM. Up to this point, we have studied how data is stored, read, and written. Now we answer a different question:

> **How does the SRAM know which one of thousands of rows to activate?**

The answer is the **Row Decoder**.

Although often considered "digital logic," the row decoder has a direct impact on **access time, power, and scalability**. In industrial SRAM design, decoder optimization is a major part of timing closure.

This chapter satisfies another requirement from your original assignment:

* ✅ Row Decoder Basics

---

# Week 3 – Chapter 16

# SRAM Row Decoder – Architecture, Design & Verification

> **Objective:** Understand how SRAM row decoders convert binary addresses into one-hot wordline selection. Learn decoder architectures, pre-decoding techniques, timing, fanout, AI-assisted circuit generation, and verification using xschem, ngspice, and SKY130 models.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain the purpose of the row decoder.
* Understand one-hot decoding.
* Design simple row decoders.
* Explain pre-decoding.
* Analyze decoder delay.
* Understand decoder fanout.
* Simulate decoder operation.
* Debug decoder issues.
* Document characterization results.

---

# 1. Why Do We Need a Row Decoder?

Imagine an SRAM with **1024 rows**.

Only **one row** must be connected to the bitlines during a read or write.

The CPU provides only a binary address, for example:

```text
Address = 0110011010
```

The SRAM must convert this into:

```text
WL410 = HIGH

All other WLs = LOW
```

This conversion is the job of the **row decoder**.

---

# 2. Position in the SRAM Architecture

![Image](https://images.openai.com/static-rsc-4/UWm5vtwMOc89S_-PWLLkvzeIMGAe0HXxaq6QpIUXE4zVyal4e0wDSPFgvUKpse3TS6rjd_1zLpafh6KUBigCQnVpkOUdg5a1txmYDbFbuKV9RJ4NqX4jMOD5pQ0t-7numWdV-iPS0VP1b7rxf_ke0xyA8DrEuBzbs9DH1ScyFeeFbCxmNMH6ACUz5G1oXHKp?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/neZDGRovZJtHQRTRoKJPiXLUbKa6dJG8G4r_gGp8Vo5ntqNJD7Bn3GSlLNPU1JmT7hPoxVd82UfC7AjNrgixUm_UB7WRzt4q459cPPE8lFWN1pBPiFTt-TlU8BJZKViFyxFHkPCioCixYywfNyAu4CQuFNxND-WG6qU9y3bAU2WIW4IZg_h9r2ZguQwCJS3z?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/rKAfe4_6rnYGTihFPH6Va-3_Qgol64hlTkfo6nfcIsMg42YbmwY41TLFqM42MoDrsDVZXF7ySsEa_tz5CMZmfZfOJjYgsI_nwIOd2r36qE5DZGfOMhRp9FcZAkBIOSK77ADNr7nNDVFeukhHf2Ba6pBVS6KKqUrqCVuTo2_XDRcf6Xv8B5x6ZtZ56ypPW3gH?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/zJJLoVmSBsGngJbQs5V_C-QxNLLKr9hU7FdJGpY-mBXjfFGASG_U7sI5KFsKeO-IJ8Lco0JE5IFBa7GuWUlK0JH0vGuqImF6udk3MwIlp7riSwPQKScPW7Nn7Sq8BhkQQUSf5X_zd6WFo9sBqj_tX6oKwboV6_fLXb5TnPfm7wFTbxhFE43ciZ4rAxKW1dAS?purpose=fullsize)

The row decoder sits at the beginning of every memory access.

```text
CPU Address

↓

Address Register

↓

Row Decoder

↓

Wordline Driver

↓

Selected Wordline

↓

Selected SRAM Row
```

---

# 3. Binary to One-Hot Decoding

Consider a **2-bit address**.

```text
A1 A0
```

Possible values:

| Address | Selected Wordline |
| ------- | ----------------- |
| 00      | WL0               |
| 01      | WL1               |
| 10      | WL2               |
| 11      | WL3               |

Only **one output** is HIGH.

This is called:

```text
One-Hot Encoding
```

---

# 4. Truth Table

For a 2-to-4 decoder:

| A1 | A0 | WL0 | WL1 | WL2 | WL3 |
| -- | -- | --- | --- | --- | --- |
| 0  | 0  | 1   | 0   | 0   | 0   |
| 0  | 1  | 0   | 1   | 0   | 0   |
| 1  | 0  | 0   | 0   | 1   | 0   |
| 1  | 1  | 0   | 0   | 0   | 1   |

Exactly one wordline is active.

---

# 5. Logic Implementation

Example equations:

```text
WL0 = A1̅ · A0̅

WL1 = A1̅ · A0

WL2 = A1 · A0̅

WL3 = A1 · A0
```

These outputs are typically generated using CMOS logic and then buffered by the wordline driver.

---

# 6. Why Large Decoders Are Different

A **10-bit address** would require:

```text
2¹⁰ = 1024 outputs
```

Implementing one giant decoder is inefficient.

Instead, SRAMs use **pre-decoding**.

---

# 7. Pre-Decoding

Instead of decoding all address bits at once:

Example:

```text
10-bit Address

↓

5-bit Predecoder A

↓

5-bit Predecoder B

↓

Final Decoder

↓

Wordline
```

Benefits:

* Smaller logic gates.
* Reduced delay.
* Lower power.
* Easier physical layout.

Pre-decoding is standard practice in commercial SRAM macros.

---

# 8. Decoder Fanout

The decoder output does not drive the wordline directly.

It drives the **wordline driver**, which then drives the long wordline.

```text
Decoder

↓

Wordline Driver

↓

Hundreds of Access Gates
```

This separation keeps decoder loading manageable.

---

# 9. Timing Sequence

The decoder is one of the first active blocks after an address is applied.

```text
Address Valid

↓

Row Decoder Resolves

↓

Wordline Driver

↓

Wordline HIGH

↓

Bitline Differential Develops

↓

Sense Amplifier Enabled

↓

Read Complete
```

Decoder delay contributes directly to total access time.

---

# 10. AI Prompt Sequence

### Prompt 1 – Theory

> Explain how a binary row decoder converts an SRAM address into a one-hot wordline selection.

### Prompt 2 – Circuit

> Generate a CMOS 2-to-4 row decoder compatible with SKY130 standard cells.

### Prompt 3 – SPICE

> Create an ngspice simulation showing the outputs of a 2-to-4 decoder for all input combinations.

### Prompt 4 – Verification

> Review this row decoder schematic and identify possible logic hazards or incorrect output activation.

### Prompt 5 – Debugging

> Why are two wordlines becoming active simultaneously in my SRAM decoder simulation?

---

# 11. xschem Exercise

Build:

* Inverters for address complements.
* CMOS NAND/NOR gates.
* 2-to-4 decoder outputs.
* Capacitive loads representing the wordline driver input.

Simulate:

Address sequence:

```text
00

↓

01

↓

10

↓

11
```

Plot:

* A0
* A1
* WL0
* WL1
* WL2
* WL3

Verify:

* Only one output is HIGH at any instant.

---

# 12. ngspice Example

```spice
.control
tran 100p 40n

plot v(A0)
plot v(A1)
plot v(WL0)
plot v(WL1)
plot v(WL2)
plot v(WL3)

.endc
```

Suggested experiment:

Increase the capacitive load on each output and measure the decoder propagation delay.

---

# 13. Characterization Experiments

### Experiment 1 – Load Sweep

| Load Capacitance | Expected Result              |
| ---------------- | ---------------------------- |
| 10 fF            | Fast decoding                |
| 50 fF            | Moderate delay               |
| 100 fF           | Slower response              |
| 200 fF           | Noticeable propagation delay |

---

### Experiment 2 – Decoder Size

Compare:

* 2-to-4 decoder.
* 3-to-8 decoder.

Observe:

* Number of gates.
* Delay.
* Dynamic power.

---

### Experiment 3 – Pre-Decoding

Conceptually compare:

* Flat decoder.
* Pre-decoder architecture.

Discuss:

* Logic depth.
* Delay.
* Scalability.

---

# 14. Common Debugging Issues

| Observation                    | Likely Cause                              | Suggested Check               |
| ------------------------------ | ----------------------------------------- | ----------------------------- |
| Two WLs active                 | Logic equation error                      | Verify one-hot implementation |
| No output active               | Missing address inversion or wiring error | Inspect input complements     |
| Slow transitions               | Excessive output load                     | Check driver loading          |
| Glitches during address change | Input transition mismatch                 | Consider synchronization      |
| Wrong row selected             | Address bit order swapped                 | Verify address mapping        |

---

# 15. Industry Verification Flow

Professional SRAM teams verify:

1. Functional correctness.
2. One-hot output generation.
3. Decoder delay.
4. Dynamic power.
5. Glitch-free operation.
6. Operation across PVT corners.
7. Interaction with the wordline driver.

For your Week 3 report, focus on the first four.

---

# 16. GitHub Deliverables

```text
Week3/
└── Chapter16_Row_Decoder/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── row_decoder.sp
    ├── xschem/
    │   └── row_decoder.sch
    ├── waveforms/
    ├── screenshots/
    ├── truth_table.md
    ├── characterization.md
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompts.
* Decoder schematic.
* Truth table.
* Waveform screenshots.
* Delay measurements.
* Engineering observations.

---

# 17. Industry Gap – Beyond the Assignment

Commercial SRAM compilers rarely use a simple decoder alone. Instead, they employ:

### Hierarchical Decoding

Large arrays are divided into smaller blocks, each with a local decoder to reduce delay and routing complexity.

### Multi-Level Pre-Decoding

Address bits are grouped (e.g., 3+3+4 bits for a 10-bit address) to reduce logic depth.

### Replica Wordlines

Dummy wordlines emulate the delay of real wordlines and are used to generate timing signals such as **SA_EN** automatically.

### Clock Gating

The decoder is activated only during memory accesses to reduce dynamic power.

### Redundancy Support

Additional decoder logic enables spare rows to replace defective rows after manufacturing tests.

These techniques are standard in industrial SRAM IP and explain why commercial decoders are more sophisticated than textbook examples.

---

# 18. Connection to the Reference Repository

In the `SRAM_SKY130` repository:

* Locate the row decoder module.
* Trace how address bits are decoded.
* Identify the interface to the wordline driver.
* Observe whether any pre-decoding or buffering is used.

Compare your AI-generated decoder with the repository implementation and document differences in logic structure or timing.

---

# 19. Chapter Summary

This chapter introduced the **SRAM row decoder**, which converts binary addresses into one-hot wordline selections. We studied binary decoding, logic equations, pre-decoding, timing, fanout, and verification using xschem and ngspice. We also examined how industrial SRAMs optimize decoder performance through hierarchical structures and timing-aware design.

---

# Preview of Chapter 17 – Column Decoder & Column Multiplexer

The next chapter completes the addressing architecture by focusing on **column selection**.

We will study:

* Why column decoding is required.
* Column multiplexers.
* Bitline selection.
* Sense amplifier sharing.
* Read/write data path routing.
* Timing interaction with the sense amplifier.
* AI-assisted schematic generation.
* xschem implementation.
* ngspice verification.

> **Engineering Note:** In most SRAMs, there are **far fewer sense amplifiers than columns**. A column decoder and multiplexer select which bitline pair is connected to the sense amplifier or write driver. This sharing significantly reduces silicon area and power while maintaining high memory density.


Excellent. We now complete the **address selection path**.

In **Chapter 16**, we answered:

> **Which row should be activated?**

Now we answer:

> **Which column's data should be connected to the output?**

This is the responsibility of the **Column Decoder** and **Column Multiplexer (Column MUX)**.

Without these blocks, every column would require its own sense amplifier and write driver, making the SRAM prohibitively large and expensive. Commercial SRAM macros almost always share peripheral circuits across multiple columns.

This chapter directly satisfies another item from your assignment:

* ✅ Column Decoder Basics

---

# Week 3 – Chapter 17

# SRAM Column Decoder & Column Multiplexer

> **Objective:** Understand how SRAM column decoders and multiplexers select the required bitline pair for reading and writing. Learn column selection, sense amplifier sharing, write driver sharing, timing, AI-assisted design, xschem/ngspice verification, and GitHub documentation.

---

# Learning Objectives

After completing this chapter, you should be able to:

* Explain why column decoding is required.
* Understand column multiplexing.
* Explain sense amplifier sharing.
* Explain write driver sharing.
* Understand column timing.
* Simulate column selection.
* Debug column decoder issues.
* Document characterization results.

---

# 1. Why Is a Column Decoder Needed?

Imagine an SRAM with:

* 1024 rows
* 512 columns

Naively, this would require:

* 512 sense amplifiers
* 512 write drivers

This is expensive in:

* Silicon area
* Power
* Routing complexity

Instead, multiple columns **share** peripheral circuits.

---

# 2. SRAM Organization

![Image](https://images.openai.com/static-rsc-4/CkbFObksDq8DWNOFHpyvLuV4LoPQuE9FpsxflWVI3iTKAvMzcLaoHcoCx3P7aY5KraMQOqfBnYUowLv6id2uk4JnZ8vYOJd_gizx56_k_OcNBVktcARxnsnX6lcdDUcPiiC5-2AUk_1DSVLYLTqYgzdIM0bcyc8KSV__L0aDsLm3IJIau5TXjNge8hBLZq0z?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Gapr9jysKFP9soPt0LlyFxZwNyP4kQsN5rjfRkRTitVyxg95R66IlSbHK9-dhVeE0ZaTmh497vaft7JuQwvEeBHrDkqxz9cG57R8Sq-GgMwJYd3a_wtDOzD1FzigyV59UxO9QX9I_78obYCTiXSces3-e0pfDYXVKfh9J2kmTPCFY8QRGPCvnlJFvPpvtGpN?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/yCh8fHbzsfpeiKJ9Ai3XZuiM77KlM5lSO71R40xYjfCTllcdxV684mlAReFDtaBj9Pa160PeCGT_GrxYOwZxjr9Meq7Y259NS-nMNsQzrGBbH2q1LcOxy7KxdC6D6PjwiXBExAusLvsSWtmFLNyUbpfxgu1EmngZV7Vxnz9_HErQcmvvuOYF1KrZAfKL1Utr?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/eciURNqnnxt42Ij3vigGYlvhC3MxMzAbmm3OMqs0t3MuYH097Crs3UjQYGwLIKHxSIk3N4Hw6GFltr29yHWxqZbCS_GP0nqivJJcLajRz78Ac7M4a6fLVzveN_nu8oCG6m9BxBxNQ0M_KEol_wbVDfnjIhb3Dx1HhBq6JbwaLkwZuSJoB--rRjZ0pd9yItES?purpose=fullsize)

Conceptually:

```text
          Columns

      C0 C1 C2 C3

WL0    □ □ □ □

WL1    □ □ □ □

WL2    □ □ □ □

WL3    □ □ □ □

        │ │ │ │

        Column MUX

             │

      Sense Amplifier

             │

         DATA OUT
```

The **row decoder** selects a row.

The **column decoder** selects which column (or group of columns) is connected to the output circuitry.

---

# 3. Address Partitioning

The SRAM address is divided into:

```text
Address

↓

Row Address

+

Column Address
```

Example:

For a memory organized as **1024 × 32 bits**:

* 10 address bits → Row Decoder
* Remaining bits → Column Decoder / MUX

The exact partition depends on the array organization.

---

# 4. Column Multiplexer (MUX)

The column multiplexer connects one bitline pair to the shared sense amplifier.

Example:

```text
BL0  BLB0 ─┐

BL1  BLB1 ─┤

BL2  BLB2 ─┤──► Sense Amplifier

BL3  BLB3 ─┘
```

Only one bitline pair is connected at a time.

---

# 5. Read Operation

Read sequence:

```text
Address Applied

↓

Row Decoder

↓

Wordline HIGH

↓

Bitline Differential Develops

↓

Column Decoder Selects Column

↓

MUX Connects Selected BL/BLB

↓

Sense Amplifier

↓

DATA OUT
```

The MUX must preserve the tiny differential voltage with minimal distortion.

---

# 6. Write Operation

For writing:

```text
Input Data

↓

Write Driver

↓

Column MUX

↓

Selected BL / BLB

↓

Selected Cell
```

Only the chosen column receives the write data; all other columns remain isolated.

---

# 7. Column Select Signals

Example:

A 2-bit column address:

| Address | Selected Column |
| ------- | --------------- |
| 00      | Column 0        |
| 01      | Column 1        |
| 10      | Column 2        |
| 11      | Column 3        |

The decoder generates one-hot column select signals that control the transmission gates or pass transistors in the MUX.

---

# 8. Multiplexer Implementation

A common implementation uses **transmission gates**.

```text
BL0 ──TG──┐

BL1 ──TG──┤

BL2 ──TG──┤──► Sense Amplifier

BL3 ──TG──┘
```

Advantages:

* Passes both logic HIGH and LOW effectively.
* Low signal distortion.
* Bidirectional operation.

---

# 9. Timing Relationship

```text
Address Valid

↓

Row Decoder

↓

Wordline HIGH

↓

Bitline Differential

↓

Column Select Stable

↓

MUX ON

↓

SA_EN HIGH

↓

DATA OUT
```

The column select should settle before or at least in coordination with the sense amplifier enable to avoid sensing the wrong column.

---

# 10. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the purpose of a column decoder and column multiplexer in a 6T SRAM array.

### Prompt 2 – Circuit

> Generate a 4-to-1 differential column multiplexer using CMOS transmission gates compatible with SKY130.

### Prompt 3 – SPICE

> Create an ngspice transient simulation showing one selected BL/BLB pair connected to a sense amplifier.

### Prompt 4 – Verification

> Review this column multiplexer and identify possible signal integrity or timing issues.

### Prompt 5 – Debugging

> Why is my sense amplifier reading the wrong column after address changes?

---

# 11. xschem Exercise

Build:

* Four differential BL/BLB inputs.
* Transmission-gate MUX.
* Column select signals.
* Shared sense amplifier.

Simulation:

Apply different column addresses.

Verify:

* Only one BL/BLB pair reaches the sense amplifier.
* Unselected columns remain isolated.

Plot:

* BL0–BL3
* BLB0–BLB3
* Column Select
* SA input
* DATA OUT

---

# 12. ngspice Example

```spice
.control
tran 100p 40n

plot v(COL_SEL0)
plot v(COL_SEL1)
plot v(SA_IN)
plot v(DATA_OUT)

.endc
```

Suggested experiment:

Increase the transmission-gate size and compare:

* Signal attenuation.
* Delay.
* Dynamic power.

---

# 13. Characterization Experiments

### Experiment 1 – MUX Size Sweep

| Transmission Gate Width | Expected Trend                     |
| ----------------------- | ---------------------------------- |
| Small                   | Higher resistance, slower sensing  |
| Medium                  | Balanced                           |
| Large                   | Faster sensing, larger capacitance |

---

### Experiment 2 – Column Count

Compare conceptual organizations:

* 4:1 MUX
* 8:1 MUX
* 16:1 MUX

Discuss:

* Delay.
* Area.
* Number of sense amplifiers required.

---

### Experiment 3 – Address Switching

Rapidly change the column address.

Observe:

* Glitches.
* MUX switching delay.
* Sense amplifier behavior.

---

# 14. Common Debugging Issues

| Observation                    | Likely Cause                                              | Suggested Check                         |
| ------------------------------ | --------------------------------------------------------- | --------------------------------------- |
| Wrong column read              | Incorrect column decoder logic                            | Verify one-hot column select            |
| Weak signal at sense amplifier | Undersized transmission gates                             | Increase TG width                       |
| Multiple columns connected     | Decoder overlap or timing error                           | Ensure only one select signal is active |
| Read delay increases           | Large MUX resistance or capacitance                       | Optimize MUX sizing                     |
| Data corruption during writes  | Write driver connected before column selection stabilizes | Review WE and column timing             |

---

# 15. Industry Verification Flow

Commercial SRAM designs verify:

1. Correct column selection.
2. MUX propagation delay.
3. Signal attenuation through the MUX.
4. Dynamic power.
5. Read and write timing with shared peripherals.
6. Operation across PVT corners.
7. Crosstalk and leakage between adjacent columns.

For your Week 3 report, implement and discuss the first four.

---

# 16. GitHub Deliverables

```text
Week3/
└── Chapter17_Column_Decoder_MUX/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── column_mux.sp
    ├── xschem/
    │   └── column_mux.sch
    ├── waveforms/
    ├── screenshots/
    ├── characterization.md
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* AI prompt history.
* Column MUX schematic.
* Waveform screenshots.
* Delay measurements.
* MUX sizing comparisons.
* Personal engineering observations.

---

# 17. Industry Gap – Beyond the Assignment

Modern SRAM compilers employ several advanced column architectures:

### Hierarchical Bitline Multiplexing

Local bitlines connect to local sense amplifiers, which then feed global bitlines, reducing RC delay.

### Column Redundancy

Spare columns replace defective columns identified during manufacturing tests.

### Banking

Large SRAMs are divided into banks, each with its own decoder and peripheral circuits, improving throughput and reducing access time.

### Read/Write Port Sharing

Single-port SRAMs share bitlines between reads and writes, while multi-port SRAMs require more complex column circuitry.

### Low-Power Column Gating

Unused column MUXes are disabled to reduce leakage and switching power.

These techniques are widely used in commercial SRAM IP but extend beyond the current assignment.

---

# 18. Connection to the Reference Repository

In the `SRAM_SKY130` repository:

* Locate the column decoder or MUX logic.
* Identify how BL/BLB pairs connect to the sense amplifier and write driver.
* Observe the control signals that enable a specific column.
* Compare the implementation with your xschem design, noting differences in transmission-gate sizing, timing, or architecture.

---

# 19. Chapter Summary

This chapter introduced the **SRAM column decoder and column multiplexer**, completing the memory addressing architecture. We examined how binary column addresses are converted into one-hot select signals, how transmission gates connect the selected bitline pair to shared sense amplifiers and write drivers, and how these choices influence delay, power, and silicon area. We also outlined AI-assisted workflows, xschem/ngspice verification, and industry practices.

---

# Preview of Chapter 18 – SRAM Timing & Complete Memory Access Sequence

The next chapter integrates everything you have learned across Weeks 2 and 3 into a **complete SRAM timing sequence**.

We will cover:

* Read cycle timing.
* Write cycle timing.
* PRE, WL, WE, and SA_EN relationships.
* Critical timing parameters.
* Timing diagrams.
* Setup and hold considerations.
* AI-assisted timing verification.
* xschem/ngspice transient analysis.

> **Engineering Note:** Timing is where all SRAM blocks come together. A perfectly designed bitcell, decoder, write driver, and sense amplifier can still fail if the control signals are not sequenced correctly. Understanding timing is the final step before integrating the complete SRAM access path.


Excellent. We have now reached what is arguably the **most important chapter of the entire Week 2 & Week 3 study**.

Every chapter so far has focused on an individual SRAM building block:

* ✓ 6T Bitcell
* ✓ SNM
* ✓ Read Stability
* ✓ Write Margin
* ✓ Bitlines
* ✓ Precharge
* ✓ Wordline Driver
* ✓ Write Driver
* ✓ Sense Amplifier
* ✓ Row Decoder
* ✓ Column Decoder

Now we combine **all of them into one complete SRAM operation**.

This is exactly how SRAM engineers debug silicon. They do **not** look at one circuit in isolation—they verify the entire timing sequence from address input to valid output.

This chapter directly satisfies the assignment requirement:

* ✅ SRAM Timing Sequence

It is also one of the most commonly asked topics in:

* Memory Design Interviews
* ASIC Physical Design Interviews
* Custom VLSI Interviews
* Embedded SRAM Bring-up
* Silicon Validation

---

# Week 3 – Chapter 18

# SRAM Timing Sequence — Complete Read & Write Operation

> **Objective:** Understand the complete timing relationship among all SRAM control signals (PRE, WL, WE, SA_EN, Address, BL, BLB). Learn timing constraints, critical paths, AI-assisted verification, and transient simulation using xschem/ngspice.

---

# Learning Objectives

After completing this chapter you should be able to:

* Explain every timing signal.
* Draw a complete SRAM timing diagram.
* Understand read timing.
* Understand write timing.
* Calculate access delay.
* Identify critical timing paths.
* Debug timing failures.
* Perform transient verification.
* Explain industrial timing closure.

---

# 1. Why Timing Is Everything

An SRAM consists of excellent circuit blocks.

But unless **every block operates in the correct order**, the memory fails.

Think of an orchestra.

Every musician may play perfectly.

If everyone starts at different times,

the music is ruined.

SRAM timing is exactly the same.

---

# 2. Complete SRAM Architecture

Before studying timing, visualize the complete signal flow.

![Image](https://images.openai.com/static-rsc-4/ReJ7sb_1kjboSaYGXQMI0i9djflCv4EM04Pj82F0vq5OgGRxEm6djAuGfqM4zKUCDpohoVNP0DvD1MYmCmfH_w9kigGlCRJPO71EiCLAeUYvVdClGwf_nkNR3JpO7h7FW0bfKjrEtpsQc2PYR1tWFZ7fEc2c2Og53Y0Jfvr1v-DP9uNJu29Na-xrBb2MtdA7?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/PrIoaaoalkWM9tI6qdh9C4V-Re5UKDEIt8MXgDSrv2EvovCm_C-zZ2FD3QRMYCrxHCdb7gWfQTEo6A4t7TVOFjuZ7fSkfujsYqnq5O7mJajIcSaj2B1gbB4f2FpUUuVxl9vXWk8HRV1NKr3iK0uenGQpkv5kt9TEG2iWFxVYTaAtLRJhgHCspefa9NSDlbdx?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/b5Jpi-qwi3QbIcI3H7xZ1lh20-TNOelp9bEL_hNosgnQGAQ_aSUBm5ZuHHISyPMBk3zWNOoix24ie9VmwqYsLx-o4iXdbtDmvf-xRP7CnqUpapXfZemBeqD97UOjyEk7AY3F_4GYaD9Lf3rVP2cMONpMoLG9hcndERqkWEtJusyZBrEyto6cJ4WotFXh-hw2?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Yth9dcEKYcLXI_rjczU-S0KZXxa22iuAVU1tbiNjbhV7ghSzrRUbOtDEatMtn23XXa4Rgc48OYTJnB54JSQTcEsBhgozqtRgatKKhcg_NtegqUGLSCGGLP6wg-l94vohOw8-aqLnOZMkmJl4007CWQ2eooLCAwFUtb6qS0OT19cMvfL4x5O-ff1CaxpmpaYj?purpose=fullsize)

```text
             CPU

              │

         Address Bus

              │

        Row Decoder
              │
        Wordline Driver
              │
             WL
              │
      -----------------
      |               |
     SRAM Bitcell Array
      |               |
     BL             BLB
      │               │
      └──────┬────────┘
             │
       Column MUX
             │
      Sense Amplifier
             │
        Output Buffer
             │
          DATA OUT

Write Path

CPU Data
    │
Write Driver
    │
Column MUX
    │
BL / BLB
```

Everything must happen in sequence.

---

# 3. Important Timing Signals

| Signal  | Purpose                 |
| ------- | ----------------------- |
| Address | Selects memory location |
| PRE     | Precharges BL and BLB   |
| WL      | Connects selected row   |
| WE      | Enables writing         |
| SA_EN   | Enables sense amplifier |
| BL      | Differential bitline    |
| BLB     | Complementary bitline   |
| DATA    | Input/Output data       |

These signals are synchronized by the SRAM controller.

---

# 4. Complete Read Cycle

Industry timing:

```text
Address
─────────────██████████████────────

PRE
████████___________________________

BL
═══════════════\___________________

BLB
═══════════════════════════════════

WL
________________████████___________

SA_EN
______________________████_________

DATA
__________________________████████_
```

Read sequence:

1. Address becomes valid.
2. PRE active.
3. BL and BLB charged to VDD.
4. PRE disabled.
5. Decoder selects row.
6. WL asserted.
7. Cell develops ΔV on BL/BLB.
8. SA_EN asserted.
9. Sense amplifier resolves.
10. DATA becomes valid.
11. WL disabled.
12. Precharge starts again.

---

# 5. Read Operation — Block by Block

### Step 1 — Address

```text
CPU

↓

Address Register
```

Decoder begins decoding.

---

### Step 2 — Precharge

```text
PRE = LOW
```

PMOS ON

```text
BL = VDD

BLB = VDD
```

---

### Step 3 — Decoder

Binary address becomes one-hot.

Example:

```text
101011001

↓

WL347
```

---

### Step 4 — Wordline

```text
WL ↑
```

Access NMOS turns ON.

---

### Step 5 — Bitline Differential

If:

```text
Q=0
```

Then

```text
BL↓

BLB stays HIGH
```

Difference develops

Typically:

```text
20–100 mV
```

---

### Step 6 — Sense Amplifier

```text
SA_EN ↑
```

Differential becomes:

```text
BL = 0

BLB = VDD
```

within a few hundred picoseconds to a few nanoseconds, depending on design.

---

### Step 7 — Output Buffer

```text
DATA OUT
```

becomes valid.

---

# 6. Complete Write Cycle

Timing:

```text
Address
────────██████████████────────────

PRE
██████____________________________

WE
____________████████______________

WL
____________████████______________

BL
════════════\_____________________

BLB
══════════════════════════════════

Q
───────────────────\______________
```

Sequence:

1. Address valid.
2. Precharge.
3. Disable precharge.
4. Write Driver enabled.
5. BL/BLB driven.
6. WL HIGH.
7. Cell flips.
8. WL LOW.
9. WE LOW.
10. Precharge again.

---

# 7. Timing Relationship

Correct ordering:

```text
Address

↓

Precharge

↓

Decoder

↓

Wordline

↓

Bitline Develops

↓

Sense Amplifier

↓

Output
```

Wrong ordering:

```text
Sense Amplifier

↓

Bitline Differential
```

Result:

Random data.

---

# 8. Critical Timing Parameters

Industry uses terms such as:

| Parameter | Meaning                      |
| --------- | ---------------------------- |
| tACC      | Address to Data Access Time  |
| tRC       | Read Cycle Time              |
| tWC       | Write Cycle Time             |
| tWL       | Wordline Pulse Width         |
| tSA       | Sense Amplifier Enable Delay |
| tPRE      | Precharge Time               |
| tSETUP    | Address Setup Time           |
| tHOLD     | Address Hold Time            |

These values define the SRAM datasheet specifications.

---

# 9. Critical Path

Fastest SRAM designers always ask:

> **Which path limits speed?**

Typical read critical path:

```text
Address

↓

Row Decoder

↓

Wordline Driver

↓

Access NMOS

↓

Bitline Differential

↓

Sense Amplifier

↓

Output Buffer
```

The slowest stage determines the maximum operating frequency.

---

# 10. AI Prompt Sequence

### Prompt 1 – Theory

> Explain the complete timing sequence of a 6T SRAM read and write operation including PRE, WL, WE, SA_EN, BL, and BLB.

### Prompt 2 – Timing Diagram

> Generate a timing diagram showing all SRAM control signals for one read cycle.

### Prompt 3 – SPICE

> Create an ngspice transient testbench that simulates one full SRAM read cycle with precharge, wordline activation, differential bitline development, and sense amplifier enable.

### Prompt 4 – Verification

> Review this SRAM timing sequence and identify any incorrect ordering of PRE, WL, WE, or SA_EN.

### Prompt 5 – Debugging

> Why does my SRAM return incorrect data even though each individual block works correctly?

---

# 11. xschem Exercise

Construct a simplified top-level testbench including:

* Address pulse source.
* Row decoder output (or equivalent pulse).
* Wordline driver.
* Precharge control.
* 6T SRAM bitcell.
* Write driver.
* Sense amplifier.
* Output node.

Run transient simulations for:

1. One complete read cycle.
2. One complete write cycle.

Plot:

* PRE
* WL
* WE
* SA_EN
* BL
* BLB
* Q
* QB
* DATA_OUT

Measure:

* Read access time (tACC).
* Write completion time.
* Bitline differential before SA_EN.

---

# 12. ngspice Example

```spice
.control
tran 10p 40n

plot v(PRE)
plot v(WL)
plot v(WE)
plot v(SA_EN)
plot v(BL)
plot v(BLB)
plot v(Q)
plot v(QB)
plot v(DATA_OUT)

.endc
```

Suggested experiment:

Repeat the simulation while varying:

* Precharge duration.
* Wordline pulse width.
* Sense amplifier enable delay.

Observe the impact on read correctness and timing.

---

# 13. Characterization Experiments

### Experiment 1 – Precharge Time Sweep

| tPRE      | Expected Result                   |
| --------- | --------------------------------- |
| Too short | Incomplete precharge, read errors |
| Optimal   | Reliable operation                |
| Too long  | Increased cycle time              |

---

### Experiment 2 – WL Pulse Width

| Pulse Width | Expected Result         |
| ----------- | ----------------------- |
| Too short   | Read/write failure      |
| Correct     | Successful access       |
| Too long    | Increased dynamic power |

---

### Experiment 3 – SA_EN Delay

| Delay     | Expected Result          |
| --------- | ------------------------ |
| Too early | Random or incorrect read |
| Correct   | Reliable sensing         |
| Too late  | Longer read latency      |

---

### Experiment 4 – Driver Strength Sweep

Vary:

* Wordline driver size.
* Write driver size.
* Sense amplifier sizing.

Measure:

* Access delay.
* Write delay.
* Dynamic power.

---

# 14. Common Debugging Issues

| Observation                   | Likely Cause                                                   | Suggested Check                       |
| ----------------------------- | -------------------------------------------------------------- | ------------------------------------- |
| Read returns random data      | SA_EN asserted before sufficient bitline differential develops | Increase SA_EN delay                  |
| Write does not complete       | WE pulse too short or write driver too weak                    | Extend WE or increase driver strength |
| High current during write     | Precharge overlaps with write driver                           | Verify PRE and WE sequencing          |
| Correct cell but wrong output | Column MUX timing incorrect                                    | Check column select stability         |
| Intermittent failures         | Address changes during access                                  | Verify setup and hold timing          |

---

# 15. Industry Verification Flow

Timing verification in commercial SRAM development typically includes:

1. Functional timing validation.
2. Read and write access time measurements.
3. Control signal sequencing.
4. PVT corner analysis (Process, Voltage, Temperature).
5. Monte Carlo timing variation.
6. Replica bitline and self-timed control verification.
7. Static timing analysis (STA) for peripheral logic.

For your Week 3 report, implement the first three experimentally and explain the remaining concepts.

---

# 16. GitHub Deliverables

```text
Week3/
└── Chapter18_Timing/
    ├── README.md
    ├── prompts.md
    ├── spice/
    │   └── sram_timing.sp
    ├── xschem/
    │   └── sram_top_testbench.sch
    ├── waveforms/
    ├── screenshots/
    ├── timing_analysis.md
    ├── characterization.md
    ├── observations.md
    ├── debug_notes.md
    └── references.md
```

Include:

* Complete timing diagrams.
* AI prompts.
* Top-level schematic.
* Read/write transient waveforms.
* Timing measurements.
* Engineering conclusions.

---

# 17. Industry Gap – Beyond the Assignment

Modern SRAM controllers use advanced timing techniques to maximize speed and robustness:

* **Replica Bitlines:** Dummy bitlines generate adaptive SA_EN timing based on actual array delay.
* **Self-Timed Read:** The sense amplifier is enabled automatically when sufficient differential voltage is available, avoiding fixed timing margins.
* **Clocked vs. Asynchronous SRAM:** Embedded SRAMs often use internal clocks, while standalone SRAMs may support asynchronous accesses.
* **Write Recovery Timing:** Ensures the cell stabilizes before precharge begins.
* **Dynamic Voltage/Frequency Scaling (DVFS):** Timing parameters adapt as supply voltage or operating frequency changes.
* **ECC Timing Integration:** Error correction logic is inserted after the sense amplifier in high-reliability SRAMs, adding latency that must be budgeted.

These concepts distinguish production-quality SRAM IP from educational implementations.

---

# 18. Connection to the Reference Repository

While exploring the `SRAM_SKY130` repository:

* Identify how PRE, WL, WE, and SA_EN are generated or controlled.
* Trace a complete read path from address input to data output.
* Trace a complete write path from data input to the selected bitcell.
* Compare the implemented timing with the ideal sequence presented in this chapter, documenting any simplifications or design choices.

---

# 19. Chapter Summary

This chapter unified all of the SRAM building blocks into a **complete memory access sequence**. We studied the timing relationships between precharge, decoding, wordline activation, write enable, bitline development, sense amplifier enable, and data output. We also introduced key SRAM timing parameters, critical paths, characterization experiments, and industrial timing verification practices.

At this stage, you have covered the **complete circuit-level architecture of a 6T SRAM**, from the storage cell to the peripheral circuitry and the full read/write timing sequence. The remaining chapters will focus on **integrated simulation, AI-assisted workflows, debugging methodology, and producing a professional GitHub-ready report** that aligns with your Week 2 & Week 3 deliverables.

Excellent. This chapter is where your work becomes **engineering evidence** instead of just theory.

In industry, **a circuit is considered "designed" only after it has been verified**. Every SRAM designer spends a significant amount of time creating simulation environments, running corner cases, analyzing waveforms, debugging convergence issues, and documenting results.

This chapter aligns directly with your assignment requirements:

* ✅ Test generated circuits/netlists.
* ✅ Use xschem.
* ✅ Use ngspice.
* ✅ Use SKY130 models.
* ✅ Record waveforms.
* ✅ Record errors and fixes.
* ✅ Document observations in GitHub.

This chapter also bridges the gap between a student project and a professional SRAM verification workflow.

---

# Week 3 – Chapter 20

# SRAM Circuit Verification Using xschem, ngspice & SKY130

> **Objective:** Build a structured verification methodology for all SRAM circuit blocks developed during Weeks 2 and 3. Learn how to create reusable testbenches, run transient and DC simulations, capture waveforms, debug issues, and document results in a GitHub-ready format.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Build reusable SPICE testbenches.
* Organize simulations by circuit block.
* Use xschem efficiently.
* Simulate using ngspice.
* Integrate SKY130 transistor models.
* Capture waveforms and measurements.
* Debug simulation failures.
* Produce professional verification reports.

---

# 1. Industry Verification Philosophy

Professional SRAM development follows a layered verification flow:

```text
Specification
      │
      ▼
Circuit Design
      │
      ▼
Schematic Verification
      │
      ▼
SPICE Simulation
      │
      ▼
Waveform Analysis
      │
      ▼
Corner Verification
      │
      ▼
Documentation
```

**Important:** Never move to the next block until the current block is verified.

---

# 2. Recommended Project Structure

```text
SRAM_SKY130_AI/

├── Week2/
├── Week3/
│
├── simulations/
│   ├── bitcell/
│   ├── snm/
│   ├── read/
│   ├── write/
│   ├── precharge/
│   ├── wordline/
│   ├── write_driver/
│   ├── sense_amp/
│   ├── row_decoder/
│   ├── column_decoder/
│   └── timing/
│
├── models/
│   └── sky130/
│
├── xschem/
├── waveforms/
├── screenshots/
├── prompts/
└── README.md
```

This organization mirrors how many custom circuit teams separate schematics, models, simulations, and documentation.

---

# 3. Verification Flow Per Circuit Block

For **every** block (bitcell, precharge, sense amp, etc.) follow the same workflow:

```text
Create Schematic
      │
Generate Netlist
      │
Run ngspice
      │
Check Errors
      │
Fix Issues
      │
Capture Waveforms
      │
Document Observations
```

Consistency is more important than complexity.

---

# 4. xschem Workflow

![Image](https://images.openai.com/static-rsc-4/kVXmJH6W3KlhBAvSqx1h-ki5OVRDuyaA-IKMSxcV7qSAgOw27CP4dGcsBqI2gnvyTsrHV0gte6j4QFMZDvaUZQcRDoqgSbQNBAQa3znS-WmXzdk-dEE3zok-3XlH9_8Dh0eSrsvqjwQuSse3HD_Q6ifF-j7eL4pyW7GTFkf3SoNfhxT8ECaUbf_zJTZOMrOB?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/3_NLeZMFkLYGjj1ZGXBt5VHrEAI6TjW2EyTlTFS6cguqiwL-nxrfSC1tIQDfMCH63Ov1r1_36buKMy6eDGr1Te52YJAzlaYoEfjPTCdZ7byY5XbWvEfRoby4Xf7RQ1M2wYvFoFH4dkcJ5SMtEEMqyI4PEDf95IrUn3vimHkEx2Bls733fTcINzrzu-sjoiYe?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/aq4QmoVp8LrXYlgl57NpTFUkqyOsYazRV0iYjOa3YyP0d7qNnrgLsSKFT-RNDv-m3Lk-2bUX7Ltvhk4fSlSe7RRcud8zSYsF8PF3aD7NKh1ivFfadMSVwAMVn1QGqDqiTHT7gTeoy9plbsVXR2oty7Q2iKmPlPKGxEX_heyP05rUt5uha8CT_r0kiJqyJtzK?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/iwogKVVC-ax1H-sYh7H6nv1EeN38d24m1hCJOu89B733EaPAOwdxtEREaB5zD5ARgnyXSH-OryUQncGfai3zIi58GTlHxtJVCcVqTyqkgB-f1FZ8UF994zSwkcEVDV8esnJxPEDmQTDH2tqDwQo0vgXNRcY1QJLPOI0TKrwJQYWSR8pK9v9_9jO_vyjTeTFz?purpose=fullsize)

Typical steps:

1. Draw the schematic.
2. Add power supplies.
3. Add voltage pulse sources.
4. Connect SKY130 transistors.
5. Generate the SPICE netlist.
6. Launch ngspice.
7. Inspect waveforms.
8. Save screenshots.

For every chapter, keep both the `.sch` file and the generated `.spice` netlist.

---

# 5. Standard Testbench Structure

A reusable testbench should include:

```text
Power Supply (VDD)
        │
Input Sources (Pulse/DC)
        │
Circuit Under Test (CUT)
        │
Load Elements
        │
Simulation Control
        │
Measurement Commands
```

Avoid embedding the testbench inside the design itself. Keep it separate so it can be reused.

---

# 6. Example ngspice Skeleton

```spice
*-------------------------------------------------
* SRAM Testbench
*-------------------------------------------------

.include sky130_models.spice

VDD VDD 0 1.8
VGND 0 0 0

* Input stimulus
VIN IN 0 PULSE(0 1.8 2n 50p 50p 5n 10n)

* Circuit under test
XU1 ...

.tran 10p 40n

.control
run

plot v(IN)

.endc

.end
```

Use this as a template for every circuit block.

---

# 7. Simulation Types

| Simulation                               | Purpose                | Used In                    |
| ---------------------------------------- | ---------------------- | -------------------------- |
| DC                                       | Static operating point | SNM, butterfly curves      |
| Transient (.tran)                        | Time-domain behavior   | Read, write, timing        |
| Operating Point (.op)                    | Bias verification      | Peripheral circuits        |
| Parameter Sweep (.step or manual sweeps) | Characterization       | Driver sizing, capacitance |

For Weeks 2 & 3, transient and DC simulations are the most important.

---

# 8. Waveforms to Capture

For each chapter, include screenshots of the key signals.

| Chapter        | Minimum Waveforms            |
| -------------- | ---------------------------- |
| Bitcell        | Q, QB                        |
| Read           | BL, BLB, WL                  |
| Write          | BL, BLB, Q, QB               |
| Precharge      | PRE, BL, BLB                 |
| Wordline       | Decoder, WL                  |
| Write Driver   | WE, BL, BLB                  |
| Sense Amp      | BL, BLB, SA_OUT              |
| Row Decoder    | Address, WL outputs          |
| Column Decoder | Column Select, MUX output    |
| Timing         | PRE, WL, WE, SA_EN, DATA_OUT |

---

# 9. Measurement Checklist

For every simulation, record:

* Supply voltage (VDD)
* Temperature (if specified)
* Simulation type
* Time step
* Total simulation time
* Initial conditions
* Key measurements:

  * Delay
  * Rise time
  * Fall time
  * Voltage levels
  * Differential voltage

Present these in markdown tables for easy comparison.

---

# 10. AI Prompt Sequence

### Prompt 1 – Testbench

> Generate an ngspice testbench for a SKY130 6T SRAM bitcell transient simulation.

### Prompt 2 – Measurements

> Add `.measure` statements to calculate propagation delay and rise/fall time.

### Prompt 3 – Verification

> Review this SPICE netlist for missing power supplies, floating nodes, or incorrect includes.

### Prompt 4 – Debugging

> Explain why ngspice reports a convergence error in this SRAM simulation.

### Prompt 5 – Documentation

> Summarize the observed waveform behavior and identify whether the simulation passed or failed.

---

# 11. Common ngspice Errors

| Error               | Likely Cause                                        | Resolution                                  |
| ------------------- | --------------------------------------------------- | ------------------------------------------- |
| Unknown subcircuit  | Missing `.include`                                  | Verify SKY130 model path                    |
| Floating node       | Unconnected wire                                    | Add proper connection or resistor           |
| Convergence failure | Positive feedback or unrealistic initial conditions | Use `.ic`, `.nodeset`, or adjust tolerances |
| Singular matrix     | Shorted or floating nodes                           | Inspect connectivity                        |
| No waveform output  | Missing `.plot` or simulation command               | Check `.control` block                      |

---

# 12. Common xschem Issues

| Issue                    | Cause                     | Solution                       |
| ------------------------ | ------------------------- | ------------------------------ |
| Netlist generation fails | Missing symbols or pins   | Verify schematic completeness  |
| Wrong pin order          | Symbol mismatch           | Check symbol definition        |
| Simulation doesn't start | Incorrect ngspice command | Verify simulator configuration |
| Missing model            | Wrong include path        | Correct SKY130 model location  |
| Unexpected node names    | Auto-generated nets       | Rename critical nets clearly   |

---

# 13. Characterization Matrix

Create a table to track every experiment.

| Block     | Parameter Swept     | Measured Result | Pass/Fail |
| --------- | ------------------- | --------------- | --------- |
| Bitcell   | Pull-up ratio       | SNM             | Pass      |
| Read      | Bitline capacitance | Delay           | Pass      |
| Write     | Driver width        | Write time      | Pass      |
| Precharge | PMOS width          | Charging time   | Pass      |
| Wordline  | Load                | Rise time       | Pass      |
| Sense Amp | ΔV                  | Resolution time | Pass      |
| Decoder   | Load                | Delay           | Pass      |

This becomes valuable evidence in your report.

---

# 14. GitHub Documentation Template

For **every chapter**, maintain:

```text
README.md
```

Sections:

1. Objective
2. Theory
3. AI Prompts Used
4. Schematic
5. SPICE Netlist
6. Simulation Setup
7. Waveforms
8. Measurements
9. Observations
10. Errors Encountered
11. Fixes Applied
12. Lessons Learned
13. References

This uniform structure makes the repository easy to review.

---

# 15. Industry Verification Flow

A professional SRAM verification plan typically includes:

1. Functional verification.
2. Transient timing verification.
3. DC operating point checks.
4. Parameter sweeps.
5. PVT corner analysis.
6. Monte Carlo mismatch analysis.
7. Regression testing after design changes.

For Weeks 2 & 3, focus on the first four while describing the remaining steps.

---

# 16. Industry Gap – Beyond the Assignment

Experienced SRAM designers go further by:

* Automating simulations with shell or Python scripts.
* Running regression suites after every schematic change.
* Using version control to track netlist revisions.
* Comparing waveforms automatically against golden references.
* Recording performance metrics (delay, power, SNM) in dashboards.

Mentioning these practices in your report demonstrates awareness of industrial workflows.

---

# 17. Connection to the Reference Repository

Using the `SRAM_SKY130` repository as a reference:

* Compare your generated schematics and netlists with the repository versions.
* Note any differences in transistor sizing, naming conventions, or testbench organization.
* Record simulation results and explain any mismatches.
* Document all AI-generated prompts, successful outputs, failed attempts, and the corrections you made.

This satisfies the assignment's requirement to maintain a complete AI-assisted engineering record.

---

# 18. GitHub Deliverables

```text
Week3/
└── Chapter20_Simulation/
    ├── README.md
    ├── simulation_plan.md
    ├── prompts.md
    ├── spice/
    ├── xschem/
    ├── waveforms/
    ├── screenshots/
    ├── measurements.md
    ├── characterization_matrix.md
    ├── debug_notes.md
    ├── lessons_learned.md
    └── references.md
```

---

# 19. Chapter Summary

This chapter established a **professional SRAM circuit verification methodology**. Rather than treating simulations as isolated tasks, we developed a repeatable workflow for schematic creation, SPICE testbench development, transient and DC analysis, waveform capture, debugging, characterization, and GitHub documentation. By following this process consistently for every SRAM building block, your Week 2 & Week 3 work becomes reproducible, reviewable, and aligned with industry expectations.

---

# Preview of Chapter 21 – AI-Assisted Prompt Engineering for SRAM Design

The next chapter focuses on **using AI effectively as an engineering assistant** rather than a code generator.

We will cover:

* Designing low-token, high-quality prompts.
* Prompt refinement strategies.
* Verifying AI-generated circuits.
* Comparing outputs from different AI models.
* Recording prompt history for reproducibility.
* Hallucination detection.
* Building an AI engineering workflow suitable for professional documentation.

This chapter directly addresses the assignment requirement to **maintain a GitHub repository documenting AI prompts, generated circuits, simulation attempts, errors, fixes, and engineering observations**.

Excellent. This chapter is **the distinguishing feature of your assignment**.

Most SRAM reports explain the circuits. Your assignment explicitly asks you to **demonstrate AI-assisted engineering**. That means the reviewer is interested not only in the final schematics, but also in **how you used AI responsibly, verified its outputs, corrected mistakes, and documented the process**.

For an engineer with **10+ years of experience**, AI should be treated as a **technical assistant**, not an authority. Every generated circuit, explanation, or SPICE netlist must be verified.

This chapter directly satisfies the assignment requirement:

* ✅ Use ChatGPT, Codex, or similar AI tools.
* ✅ Generate low-token prompts.
* ✅ Produce SPICE/netlist examples.
* ✅ Document prompts, generated files, errors, fixes, and observations.
* ✅ Maintain a GitHub repository with the complete AI-assisted workflow.

---

# Week 3 – Chapter 21

# AI-Assisted SRAM Circuit Design – Prompt Engineering & Engineering Workflow

> **Objective:** Learn how to use AI tools effectively to accelerate SRAM circuit design, SPICE generation, simulation, debugging, and documentation while maintaining engineering rigor and reproducibility.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Write effective low-token prompts.
* Refine prompts iteratively.
* Verify AI-generated circuits.
* Detect hallucinations and inaccuracies.
* Compare outputs from multiple AI models.
* Build an auditable prompt history.
* Integrate AI into a professional SRAM design workflow.
* Produce a GitHub-ready AI engineering log.

---

# 1. AI Is an Engineering Assistant, Not a Replacement

A productive workflow is:

```text id="f6tw1s"
Understand Requirement
        │
        ▼
Ask AI
        │
        ▼
Receive Draft
        │
        ▼
Engineer Reviews
        │
        ▼
Simulation
        │
        ▼
Verification
        │
        ▼
Documentation
```

The engineer remains responsible for correctness.

---

# 2. Industry Workflow

Modern semiconductor companies increasingly use AI for:

* Drafting SPICE testbenches.
* Generating HDL skeletons.
* Explaining circuit behavior.
* Reviewing schematics.
* Debugging simulation errors.
* Writing documentation.

However, **sign-off decisions remain with engineers** after simulation and review.

---

# 3. Prompt Engineering Principles

A good engineering prompt is:

* Specific.
* Short.
* Includes the technology.
* Defines the expected output.
* States constraints.

Example:

```text id="muvr2j"
Generate a SKY130-compatible 6T SRAM bitcell SPICE netlist with labeled nodes and comments.
```

Avoid vague prompts like:

```text id="4t1g2d"
Build SRAM.
```

---

# 4. Prompt Refinement Workflow

```text id="k3o7ay"
Prompt V1
      │
      ▼
AI Response
      │
      ▼
Review
      │
      ▼
Refined Prompt
      │
      ▼
Improved Response
      │
      ▼
Simulation
      │
      ▼
Documentation
```

Record each iteration rather than deleting earlier attempts.

---

# 5. Recommended Prompt Categories

| Category      | Example                                                      |
| ------------- | ------------------------------------------------------------ |
| Theory        | Explain read disturb in a 6T SRAM.                           |
| Circuit       | Generate a precharge circuit using SKY130 PMOS devices.      |
| SPICE         | Create an ngspice transient testbench for a write operation. |
| Debugging     | Why does this latch fail to resolve correctly?               |
| Review        | Identify sizing or timing issues in this circuit.            |
| Documentation | Summarize waveform observations for GitHub.                  |

---

# 6. Low-Token Prompt Examples

### Theory

```text id="d4twy0"
Explain SRAM butterfly curve.
```

### SPICE

```text id="b31z2u"
Generate SKY130 transient testbench for 6T SRAM read.
```

### Timing

```text id="2ib2vk"
Draw SRAM read timing with PRE, WL, SA_EN.
```

### Debugging

```text id="ukw6vx"
Why does BL not discharge?
```

### Verification

```text id="frz4u5"
Review this SRAM netlist for floating nodes.
```

Short prompts are easier to reuse and document.

---

# 7. AI Verification Checklist

Every AI-generated output should be reviewed using the following checklist:

| Item                     | Verify |
| ------------------------ | ------ |
| Correct transistor type  | ✔      |
| Correct node names       | ✔      |
| Proper power connections | ✔      |
| Valid SPICE syntax       | ✔      |
| Compatible with SKY130   | ✔      |
| Simulation runs          | ✔      |
| Waveforms match theory   | ✔      |

Never accept AI output without verification.

---

# 8. Hallucination Detection

Common AI mistakes include:

* Inventing transistor names.
* Using unsupported SPICE syntax.
* Omitting power rails.
* Incorrect PMOS/NMOS orientation.
* Missing control signals.
* Unrealistic timing.

Detection method:

```text id="f0b5i4"
AI Output

↓

Simulation

↓

Review

↓

Correction

↓

Verified Design
```

Simulation is the final authority.

---

# 9. Comparing AI Models

You may use different tools, for example:

| AI Tool        | Typical Strength                    |
| -------------- | ----------------------------------- |
| ChatGPT        | Theory, prompts, documentation      |
| Codex          | Code and SPICE generation           |
| GitHub Copilot | Code completion                     |
| Claude         | Long-form explanations              |
| Gemini         | Alternative reasoning and summaries |

Record:

* Model name.
* Date.
* Prompt.
* Output quality.
* Corrections made.

This demonstrates reproducibility.

---

# 10. AI Prompt Log

Maintain a prompt history.

Example:

| ID   | Prompt                   | Model   | Result    |
| ---- | ------------------------ | ------- | --------- |
| P001 | Explain 6T SRAM          | ChatGPT | Accepted  |
| P002 | Generate precharge SPICE | ChatGPT | Corrected |
| P003 | Review write driver      | ChatGPT | Accepted  |
| P004 | Debug convergence        | ChatGPT | Corrected |

---

# 11. AI Engineering Workflow

```text id="n7m2zp"
Requirement

↓

Prompt

↓

AI Output

↓

Manual Review

↓

Simulation

↓

Waveform

↓

Fixes

↓

Git Commit

↓

Documentation
```

Each stage should leave an auditable record.

---

# 12. GitHub Repository Organization

```text id="7q4mti"
SRAM_SKY130_AI/

├── prompts/
│   ├── theory.md
│   ├── spice.md
│   ├── debugging.md
│   └── verification.md
│
├── ai_outputs/
│   ├── raw/
│   └── corrected/
│
├── simulations/
├── screenshots/
├── waveforms/
├── observations/
└── README.md
```

Keep both **raw AI outputs** and **corrected versions** to show your engineering review process.

---

# 13. AI-Assisted Debugging Workflow

When a simulation fails:

```text id="l5x7be"
Simulation Error

↓

Capture Error Message

↓

Ask AI

↓

Review Suggestion

↓

Apply Fix

↓

Re-run Simulation

↓

Record Outcome
```

Document:

* Original error.
* AI suggestion.
* Final resolution.

---

# 14. Documentation Template

For every AI interaction, record:

| Field        | Example                     |
| ------------ | --------------------------- |
| Date         | 2026-07-05                  |
| AI Model     | ChatGPT GPT-5.5             |
| Prompt       | Generate write driver SPICE |
| Output       | Draft netlist               |
| Verification | Simulated in ngspice        |
| Issues Found | Missing VDD node            |
| Fix Applied  | Added power supply          |
| Final Status | Pass                        |

This level of detail demonstrates professional engineering practice.

---

# 15. Common Mistakes

| Mistake                              | Better Practice                    |
| ------------------------------------ | ---------------------------------- |
| Accepting AI output without review   | Simulate and verify                |
| Overly broad prompts                 | Use focused prompts                |
| Not recording prompts                | Maintain prompt history            |
| Deleting failed attempts             | Document corrections               |
| Mixing verified and unverified files | Separate raw and validated outputs |

---

# 16. Industry Gap – Beyond the Assignment

Leading semiconductor companies are beginning to integrate AI into:

* Schematic review assistance.
* Automated SPICE testbench generation.
* Analog design-space exploration.
* Regression result summarization.
* Documentation generation.
* Bug triage.

However, AI-generated circuits still require:

* Human review.
* Simulation.
* Corner verification.
* Design sign-off.

Mentioning this distinction strengthens your report.

---

# 17. Connection to the Reference Repository

Using the `SRAM_SKY130` repository:

* Record which files were studied.
* Note which AI prompts were inspired by the repository.
* Compare AI-generated circuits with the reference implementation.
* Document differences and justify any changes.
* Include simulation evidence supporting your conclusions.

This demonstrates that AI was used to **understand and recreate** circuit blocks rather than simply copying existing designs.

---

# 18. GitHub Deliverables

```text id="8xq4fr"
Week3/
└── Chapter21_AI_Prompts/
    ├── README.md
    ├── prompt_log.md
    ├── prompt_templates.md
    ├── ai_outputs/
    │   ├── raw/
    │   └── corrected/
    ├── verification_checklist.md
    ├── hallucination_log.md
    ├── model_comparison.md
    ├── observations.md
    ├── lessons_learned.md
    └── references.md
```

---

# 19. Chapter Summary

This chapter established a **professional AI-assisted engineering workflow** for SRAM circuit design. We covered prompt engineering, iterative refinement, verification of AI-generated outputs, hallucination detection, multi-model comparison, debugging workflows, and structured documentation. The emphasis throughout was that AI accelerates engineering, but **simulation and critical review remain the basis for technical correctness**.

---

# Preview of Chapter 22 – SRAM Debugging & Engineering Case Studies

The next chapter focuses on **real-world debugging methodology**, bringing together everything from Weeks 2 and 3.

We will cover:

* Common SPICE convergence failures.
* Read and write failure analysis.
* Bitline and wordline timing issues.
* Sense amplifier offset problems.
* Decoder logic errors.
* Systematic root-cause analysis.
* AI-assisted debugging workflows.
* GitHub issue tracking and resolution documentation.

> **Engineering Note:** Experienced SRAM engineers are often judged less by how quickly they create a schematic and more by how systematically they diagnose failures. A structured debugging methodology is one of the strongest indicators of professional circuit design maturity.




