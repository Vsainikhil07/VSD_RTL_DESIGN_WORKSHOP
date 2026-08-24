# Module 0 — Workshop Introduction

> **Part of the RTL Design Workshop series.**

---

## Overview

Module 0 introduces the RTL Design Workshop environment and prepares the required tools before starting the RTL design exercises. It covers the workshop structure, available lab environment, local tool installation, and verification of the RTL simulation and synthesis toolchain.

### Environment & Tool Summary

| Toolchain | Lab Environment | System Requirements |
| :--- | :--- | :--- |
| **Icarus Verilog**, **GTKWave**, **Yosys** | Cloud Lab / Ubuntu Linux | Linux-based environment (Native or VM) |

---

## Table of Contents

- [1. Workshop Introduction](#1-workshop-introduction)
  - [1.1 Workshop Structure](#11-workshop-structure)
  - [1.2 Cloud Lab](#12-cloud-lab)
- [2. Local Tool Setup](#2-local-tool-setup)
  - [2.1 System Requirements](#21-system-requirements)
  - [2.2 Installing Icarus Verilog and GTKWave](#22-installing-icarus-verilog-and-gtkwave)
  - [2.3 Installing Yosys & Repository Setup](#23-installing-yosys--repository-setup)
  - [2.4 Checking the Installation](#24-checking-the-installation)
- [3. Takeaways](#3-takeaways)

---

## 1. Workshop Introduction

# Module 0 — Workshop Introduction

> **Part of the RTL Design Workshop series.**

---

## Overview

Module 0 introduces the RTL Design Workshop environment and prepares the required tools before starting the RTL design exercises. It covers the workshop structure, available lab environment, local tool installation, and verification of the RTL simulation and synthesis toolchain.

### Environment & Tool Summary

| Toolchain                                  | Lab Environment          | System Requirements                    |
| :----------------------------------------- | :----------------------- | :------------------------------------- |
| **Icarus Verilog**, **GTKWave**, **Yosys** | Cloud Lab / Ubuntu Linux | Linux-based environment (Native or VM) |

---

## Table of Contents

* [1. Workshop Introduction](#1-workshop-introduction)

  * [1.1 Workshop Structure](#11-workshop-structure)
  * [1.2 Cloud Lab](#12-cloud-lab)
* [2. Local Tool Setup](#2-local-tool-setup)

  * [2.1 System Requirements](#21-system-requirements)
  * [2.2 Installing Icarus Verilog and GTKWave](#22-installing-icarus-verilog-and-gtkwave)
  * [2.3 Installing Yosys & Repository Setup](#23-installing-yosys--repository-setup)
  * [2.4 Checking the Installation](#24-checking-the-installation)
* [3. Takeaways](#3-takeaways)

---

## 1. Workshop Introduction

### 1.1 Workshop Structure

The RTL Design Workshop is organized into multiple modules that gradually introduce the RTL-to-synthesis flow. The modules combine theoretical concepts with practical exercises, progressing from RTL coding and simulation towards synthesis, timing concepts, standard-cell libraries, and sequential logic design.

Module 0 focuses mainly on understanding the workshop environment and preparing the tools required for upcoming modules.

### 1.2 Cloud Lab

A pre-configured cloud-based laboratory environment can be used to perform the workshop exercises without setting up all the tools locally.

The cloud environment provides the required software and can be accessed through a web browser after signing in with the provided credentials.

---

## 2. Local Tool Setup

### 2.1 System Requirements

The local setup requires a Linux-based environment such as **Ubuntu**. Ubuntu may be installed directly on hardware or run through a virtual machine such as **Oracle VM VirtualBox**.
<img width="1918" height="1003" alt="image" src="https://github.com/user-attachments/assets/e314a3aa-58f9-498e-961f-84349db1b722" />

### 2.2 Installing Icarus Verilog and GTKWave

* **Icarus Verilog (`iverilog`)**: Used for compiling and simulating Verilog designs.
* **GTKWave**: Used to view and analyze simulation waveform traces (`.vcd` files).

Run the following command in the terminal:

```bash
sudo apt install iverilog
 sudo apt install  gtkwave
```
<img width="711" height="470" alt="image" src="https://github.com/user-attachments/assets/c9e359b4-677d-45f5-b42f-48659b9834f6" />


### 2.3 Installing Yosys & Repository Setup

**Yosys** is an open-source framework for Verilog RTL synthesis used to convert RTL designs into gate-level netlists.

Install Yosys using the following command:

```bash
sudo apt install  yosys
```

Clone the workshop exercise repository:

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```

### 2.4 Checking the Installation

Verify that all tools are correctly installed and accessible in the system path by executing the following version-check commands:

```bash
iverilog -V
gtkwave --version
yosys -V
```

These commands confirm that Icarus Verilog, GTKWave, and Yosys are installed and accessible from the terminal.

---

## 3. Takeaways

* [x] Understood the overall structure and goals of the RTL Design Workshop.
* [x] Learned about the cloud-based lab environment workflow.
* [x] Configured local Linux prerequisites.
* [x] Installed Icarus Verilog, GTKWave, and Yosys.
* [x] Verified tool versions and repository clone setup prior to RTL design labs.
