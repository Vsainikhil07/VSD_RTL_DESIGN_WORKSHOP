# Physical Design -- Module 2

## Floorplanning & Introduction to Library Cells

> **Module Focus:** Understanding how a digital design is organized
> physically on silicon, from floorplan creation to standard-cell
> placement and library-based physical representation.

------------------------------------------------------------------------

## 1. Module at a Glance

This module introduces the basic physical-design concepts required to
move from a logical design toward a physical layout.

The practical work focuses on:

-   Core and die dimensions
-   Utilization factor and aspect ratio
-   Standard-cell physical representation
-   Cell characterization and library information
-   Floorplan creation
-   Standard-cell placement
-   Metal-layer understanding
-   Physical layout inspection using Magic

The module connects **logical cells → physical library cells → floorplan
→ placement → layout view**.

------------------------------------------------------------------------

## 2. Learning Path

``` text
Logical Design
      ↓
Cell / Library Information
      ↓
Physical Cell Dimensions
      ↓
Floorplan
      ↓
Core Utilization + Aspect Ratio
      ↓
Standard-Cell Placement
      ↓
Metal Layer View
      ↓
Magic Layout Inspection
```

This sequence is used to understand why library data and floorplan
parameters directly affect physical implementation.

------------------------------------------------------------------------

## 3. Core Concepts

### 3.1 Floorplanning

Floorplanning defines the basic physical organization of the chip.

It determines:

-   Die dimensions
-   Core dimensions
-   Available placement area
-   Cell density
-   Overall physical shape

**Image:**\
`images/01_floorplan_overview.png`

> **Caption:** Initial floorplan showing the core and die regions.

------------------------------------------------------------------------

### 3.2 Utilization Factor

Utilization indicates how much of the available core area is occupied by
cells.

\[ Utilization = `\frac{Occupied\ Cell\ Area}{Core\ Area}`{=tex}
`\times 100`{=tex} \]

A suitable utilization value leaves enough space for placement and
routing.

**Image:**\
`images/02_utilization_calculation.png`

> **Caption:** Utilization-factor calculation and core-area
> relationship.

------------------------------------------------------------------------

### 3.3 Aspect Ratio

The aspect ratio controls the shape of the core.

\[ Aspect Ratio = `\frac{Height}{Width}`{=tex} \]

Changing the aspect ratio changes the physical shape of the floorplan.

**Image:**\
`images/03_aspect_ratio.png`

> **Caption:** Effect of aspect ratio on core dimensions.

------------------------------------------------------------------------

## 4. Introduction to Library Cells

Standard cells are not only logical symbols. Each cell also has physical
information required by placement and routing tools.

The library provides information such as:

-   Cell dimensions
-   Pin locations
-   Metal connections
-   Physical boundaries
-   Technology-layer information
-   Timing/characterization information

**Image:**\
`images/04_library_cell.png`

> **Caption:** Standard-cell physical representation from the technology
> library.

------------------------------------------------------------------------

## 5. Cell Characterization

Cell characterization describes the electrical and timing behavior of a
standard cell under different operating conditions.

Important parameters include:

-   Propagation delay
-   Rise time
-   Fall time
-   Input capacitance
-   Output behavior
-   Power-related characteristics

This information is used by physical-design and timing tools when
selecting and using library cells.

**Image:**\
`images/05_cell_characterization.png`

> **Caption:** Cell characterization data / timing information.

------------------------------------------------------------------------

## 6. From Cell Library to Physical Layout

A logical cell must be represented using an available physical library
cell before it can be placed in the layout.

``` text
Logical Cell
     ↓
Library Cell
     ↓
Physical Dimensions
     ↓
Placement Site
     ↓
Physical Layout
```

**Image:**\
`images/06_logical_to_physical.png`

> **Caption:** Mapping of a logical cell to its physical library
> representation.

------------------------------------------------------------------------

## 7. Practical Floorplan Work

### Step 1 --- Define Floorplan Parameters

The main parameters studied are:

  Parameter          Purpose
  ------------------ ---------------------------------
  Core Utilization   Controls cell density
  Aspect Ratio       Controls core shape
  Core Area          Defines usable placement region
  Die Area           Defines overall chip boundary

**Screenshot:**\
`images/07_floorplan_parameters.png`

------------------------------------------------------------------------

### Step 2 --- Generate the Floorplan

The floorplan is generated using the selected physical-design flow and
then inspected to verify the core/die arrangement.

**Screenshot:**\
`images/08_generated_floorplan.png`

> **Expected Output:** A valid floorplan with clearly identifiable core
> and die regions.

------------------------------------------------------------------------

### Step 3 --- Observe Standard-Cell Placement

Standard cells are positioned inside the available core area.

The placement should provide:

-   Organized cell rows
-   Reasonable cell density
-   Available routing space
-   Correct physical alignment

**Screenshot:**\
`images/09_standard_cell_placement.png`

------------------------------------------------------------------------

## 8. Metal Layer Study

The module also introduces the role of metal layers used for physical
interconnection.

The practical observation includes:

-   Metal-layer structure
-   Cell-level interconnects
-   M2 and M3 views
-   Relationship between cells and routing layers

**Screenshot:**\
`images/10_metal_m2_view.png`

**Screenshot:**\
`images/11_metal_m3_view.png`

------------------------------------------------------------------------

## 9. Magic Layout Inspection

Magic is used to inspect the physical layout at a lower level.

The layout view helps identify:

-   Standard-cell geometry
-   Metal layers
-   Contacts/vias
-   Cell boundaries
-   Physical interconnections

**Screenshot:**\
`images/12_magic_layout.png`

> **Expected Output:** A clear physical layout view showing the
> implemented cell structures and metal connections.

------------------------------------------------------------------------

## 10. Practical Evidence

The following screenshots can be used as the main evidence section of
the module.

  Evidence                  Screenshot
  ------------------------- -----------------------------------------
  Floorplan parameters      `images/07_floorplan_parameters.png`
  Generated floorplan       `images/08_generated_floorplan.png`
  Standard-cell placement   `images/09_standard_cell_placement.png`
  M2 view                   `images/10_metal_m2_view.png`
  M3 view                   `images/11_metal_m3_view.png`
  Magic layout              `images/12_magic_layout.png`

Keep the screenshots **inside an `images/` folder in the same Module-2
repository directory**.

Recommended structure:

``` text
PD-Module-2/
│
├── README.md
├── images/
│   ├── 01_floorplan_overview.png
│   ├── 02_utilization_calculation.png
│   ├── 03_aspect_ratio.png
│   ├── 04_library_cell.png
│   ├── 05_cell_characterization.png
│   ├── 06_logical_to_physical.png
│   ├── 07_floorplan_parameters.png
│   ├── 08_generated_floorplan.png
│   ├── 09_standard_cell_placement.png
│   ├── 10_metal_m2_view.png
│   ├── 11_metal_m3_view.png
│   └── 12_magic_layout.png
│
└── ...
```

------------------------------------------------------------------------

## 11. Expected Practical Outputs

By the end of the module, the practical work should demonstrate:

-   A defined floorplan
-   Calculated utilization factor
-   Selected aspect ratio
-   Understanding of standard-cell physical dimensions
-   Basic cell-characterization information
-   Visible standard-cell placement
-   M2 and M3 metal-layer observations
-   Physical layout inspection in Magic

------------------------------------------------------------------------

## 12. Key Takeaways

-   **Floorplanning** establishes the physical organization of the
    design.
-   **Utilization** determines how densely cells occupy the core.
-   **Aspect ratio** controls the shape of the core.
-   **Library cells** provide the physical information required for
    implementation.
-   **Cell characterization** provides timing and electrical information
    used by the design flow.
-   **Placement** converts the floorplan into an organized arrangement
    of physical cells.
-   **Metal layers** provide the interconnections between cells.
-   **Magic** helps inspect the resulting physical geometry.

------------------------------------------------------------------------

## 13. Module Outcome

The module builds a practical connection between digital logic and
physical implementation:

> **Logical Design → Library Cells → Floorplanning → Placement → Metal
> Layers → Layout Inspection**

This provides the foundation for understanding later stages of ASIC
physical design.

------------------------------------------------------------------------

### Screenshot Guidelines

-   Use **your own terminal/tool screenshots** for practical evidence.
-   Crop unnecessary desktop areas where possible.
-   Keep terminal commands and important outputs readable.
-   Use descriptive filenames instead of `Screenshot1.png`,
    `Screenshot2.png`, etc.
-   Place screenshots immediately after the concept or practical step
    they prove.
-   Avoid adding a screenshot for every sentence; one strong screenshot
    per practical result is usually enough.
