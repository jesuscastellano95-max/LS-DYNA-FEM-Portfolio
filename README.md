# Ball Impact on an Aluminium Plate — LS-DYNA Explicit FEM Simulation

## 📌 Project Overview

This project presents an explicit dynamic Finite Element Method (FEM) simulation of a rigid steel spherical projectile impacting a thin aluminium plate using **LS-DYNA** and **LS-PrePost**.

The main objective was to study the structural response of the plate during impact, including:

- Large deformation
- Plastic deformation
- Material failure
- Element erosion
- Plate fragmentation
- Energy transfer during impact

An important part of the project was also the troubleshooting of the failure behaviour. The initial model developed severe plastic deformation but did not reproduce the expected plate fragmentation.

The model was therefore investigated and corrected until the expected erosion and fragmentation behaviour was obtained.

---

# ⚙️ Software

- **LS-DYNA Student — R16.1**
- **Double Precision SMP solver**
- **LS-PrePost 2025 R1**
- Git & GitHub

Analysis type:

**Explicit dynamic finite element analysis**

---

# 📐 Unit System

LS-DYNA does not impose a predefined unit system. A consistent system of units was used throughout this model:

| Magnitude | Unit |
|---|---|
| Length | mm |
| Time | ms |
| Mass | kg |
| Velocity | mm/ms = m/s |
| Force | kN |
| Stress / Pressure | kN/mm² = GPa |
| Density | kg/mm³ |
| Energy | kN·mm = J |

An important consequence of this unit system is:

**1 kN·mm = 1 J**

Therefore, the energy values obtained during post-processing can be directly interpreted in **joules**.

---

# 🔷 Model Description

The numerical model consists of two main components:

| Component | Element Type | Formulation | Number of Elements | Material Model |
|---|---|---:|---:|---|
| Aluminium plate | Shell | ELFORM 2 | 10,000 | MAT_POWER_LAW_PLASTICITY |
| Steel projectile | Solid | ELFORM 1 | 7,000 | MAT_RIGID |

**Total number of finite elements: 17,000**

The aluminium plate has dimensions of:

**200 × 200 mm**

with a thickness of:

**0.1 mm**

The spherical projectile has a radius of approximately:

**15 mm**

---

# 🕸️ Finite Element Mesh

## Aluminium Plate

The plate is discretized using:

- **10,000 shell elements**
- ELFORM = 2
- 4 through-thickness integration points
- Element dimensions ≈ **2 × 2 mm**
- Shell thickness = **0.1 mm**

The relatively fine and regular mesh allows the highly localized deformation around the impact region to be represented.

## Steel Projectile

The spherical projectile is discretized using:

- **7,000 solid elements**
- ELFORM = 1

The projectile is defined as a rigid body so that the analysis primarily focuses on the deformation and failure response of the aluminium plate.

---

# 🧱 Material Models

## Aluminium Plate

The aluminium plate uses:

`*MAT_POWER_LAW_PLASTICITY`

Main material parameters:

| Parameter | Value |
|---|---:|
| Density | 2.685 × 10⁻⁶ kg/mm³ |
| Young's modulus, E | 68.95 GPa |
| Poisson's ratio, ν | 0.33 |
| Strength coefficient, K | 0.0974 GPa |
| Hardening exponent, n | 0.33587 |
| Failure strain, EPSF | 0.4876 |

This material model allows the nonlinear plastic response of the aluminium plate to be represented during the impact event.

---

## Steel Projectile

The projectile is modelled as a rigid body using:

`*MAT_RIGID`

Main parameters:

| Parameter | Value |
|---|---:|
| Density | 7.8 × 10⁻⁶ kg/mm³ |
| Young's modulus, E | 200 GPa |
| Poisson's ratio, ν | 0.30 |

The rigid formulation prevents significant deformation of the projectile and allows the analysis to focus on the response of the impacted plate.

---

# 🔒 Boundary Conditions

The external boundary of the aluminium plate is constrained using:

`*BOUNDARY_SPC_SET`

The selected boundary nodes are constrained in all translational and rotational degrees of freedom.

This represents a **fully clamped plate boundary condition**.

---

# 🚀 Initial Conditions

The spherical projectile is assigned an initial velocity using:

`*INITIAL_VELOCITY_GENERATION`

The initial velocity is:

**Vz = −20 mm/ms**

Since:

**1 mm/ms = 1 m/s**

the impact velocity is therefore:

**Vz = −20 m/s**

The negative sign indicates that the projectile travels toward the plate in the negative Z direction.

---

# 🔗 Contact Definition

The interaction between the projectile and the plate is defined using:

`*CONTACT_AUTOMATIC_SURFACE_TO_SURFACE`

The friction coefficients are:

| Parameter | Value |
|---|---:|
| Static friction coefficient | 0.20 |
| Dynamic friction coefficient | 0.10 |

This contact definition controls the interaction between the projectile and plate during the impact event.

---

# 💥 Material Failure and Element Erosion

One of the most important learning points of this project was the implementation and troubleshooting of material failure.

In the initial simulation, the aluminium plate experienced very large plastic deformation but did not fragment as expected.

The projectile caused the plate to deform severely, but the affected shell elements remained active instead of being removed after reaching the expected failure condition.

The material and model definitions were systematically reviewed, including:

- Geometry
- Mesh
- Shell formulation
- Through-thickness integration points
- Material properties
- Failure strain
- Projectile material
- Initial velocity
- Boundary conditions
- Contact
- Simulation termination time
- Solver precision

An explicit additional erosion criterion was finally introduced using:

`*MAT_ADD_EROSION`

associated with the aluminium material.

The effective plastic strain criterion was defined as:

**EFFEPS = 0.4876**

After implementing this erosion criterion, elements reaching the prescribed failure condition were removed from the calculation and the expected plate fragmentation was obtained.

This troubleshooting process highlighted an important distinction between:

**Plastic deformation**

↓

**Material failure criterion**

↓

**Element erosion**

↓

**Visible fragmentation**

A material can therefore experience very large plastic deformation without necessarily producing visible fragmentation if the corresponding failure and element-removal behaviour is not properly represented.

---

# ⏱️ Simulation Control

The explicit simulation is performed until:

**Termination time = 10 ms**

The LS-DYNA binary result database is generated using:

`*DATABASE_BINARY_D3PLOT`

with an output interval of:

**Δt = 0.1 ms**

This provides sufficient temporal resolution to analyse the evolution of the impact and fragmentation process.

---

# 📊 Results

## Impact and Plate Deformation

The projectile impacts the central region of the aluminium plate.

The impact initially produces highly localized deformation around the contact region. As the projectile continues moving through the plate, plastic deformation increases until the defined failure condition is reached.

Element erosion then allows the formation of an opening and the subsequent fragmentation of the plate.

<!-- Image will be added here -->

---

## Effective Plastic Strain

The effective plastic strain field provides a useful representation of the regions undergoing irreversible plastic deformation.

The highest plastic strains are concentrated around the projectile impact region.

As deformation progresses, elements in this region reach the defined failure criterion:

**EFFEPS = 0.4876**

and are subsequently eroded from the calculation.

This allows the transition from severe plastic deformation to actual plate fragmentation.

<!-- Effective Plastic Strain image will be added here -->

---

# ⚡ Energy Analysis

Global energy histories were analysed to evaluate the evolution of the system during impact.

The three main quantities considered were:

- Kinetic Energy
- Internal Energy
- Total Energy

---

## Kinetic Energy

At the beginning of the simulation, most of the system energy is associated with the motion of the projectile.

The initial kinetic energy is approximately:

**Ek,initial ≈ 21.8 J**

During the impact event, the kinetic energy decreases rapidly.

After the main impact event it stabilizes at approximately:

**Ek,final ≈ 11.0 J**

Therefore, the approximate reduction in kinetic energy is:

**ΔEk ≈ 10.8 J**

This reduction corresponds to energy transferred from projectile motion to the structural response of the impacted plate and other mechanisms involved in the impact process.

<!-- Kinetic Energy graph will be added here -->

---

## Internal Energy

At the beginning of the simulation:

**Ei,initial ≈ 0 J**

During the impact, internal energy increases rapidly until reaching approximately:

**Ei,final ≈ 9.7 J**

The main increase in internal energy occurs during approximately the same time interval in which kinetic energy decreases.

This behaviour is consistent with the conversion of part of the projectile's kinetic energy into the structural and material response of the plate, including elastic and plastic deformation.

The internal energy stabilizes after the main impact event.

<!-- Internal Energy graph will be added here -->

---

## Total Energy

The total energy provides a useful global assessment of the numerical behaviour of the simulation.

Initially:

**Etotal,initial ≈ 21.75 J**

At the end of the analysed event:

**Etotal,final ≈ 20.82 J**

The approximate difference is:

**ΔEtotal ≈ 0.93 J**

which corresponds to:

**ΔEtotal ≈ 4.3 %**

The main variation occurs during the impact and erosion event.

After the main impact process, the total energy becomes approximately stable.

It would therefore not be correct to state that total energy remains perfectly constant throughout the entire simulation. Instead, the model shows an approximately **4.3 % variation in the recorded total energy during the impact/erosion process**, followed by a stable post-impact response.

<!-- Total Energy graph will be added here -->

---

# 🔄 Energy Transfer During Impact

The energy histories provide a clear representation of the physical evolution of the simulation.

Initially, the system contains approximately:

**21.8 J of kinetic energy**

During impact:

**Kinetic Energy ↓**

while:

**Internal Energy ↑**

After the main impact event:

- Kinetic Energy ≈ **11.0 J**
- Internal Energy ≈ **9.7 J**
- Total Energy ≈ **20.8 J**

The approximate sum:

**11.0 + 9.7 ≈ 20.7 J**

is close to the recorded total energy of approximately:

**20.8 J**

The three energy histories therefore show a consistent overall relationship during the impact event.

The physical sequence can be summarized as:

**Projectile kinetic energy**

↓

**Impact**

↓

**Plate deformation**

↓

**Plastic deformation**

↓

**Material failure**

↓

**Element erosion and fragmentation**

↓

**Energy redistribution**

↓

**Post-impact stabilization**

---

# 🔍 Troubleshooting and Model Development

The troubleshooting stage became an important part of this project.

The initial model reproduced the impact and large deformation of the plate, but not the expected fragmentation.

Several potential causes were investigated before modifying the failure formulation.

The analysis confirmed that:

- The projectile geometry was correct.
- The plate geometry and thickness were correct.
- The shell and solid formulations were correctly defined.
- The projectile initial velocity was correct.
- The material parameters were consistent with the reference model.
- The contact definition was active.
- Increasing the termination time did not solve the problem.
- Reducing the failure strain alone did not produce the expected fragmentation.
- The simulation was already running with the LS-DYNA **Double Precision** solver.

The final working model retained:

`*MAT_POWER_LAW_PLASTICITY`

for the aluminium constitutive behaviour and added:

`*MAT_ADD_EROSION`

to explicitly define the element erosion criterion.

This produced the expected fragmentation behaviour.

---

# 🎯 Key Learning Outcomes

This project provided practical experience with:

- Finite Element Method fundamentals
- Explicit dynamic analysis
- LS-DYNA keyword models
- LS-PrePost preprocessing
- LS-PrePost post-processing
- Shell finite elements
- Solid finite elements
- Mesh generation
- Material modelling
- Rigid body modelling
- Contact definitions
- Boundary conditions
- Initial conditions
- Plastic deformation
- Effective plastic strain
- Material failure
- Element erosion
- Impact simulation
- Fragmentation
- Energy transfer
- Energy balance assessment
- Numerical troubleshooting
- Interpretation of FEM results

The project also demonstrated that obtaining a visually plausible simulation is not sufficient by itself. Numerical results, material behaviour and energy histories must also be analysed to understand whether the model is behaving as intended.

---

# 📁 Repository Structure

```text
01_Ball_Impact_on_Plate/
│
├── README.md
│
├── model/
│   └── ball_impact_plate.k
│
├── images/
│
├── video/
│
└── documentation/
```

The original LS-DYNA result databases (`d3plot`, `binout`, etc.) are stored locally and are not included in the Git repository due to their size.

---

# 📄 LS-DYNA Model

The final LS-DYNA keyword model used for the simulation is available here:

[`ball_impact_plate.k`](./model/ball_impact_plate.k)

---

# ⚠️ Disclaimer

This is an educational Finite Element Analysis project developed as part of my practical training in **LS-DYNA, explicit dynamics and nonlinear FEM**.

The model is intended for learning, numerical experimentation and portfolio demonstration.

It should not be interpreted as an engineering-certified model or as a validated design analysis.