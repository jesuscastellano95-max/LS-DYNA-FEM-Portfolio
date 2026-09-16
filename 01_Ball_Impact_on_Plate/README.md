# Ball Impact on a Plate — LS-DYNA Explicit FEM Simulation

## Project Overview

This project presents an explicit dynamic finite element simulation of a rigid spherical projectile impacting an aluminium plate using **LS-DYNA** and **LS-PrePost**.

The main objective was to study the structural response of the plate during impact, including plastic deformation, material failure, element erosion, fragmentation and energy transfer.

The project also provided practical experience in troubleshooting an explicit FEM model when the initial simulation produced severe plastic deformation but failed to reproduce the expected plate fragmentation.

---

## Model Description

The model consists of two main components:

| Component | Element formulation | Elements | Material model |
|---|---:|---:|---|
| Aluminium plate | Shell, ELFORM 2 | 10,000 | MAT_POWER_LAW_PLASTICITY |
| Steel ball | Solid, ELFORM 1 | 7,000 | MAT_RIGID |

**Total number of elements: 17,000**

The plate has in-plane dimensions of **200 × 200** and a shell thickness of **0.1**, according to the consistent unit system used in the model.

---

## Finite Element Mesh

The plate is discretized using **10,000 shell elements** with:

- ELFORM = 2
- 4 through-thickness integration points
- Shell thickness = 0.1

The spherical projectile is discretized using **7,000 solid elements** with:

- ELFORM = 1

This results in a total model size of **17,000 finite elements**.

---

## Material Models

### Aluminium Plate

The plate uses:

`*MAT_POWER_LAW_PLASTICITY`

Main parameters:

| Parameter | Value |
|---|---:|
| Young's modulus, E | 68.95 |
| Poisson's ratio, ν | 0.33 |
| Strength coefficient, K | 0.0974 |
| Hardening exponent, n | 0.33587 |
| Failure strain, EPSF | 0.4876 |

An additional erosion criterion is defined using:

`*MAT_ADD_EROSION`

with:

**Effective plastic strain at failure (EFFEPS) = 0.4876**

This criterion allows highly deformed elements to be removed once the defined failure condition is reached.

### Steel Projectile

The projectile is modelled as a rigid body using:

`*MAT_RIGID`

Main parameters:

| Parameter | Value |
|---|---:|
| Young's modulus, E | 200 |
| Poisson's ratio, ν | 0.30 |

The rigid formulation allows the analysis to focus primarily on the deformation and failure response of the impacted aluminium plate.

---

## Boundary Conditions

The external boundary of the plate is constrained using:

`*BOUNDARY_SPC_SET`

The selected boundary nodes are constrained in all translational and rotational degrees of freedom.

This represents a fully clamped plate boundary.

---

## Initial Conditions

The spherical projectile is assigned an initial velocity using:

`*INITIAL_VELOCITY_GENERATION`

The initial velocity is:

**Vz = -20**

The projectile therefore travels in the negative Z direction toward the plate.

---

## Contact Definition

Interaction between the projectile and plate is defined using:

`*CONTACT_AUTOMATIC_SURFACE_TO_SURFACE`

with:

- Static friction coefficient: 0.2
- Dynamic friction coefficient: 0.1

This contact definition controls the interaction between both surfaces during the impact event.

---

## Material Failure and Element Erosion

One of the most important parts of this project was the implementation and verification of material failure.

During an initial version of the simulation, the plate underwent very large plastic deformation but did not fragment as expected.

Post-processing showed that very high effective plastic strain values were being reached while the corresponding elements remained active.

The model was therefore reviewed and an explicit erosion criterion was implemented using:

`*MAT_ADD_EROSION`

with:

**EFFEPS = 0.4876**

After correctly implementing the erosion criterion, elements reaching the prescribed failure condition were removed from the calculation and the expected fragmentation behaviour was obtained.

This troubleshooting process demonstrated an important distinction between:

**plastic deformation → material failure criterion → numerical element erosion**

and showed why a material can undergo extreme plastic deformation without necessarily producing visible fragmentation unless an appropriate failure/erosion formulation is defined.

---

## Simulation Control

The explicit simulation is performed until:

**Termination time = 10**

The binary LS-DYNA result database is generated using:

`*DATABASE_BINARY_D3PLOT`

with an output interval of:

**Δt = 0.1**

This provides sufficient temporal resolution for post-processing the impact sequence.

---

# Results

## Impact and Plate Deformation

The projectile impacts the centre region of the plate, producing a highly localized deformation field.

> Image to be added.

---

## Effective Plastic Strain

Effective plastic strain is strongly concentrated around the projectile impact region.

As deformation progresses, the failure criterion is reached and elements begin to erode, allowing fragmentation of the plate to develop.

> Effective Plastic Strain image to be added.

---

## Energy Analysis

The global energy histories were analysed to evaluate the physical and numerical behaviour of the simulation.

### Kinetic Energy

The initial kinetic energy is approximately:

**Ek ≈ 21.8**

During the impact it decreases to approximately:

**Ek ≈ 11.0**

The reduction in kinetic energy occurs primarily during the main impact event.

---

### Internal Energy

Internal energy starts close to zero and increases during the impact to approximately:

**Ei ≈ 9.7**

This behaviour is consistent with the conversion of projectile kinetic energy into structural deformation, plasticity and damage.

The main increase in internal energy occurs during approximately the same time interval in which kinetic energy decreases.

---

### Total Energy

The total energy changes approximately from:

**Initial total energy ≈ 21.75**

to:

**Final total energy ≈ 20.82**

This corresponds to an approximate variation of:

**≈ 4.3 %**

The main variation occurs during the impact and erosion event. After this stage, the total energy remains approximately stable.

---

## Energy Transfer

The three energy histories show a consistent relationship during the impact:

**Projectile kinetic energy**

↓

**Plate deformation**

↓

**Plastic deformation**

↓

**Material failure and element erosion**

↓

**Increase in internal energy**

After the main impact event:

- Kinetic Energy ≈ 11.0
- Internal Energy ≈ 9.7
- Total Energy ≈ 20.8

The energy histories therefore provide an important global check of the simulation behaviour.

---

# Key Learning Outcomes

This project provided practical experience with:

- Explicit finite element analysis
- LS-DYNA keyword models
- Shell and solid finite elements
- Mesh generation
- Material modelling
- Rigid body modelling
- Contact definitions
- Boundary conditions
- Initial velocity conditions
- Plastic deformation
- Effective plastic strain
- Material failure
- Element erosion
- Impact and fragmentation
- Energy balance assessment
- LS-PrePost post-processing
- FEM model troubleshooting

---

## Software

- LS-DYNA Student
- LS-PrePost 2025 R1

---

## Model File

The LS-DYNA keyword model is available here:

[`ball_impact_plate.k`](./model/ball_impact_plate.k)

---

## Disclaimer

This is an educational finite element analysis project developed as part of my practical training in LS-DYNA and explicit dynamics.

The model is intended for learning, numerical experimentation and portfolio demonstration rather than engineering certification or design validation.