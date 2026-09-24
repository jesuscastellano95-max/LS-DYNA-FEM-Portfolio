# LS-DYNA FEM Portfolio

Finite Element Analysis portfolio developed using **LS-DYNA** and **LS-PrePost**.

This repository documents my practical learning and development in nonlinear Finite Element Analysis through progressively more complex simulation projects.

The objective is not only to build numerical models, but also to understand their physical behaviour, validate simulation results, troubleshoot modelling problems and document each workflow in a reproducible way.

---

## 🔧 Software

- LS-DYNA Student
- LS-PrePost
- Microsoft Excel
- Git & GitHub

---

# 📚 Projects

| # | Project | Main Topics | Status |
|---|---|---|---|
| 01 | [Ball Impact on Aluminium Plate](./01_Ball_Impact_on_Plate/) | Explicit dynamics, contact, material failure, element erosion, energy analysis | ✅ Completed |
| 02 | [Tensile Test](./02_Tensile_test/) | Shell FEM, MAT_024, plasticity, strain hardening, failure, post-processing | ✅ Completed |

---

# 01 — Ball Impact on Aluminium Plate

Explicit dynamic simulation of a rigid steel spherical projectile impacting a thin aluminium plate.

The project focuses on:

- Explicit dynamic analysis
- Shell and solid elements
- Contact modelling
- Plastic deformation
- Material failure
- Element erosion
- Plate perforation
- Energy transfer
- Numerical troubleshooting

One of the main learning outcomes of this exercise was understanding the difference between **plastic deformation, material failure and actual element erosion**.

The model was progressively investigated until the expected perforation behaviour was obtained using an explicit erosion criterion.

➡️ **[View Exercise 01](./01_Ball_Impact_on_Plate/)**

---

# 02 — Tensile Test

Numerical tensile test of an aluminium specimen using shell elements and:

`*MAT_PIECEWISE_LINEAR_PLASTICITY (*MAT_024)`

The exercise covers the complete workflow from preprocessing to interpretation of the final material response.

Main topics:

- IGES geometry import
- Shell meshing
- `*SECTION_SHELL`
- `ELFORM = 2`
- Piecewise linear plasticity
- Tabulated stress–plastic strain data
- Prescribed-motion loading
- Node-set boundary conditions
- Effective Plastic Strain
- Effective von Mises Stress
- Material failure
- Element-level history extraction
- CSV data processing
- Stress vs Effective Plastic Strain analysis

The final post-processing stage combines the Effective Plastic Strain and von Mises Stress histories of the same finite element (**Element 899**) to study its local nonlinear response up to failure.

Detailed step-by-step modelling and post-processing tutorials are also included with this project.

➡️ **[View Exercise 02](./02_Tensile_test/)**

---

# 🎯 Learning Objectives

This portfolio is intended to progressively develop practical skills in:

- Finite Element Method fundamentals
- Explicit nonlinear analysis
- LS-DYNA keyword modelling
- LS-PrePost preprocessing
- LS-PrePost post-processing
- Shell and solid elements
- Material modelling
- Plasticity
- Contact
- Boundary and initial conditions
- Failure and element erosion
- Result interpretation
- Energy analysis
- Numerical troubleshooting
- Engineering documentation
- Reproducible simulation workflows

Future exercises will progressively introduce additional modelling techniques and more complex nonlinear FEM problems.

---

# 📁 Repository Structure

```text
LS-DYNA-FEM-Portfolio/
│
├── README.md
│
├── 01_Ball_Impact_on_Plate/
│   ├── README.md
│   ├── model/
│   └── images/
│
└── 02_Tensile_test/
    ├── README.md
    ├── model/
    ├── results/
    ├── images/
    ├── tutorials/
    └── sources/
```

Each project contains its own README with detailed information about the model, simulation setup and results.

Large LS-DYNA binary result files such as `d3plot` are stored locally and are not included in the repository due to their size.

---

# 📖 Documentation

The portfolio is designed to document not only the final models but also the modelling process and the lessons learned during their development.

Where appropriate, projects may include:

- Step-by-step tutorials
- LS-DYNA keyword files
- Material data used in the model
- Processed simulation results
- Post-processing figures
- Troubleshooting notes
- External source attribution

This makes the repository both a **technical portfolio** and a record of my progression in LS-DYNA and nonlinear FEM.

---

# ⚠️ Disclaimer

The simulations contained in this repository were developed for **educational, numerical experimentation and portfolio purposes**.

They are intended to demonstrate finite element modelling, simulation and post-processing workflows.

The models should not be considered engineering-certified analyses or validated designs for safety-critical applications.