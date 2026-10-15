# Module 4 — Backend Implementation and Static Timing Analysis

## Project Overview

This module documents a backend RTL-to-GDSII implementation study using the **PicoRV32A RISC-V core**, **OpenLane**, **OpenROAD**, and the **SKY130 open-source PDK**. It follows the design from RTL and synthesis through placement, clock-tree construction, routing, parasitic-aware timing analysis, and physical verification.

The focus is on understanding how physical implementation affects timing—not just whether the RTL is logically correct.

## Objectives

- Run an end-to-end RTL-to-GDSII implementation flow.
- Review synthesis and standard-cell technology mapping.
- Interpret cell statistics, area, placement, and legality reports.
- Understand Clock Tree Synthesis (CTS), clock buffers, insertion delay, and skew.
- Read setup- and hold-timing reports.
- Compare ideal-clock analysis with propagated-clock analysis.
- Examine interconnect RC effects and crosstalk-driven delay.
- Understand common methods used to achieve timing closure.

## Tools and Technologies

| Tool / Technology | Purpose |
| --- | --- |
| OpenLane | Automated RTL-to-GDSII flow |
| OpenROAD | Physical-design implementation and static timing analysis |
| Yosys | RTL synthesis and logic optimization |
| SKY130 PDK | Process design kit and standard-cell libraries |
| Magic | Layout inspection and physical verification |
| PicoRV32A | Reference RISC-V processor core |
| Linux shell | Running commands and managing the flow |
| Tcl | Flow and design configuration |
| Git and GitHub | Version control and project documentation |

## Implementation Flow

```text
RTL Capture
   ↓
OpenLane Setup
   ↓
Synthesis and Technology Mapping
   ↓
Floorplanning
   ↓
Placement and Legality Checks
   ↓
Clock Tree Synthesis (CTS)
   ↓
Routing and Parasitic Extraction
   ↓
Static Timing Analysis
   ↓
Setup / Hold Verification
   ↓
Physical Sign-off Checks
```

## Results and Screenshot Walkthrough

The 22 screenshots are placed throughout this README, directly below the subsection that explains what each image shows. Keep them in these positions rather than collecting all images at the end of the document.

### Screenshot Placement Guide

| Screenshot numbers | Place under this section | What the screenshots cover |
| --- | --- | --- |
| 1–3 | **1. Physical Layers and Layout Inspection** | Routing-layer coordinates, Magic layout editor, and standard-cell geometry |
| 4–8 | **2. Flow Setup and Synthesis** | OpenLane configuration and startup, synthesis/flip-flop mapping, and mapped-cell statistics |
| 9–15 | **3. Clock Distribution, Layout, and Timing** | STA slack report, clock-tree construction, chip layout, placed-cell wiring, CTS, clock buffers, and crosstalk |
| 16–20 | **4. Placement Quality and Hold Analysis** | Propagated-clock hold timing, flip-flop hold timing, real-clock timing paths, OpenROAD clock report, and post-CTS skew |
| 21–22 | **5. Ideal Clock and Setup/Hold Concepts** | Ideal-clock timing analysis and setup/hold timing concepts |

**How to place the images:** Put each screenshot immediately after the explanatory paragraph in its numbered subsection. Keep the existing caption/alt text with the image. If you upload the screenshots to GitHub separately, use stable relative image paths in place of the current image URLs; the URLs copied from the original document may expire.

### 1. Physical Layers and Layout Inspection

#### 1.1 Routing-layer coordinate data

The coordinate view shows physical geometry associated with the SKY130 routing stack, including `li1`, `met1`, `met2`, `met3`, `met4`, and `met5`. These coordinates help relate logical connectivity to physical locations and routing dimensions.

![Routing-Layer Coordinates](https://private-user-images.githubusercontent.com/313950167/658716705-460b53d1-16d5-4a7f-824b-5758aad39e4e.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTYsIm5iZiI6MTc5MjA2NzU1NiwicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzE2NzA1LTQ2MGI1M2QxLTE2ZDUtNGE3Zi04MjRiLTU3NThhYWQzOWU0ZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1mOTQ1ZWQzMzYzNjQ2NTQzOGExNzA4Y2FhOTJhNWYyN2NkY2Y3YWYxNGQ4M2VhNjY4YjdmODI5Nzg2ZDg5M2ZmJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.kS6G4_drHVVxTzkmCzzSAoV2ddA8ljdVmR8X8s63_-o)

#### 1.2 Magic layout editor

The Magic session displays process layers such as diffusion, polysilicon, contacts, and metal, together with the layout grid. It is useful for inspecting geometry and investigating physical-design rule issues.

![Magic Layout Editor](https://private-user-images.githubusercontent.com/313950167/658718248-e5af02df-e4c8-4eeb-88ed-fdaaa12585bf.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTYsIm5iZiI6MTc5MjA2NzU1NiwicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzE4MjQ4LWU1YWYwMmRmLWU0YzgtNGVlYi04OGVkLWZkYWFhMTI1ODViZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02NzU5MzQ3YjI1ODcyM2EzNzA0NDY4MzQwYTI1YzgwMGUyNWFmZWNiZDgxY2QzOTQyZTQ4YWRiOTNkOTBiMWM0JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.Iwe4J4LOpvhQVlxQez8GK4AyjgHA1FIByURN0DOAvQA)

#### 1.3 Standard-cell geometry close-up

This view highlights the physical structures inside a standard cell, including interconnect, diffusion regions, polysilicon gates, contacts, transistor structures, and power connections.

![Zoomed Standard-Cell Geometry](https://private-user-images.githubusercontent.com/313950167/658722710-7e80e3b9-9f55-4630-ba6a-898a4e29c2df.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzIyNzEwLTdlODBlM2I5LTlmNTUtNDYzMC1iYTZhLTg5OGE0ZTI5YzJkZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1jNjI3M2JmNWNiMzhkZDE4NGRiNzYzYTBiZjFjYmY0YmJlMGI2MTQ4ODNlODFlNGYzMTEwZDZlNTQ0NjRiYTZiJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.JXcMZtKM4lNvCpMj4d7wj3dXLQ9PVxulheVpfzg-IIA)

### 2. Flow Setup and Synthesis

#### 2.1 OpenLane design configuration

The configuration screenshot shows the reference design settings, including the design name, clock port, target clock period, RTL source, constraints, and library selection. The source configuration uses `picorv32a` and a `4.500 ns` clock period.

![OpenLane Run Configuration](https://private-user-images.githubusercontent.com/313950167/658723298-32023345-9ca1-4fdf-9006-05a9ea94c2eb.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzIzMjk4LTMyMDIzMzQ1LTljYTEtNGZkZi05MDA2LTA1YTllYTk0YzJlYi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1lNTRiZjFiZWJjMTkyN2I4NDQ0ZDdhMzM4ZGIxNjVmMmFhZjAzZjJmM2I1YjliOWZiOTY2MTNkOTZiM2Q0NzVhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.IqIta5rA7P-JIvFdFQe92H9oThIEFdsGNxEb_F6Ay7c)

#### 2.2 Starting the OpenLane environment

The terminal view records the OpenLane startup and version information. OpenLane coordinates the main implementation stages through a shell-driven flow.

![Launching OpenLane](https://private-user-images.githubusercontent.com/313950167/658725912-243b5fe3-254d-4398-9045-055a600964c4.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzI1OTEyLTI0M2I1ZmUzLTI1NGQtNDM5OC05MDQ1LTA1NWE2MDA5NjRjNC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0wMGJiYjcyNmE3YWEyODBjNWQ5MmMwNWRkYzc1NTc4NTZjM2E2NzVlMTFiY2E5YWYyYzNiNzg2ZWRmNDQ1NjY3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.wGZjwM9ftDFbbAjzoYnmheJm-DD7bgO_nEU-_VFaKRU)

#### 2.3 Synthesis and flip-flop mapping

The synthesis output shows generic sequential logic being mapped to SKY130 standard cells. The source report includes the line `mapped 1572 $DFF_P cells`.

![Synthesis Output and Flip-Flop Mapping](https://private-user-images.githubusercontent.com/313950167/658726949-3b197491-fac0-4715-8cb5-65d3c180e247.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzI2OTQ5LTNiMTk3NDkxLWZhYzAtNDcxNS04Y2I1LTY1ZDNjMTgwZTI0Ny5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT00OTRhZjMyMDZkMWE2YzA1ZjQyNjYxNjk2OTRlM2U3MzRiYzMzMDlkYjA4ZWYxNzhjNWY3MGY2ZGQ4MzEzMjdjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.M3yuvLReetDXuHEUg00DfgNciFVu2xPOmbTiOinWnkQ)

#### 2.4 Cell statistics after technology mapping

The mapped-cell report includes combinational and sequential cell categories, wire counts, and an estimated area for the `picorv32a` module. These statistics provide an initial view of design complexity and cell-library usage.

![Cell-Type Breakdown After Mapping](https://private-user-images.githubusercontent.com/313950167/658728007-46817632-ed8f-4afa-bb70-91b5339b48a8.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzI4MDA3LTQ2ODE3NjMyLWVkOGYtNGFmYS1iYjcwLTkxYjUzMzliNDhhOC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xYjRjZDA4ZmU3MzU4OWU3NzUxZjAwYTVmMWYzNjEwY2YwNGUyNzAyZTI2YTE0ZjZlN2Q4NGMwNWI1MTE4YTE5JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.jdIHJDRgm1rqy6c5XiforxY-6nsY4XPWsZ5Rbfb34mk)

### 3. Timing Analysis and Clock Distribution

#### 3.1 Timing report with a slack violation

The report includes arrival time, required time, clock-path delay, setup margin, and slack. A negative slack value means that the reported path misses its timing requirement under the evaluated conditions.

![STA Report Showing a Slack Violation](https://private-user-images.githubusercontent.com/313950167/658729055-37259769-d279-4c0c-a5a7-fc246cd2e457.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzI5MDU1LTM3MjU5NzY5LWQyNzktNGMwYy1hNWE3LWZjMjQ2Y2QyZTQ1Ny5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02ZWUzNGIxMDUyZjE0NDM3ZWZiMjBkODc5MzVmZGY5ODg0ODFiOWJiZTAwMmNjMTJiZjVjZTcyOTE1NTA1M2I4JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.iK67NObKN3WJ6NJjKr3bmXhrd0de_kvncD6pOEem3S0)

Common approaches to investigate timing violations include cell sizing, buffer insertion or resizing, placement refinement, clock-tree adjustment, and critical-net routing improvements.

#### 3.2 Power-conscious clock-tree construction

Clock-tree design must balance timing and power. Buffer delay depends on factors such as input transition (slew) and output capacitive load.

![Power-Conscious Clock Tree Construction](https://private-user-images.githubusercontent.com/313950167/658730675-d4482e5e-4055-4ea9-a9bd-04acde0d2267.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4NzMwNjc1LWQ0NDgyZTVlLTQwNTUtNGVhOS1hOWJkLTA0YWNkZTBkMjI2Ny5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04N2VhMzQyMTA1Nzg3NTM0NGU0MDZiY2Q4OWM0ZDU0YWQ0NzY5OWEyNzY2YTlmNzYzY2FiMGMyZDhkMTI3MWI1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.nXUQ1s3JZeXAhkHNH1g4HgvZSyupV29Pc7aTVnHR9NU)

#### 3.3 Full-chip physical layout

The full-chip view illustrates the physical realization of the design after implementation, with placed standard cells and their associated routing.

![Full-Chip Physical Layout](https://private-user-images.githubusercontent.com/313950167/658797779-22ac93c7-d9b0-4021-9123-4d2e79289693.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4Nzk3Nzc5LTIyYWM5M2M3LWQ5YjAtNDAyMS05MTIzLTRkMmU3OTI4OTY5My5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hYjA5ZGYwNWEzMzJlNDU1YTYwZjdiZTdiZDc1ZWZmMWRlZjI3ZGM1M2EwZDI5NzdjODFmMjQ2NjYzMThkY2ZjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.viRlSTG7MzetxqdrRz79gGBCpjbwOt96IPKmpEpZRc4)

#### 3.4 Placed-cell and wiring close-up

The zoomed view shows individual standard cells and their interconnections, including combinational logic, sequential cells, and clock or power structures.

![Placed Cells and Wiring Close-Up](https://private-user-images.githubusercontent.com/313950167/658798946-dc90b866-5792-4171-a058-13b847cbfdc9.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4Nzk4OTQ2LWRjOTBiODY2LTU3OTItNDE3MS1hMDU4LTEzYjg0N2NiZmRjOS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kYTkyMmVmYWZkNzMzNDg4ZDgzODcxNTgwMDQ3NmViMDBkM2E0ZjVmYTdlNmY1YTY1NmYxZDFlYzM4OThhYjAwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.YjG6vHPXlq8nfd9KrPREK3sVPWURIB7E1Hs7vteWVOs)

#### 3.5 Clock Tree Synthesis (CTS)

CTS distributes the clock from its source to sequential elements through a branching network. A simplified skew relationship is:

\[
\text{Skew} = t_B - t_A
\]

The objective is to keep clock-arrival differences controlled while meeting insertion-delay, slew, and fan-out requirements.

![Clock Tree Synthesis Concept](https://private-user-images.githubusercontent.com/313950167/658800685-2cac79fc-cf24-40cd-b248-4f27b50fe6ea.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODAwNjg1LTJjYWM3OWZjLWNmMjQtNDBjZC1iMjQ4LTRmMjdiNTBmZTZlYS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hYTgwNmJjNjM0ZmRkMDg0YTUzZWU4YzYzZmUwNTM0NTQyODYyZDQ5NmY3MDlhMDYwMTQ0NjY3NTE4NDJjOGMxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.57itXkyUnpvoXjFn6ZPtAKdUbURPIxwN2kI6xyIDbls)

#### 3.6 Buffer insertion in the clock network

Clock buffers create branches and provide sufficient drive for the connected loads. Their sizing and placement influence edge rates, propagation delay, fan-out, and clock-arrival balance.

![Clock Buffer Insertion](https://private-user-images.githubusercontent.com/313950167/658801677-0c87a996-26f9-49af-aabf-30b0a5061703.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODAxNjc3LTBjODdhOTk2LTI2ZjktNDlhZi1hYWJmLTMwYjBhNTA2MTcwMy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kOWUxZjVkYmJjN2VlN2I3MDMwZmY5YzRmN2IxYjk0ZTc0ZGI5Njc2YmY5MzBkMWI1YjNmM2Q1ZmYxNTk2ODI5JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.0GBSUe_i9ehaTFNDUAAjoVcJA3LxRF-oFTbUTJYbcm8)

#### 3.7 Crosstalk effects on delay and skew

Coupling between neighboring wires can change signal delay. The source illustration compares delay without aggressor coupling (`D`) with delay including a coupling-related change (`D + Δ`). Such effects can influence timing and skew.

![Crosstalk Impact on Delay and Skew](https://private-user-images.githubusercontent.com/313950167/658803176-80edcfd8-56d2-4c2d-a11a-c7a09699a831.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODAzMTc2LTgwZWRjZmQ4LTU2ZDItNGMyZC1hMTFhLWM3YTA5Njk5YTgzMS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02MmMwZmNlZmEyNWJlMGUxMzg1ZmVjNzRhNDZhZGMwMGMwNTIyNWQ2NGI0YTMzYjEyNGYyNjJiZWM2N2JmZmNkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.VAnM3stOo8fHgob6tBBrY4MQl4xeLxvz3m_dBtxHq9Y)

### 4. Placement Quality and Hold Analysis

#### 4.1 Hold timing with propagated clocks

This report considers a path from a launching register through combinational logic to a capturing register. Propagated-clock analysis includes clock-network delay and the relevant hold requirements.

![Hold Timing with Propagated Clocks](https://private-user-images.githubusercontent.com/313950167/658805515-4bcff447-4c39-4391-8f53-b669070a3f8f.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODA1NTE1LTRiY2ZmNDQ3LTRjMzktNDM5MS04ZjUzLWI2NjkwNzBhM2Y4Zi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zZmY3MWUyZTA0OWJiNzdlMzdmMDQ5ZTI0MGJmNjIwODViMDljMTQzYjU0MGMwZDA0MDE3ZTkwOTY4NzRmNDhkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.TpgV0T0YVfX0JBhCrovWh-hRDIeS3E7VP850lov7SW0)

The source notes the hold relationship:

\[
\theta + \Delta_1 > H + \Delta_2 + HU
\]

It also lists an example clock frequency of `1.2 GHz` and a clock period of `0.83 ns`.

#### 4.2 Hold timing inside a flip-flop

The internal flip-flop illustration uses two mux stages (`MuxA` and `MuxB`) to explain data movement through the master-slave structure. Internal propagation time contributes to the timing requirement.

![Internal Flip-Flop Hold Timing](https://private-user-images.githubusercontent.com/313950167/658806863-31741c35-0945-405f-98c7-be58449410de.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODA2ODYzLTMxNzQxYzM1LTA5NDUtNDA1Zi05OGM3LWJlNTg0NDk0MTBkZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02MjE1Y2NiMTFjMzM5MmQ5ZjdmM2UxNDBhZDVhZDRhODdhMDg4YmJhYTczYmI1ZDRkMDA1ZDBmMzU2Mjk0ODk1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.BRrw3ATUPkYx4B1mXTRPV2Dh6LUmCUjXWQGaO_jak4M)

#### 4.3 Timing paths under real-clock conditions

This view illustrates timing paths involving primary inputs, registers, combinational logic, clock buffers, and primary outputs. Total path delay includes both cell delay and interconnect delay.

![Timing Paths Under Real-Clock Conditions](https://private-user-images.githubusercontent.com/313950167/658807667-2cd14f4e-0b57-4b5c-b01e-89c00453c381.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODA3NjY3LTJjZDE0ZjRlLTBiNTctNGI1Yy1iMDFlLTg5YzAwNDUzYzM4MS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT01ZTFiNzczOWNjMDAwNjM0NzNkYjQ0NWZlOTlmZGQ1NzcxYWY2NWMxOTA3Y2VkNDIzZTRlOTVjOWZkMGI3Yjg3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.7QyaHyM5FYroAL0OXVTo1h6VmxOu5FR01DYCz0WyohM)

#### 4.4 OpenROAD clock propagation report

The report includes clock-source information, network insertion delay, buffer-stage delays, clock reconvergence pessimism removal (CRPR), required time, arrival time, and slack. The shown report records a slack violation for the evaluated path.

![OpenROAD Clock Propagation Report](https://private-user-images.githubusercontent.com/313950167/658808921-e22eee41-c9be-4cfc-96f3-29fff19fd38f.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODA4OTIxLWUyMmVlZTQxLWM5YmUtNGNmYy05NmYzLTI5ZmZmMTlmZDM4Zi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zM2I5ZWNiYmE0NTRkNGM3NzY0N2FmMjA0N2VkNjc3N2YzMmU3ODliZjIxNTlmNGZhMWIxOTJmYjFkMjQ5MTliJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.DcsZl5hQMKVsPVhjo-VoFrIbvBCVktliscRxiFKAYSg)

#### 4.5 Post-CTS skew and buffer report

The post-CTS report shows a path with slack marked `MET` and includes clock-skew reporting commands and a clock-buffer selection list. The screenshot reports a skew value of `0.19` in the displayed units.

![Post-CTS Skew Report and Buffer List](https://private-user-images.githubusercontent.com/313950167/658810111-16704675-c56f-44cc-965f-731aa87b316c.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODEwMTExLTE2NzA0Njc1LWM1NmYtNDRjYy05NjVmLTczMWFhODdiMzE2Yy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xYjNhMjMwNjZhMjUyYjU5ZGVmZGE1YWMyZDQ5MTE2MzM3NmJlMjExY2MwZDgzNGRjMjY1YTI2MDllN2RlMThjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.Uwkedkd1OhzHMGXYskghxeOSNfr9C0JieILYueHPcBM)

#### 4.6 Placement after configuration changes

The second placement report provides cell, net, area, utilization, row-count, HPWL, and displacement metrics. The displayed legality checks include row, site, power, edge, placement, and overlap checks, all marked `PASS`.

![Placement Snapshot After Re-Configuration](https://private-user-images.githubusercontent.com/313950167/658811260-d0ceb389-a03e-460f-b00a-3a760e97453d.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODExMjYwLWQwY2ViMzg5LWEwM2UtNDYwZi1iMDBhLTNhNzYwZTk3NDUzZC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zODgwYWU0NGRlNjIyZjBmZjNkMThkOTZjZGI5YjAwZmI0ODQzYWJhYmRjYzY2MTNjZTk2YWFkN2RiMTBmNWNjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.3_g4HQDrI4ks1gzPwaCutL4hTBG3tTeQMosn3evGu5A)

### 5. Ideal Clock, Propagated Clock, and Timing Definitions

#### 5.1 Timing with ideal-clock assumptions

Ideal-clock analysis uses a simplified clock model before the implemented clock network is fully represented. It helps separate data-path contributions from clock-network effects.

![Ideal-Clock Timing Evaluation](https://private-user-images.githubusercontent.com/313950167/658812932-70752c94-829b-49e9-8b84-1886856cc095.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODEyOTMyLTcwNzUyYzk0LTgyOWItNDllOS04Yjg0LTE4ODY4NTZjYzA5NS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hNTVhODA2YmE2ZTY1ZDJhYzI4YWE3ZTM3ODkyY2E5YmZkYjQ5MGQwNDhiZTA0ZmFmMGM1NTJhNTJkZDAzOTRjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.gZFo0CP9gKMy-p1CitiGkehlJl_jLDy6u04qCnC1omA)

#### 5.2 Setup and hold timing concepts

- **Setup time:** the minimum interval for which data must be stable before the active clock edge.
- **Hold time:** the minimum interval for which data must remain stable after the active clock edge.
- **Arrival time:** the time at which data reaches the capturing element.
- **Required time:** the deadline by which data must arrive to satisfy the constraint.
- **Slack:** the margin between required and actual timing.

For setup analysis:

\[
\text{Slack} = \text{Required Time} - \text{Arrival Time}
\]

Positive slack means the evaluated constraint is met; zero slack is the boundary; negative slack indicates a violation.

![Setup and Hold Timing Concepts](https://private-user-images.githubusercontent.com/313950167/658815902-f9560748-dd69-4266-929a-2eed92d1fbc0.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTIwNjc4NTcsIm5iZiI6MTc5MjA2NzU1NywicGF0aCI6Ii8zMTM5NTAxNjcvNjU4ODE1OTAyLWY5NTYwNzQ4LWRkNjktNDI2Ni05MjlhLTJlZWQ5MmQxZmJjMC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYxMDE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MTAxNVQxMjMyMzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05ZjJjNWRlYTgxNTEzN2FjZDBkNDcyMGNlMjM4MTJiNzY3NjI5MzQwYjMwNjdlMDY2ZjRiYmY5MGNmNDIzMThjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.JryWic0f_s6ycNXUaEzLg89ZApDdVcUJjoAX5Tw3_As)

### 6. Ideal Clock vs. Propagated Clock

| Attribute | Ideal clock | Propagated clock |
| --- | --- | --- |
| Clock model | Simplified / idealized | Implemented clock network |
| Buffer delays | Not fully represented | Included in clock paths |
| Clock skew | Assumed idealized or negligible | Calculated from clock arrival times |
| Interconnect parasitics | Simplified or not included | Included when available |
| CTS effects | Not reflected as a completed network | Reflected in the propagated network |
| Typical use | Early-stage timing estimates | Later-stage, more physically representative analysis |

### 7. Placement Checks and Timing Closure

Placement reports commonly include:

- Cell and net counts
- Design area and utilization
- Row count and fixed-cell count
- Half-perimeter wirelength (HPWL)
- Mean and maximum displacement
- Placement legality checks

Timing closure is the iterative process of adjusting the implementation until the required timing constraints are met. Typical techniques include:

1. Cell upsizing or downsizing where appropriate
2. Buffer insertion and buffer resizing
3. Placement refinement
4. Clock-tree tuning and skew reduction
5. Routing adjustments
6. Reducing critical-path interconnect delay
7. Rechecking setup and hold paths after each change

## Conclusion

This module connects RTL synthesis with physical implementation and static timing analysis. The PicoRV32A example on SKY130 demonstrates how standard-cell mapping, placement, clock distribution, interconnect parasitics, and crosstalk affect timing.

**A successful physical design needs both physical legality and timing correctness.**
