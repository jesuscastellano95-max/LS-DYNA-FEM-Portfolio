# External Sources

This project uses external geometry and material data as starting references.

The original third-party files are **not redistributed in this repository**.  
The links below point to their original sources.

---

## 1. Tensile Specimen Geometry

**Source:** GrabCAD  
**Resource:** Tensile Test Specimen  
**Original file used:** `ASTM C1273-05.igs`

The IGES geometry was imported into LS-PrePost and used as the basis for the finite element tensile specimen.

### Original source

https://grabcad.com/library/tensile-test-specimen-3

To reproduce the preprocessing workflow from the original CAD geometry:

1. Download the tensile specimen from the original GrabCAD page.
2. Locate `ASTM C1273-05.igs`.
3. Import the IGES file into LS-PrePost.
4. Follow the preprocessing procedure described in the Model Setup Tutorial.

---

## 2. LS-DYNA MAT_024 Material Database

**Source:** Varmint Al's Engineering Page  
**Database:** LS-DYNA `*MAT_PIECEWISE_LINEAR_PLASTICITY` material database  
**Original archive:** `mat24-all.zip`  
**File used:** `GMAT24F.TXT`

### Original source

https://www.varmintal.com/aengr.htm

The GM material database was selected because the model uses a consistent unit system based on:

- kg
- mm
- ms
- kN
- GPa
- kN·mm

The material selected for this exercise was:

**ALUMINUM PURE 99.45-O CONDITION**

Main properties used in the LS-DYNA model:

| Property | Value |
|---|---:|
| Density | 2.713E-06 kg/mm³ |
| Young's Modulus | 68.948 GPa |
| Poisson's Ratio | 0.33 |
| Initial Yield Stress | 0.013362 GPa |
| Failure Plastic Strain | 0.2723 |

The tabulated stress–plastic strain data were incorporated into the LS-DYNA model through `*DEFINE_CURVE` and referenced from `*MAT_024` using `LCSS`.

---

## Reproducibility

The original third-party geometry and complete material database are not required to run the final simulation contained in this repository.

The final keyword model:

`../model/02_Tensile_test.k`

already contains the LS-DYNA finite element model, material definition, material curve, boundary conditions and simulation controls required to reproduce the analysis.

The external sources are documented here to preserve traceability and to allow the complete preprocessing workflow to be reproduced from the original source data.

---

## Attribution

All third-party resources remain the property of their respective authors and publishers.

They are referenced here for educational, traceability and reproducibility purposes.