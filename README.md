# 🧬 CADD Pipeline Capstone Project — Protein 7SFB
**The Insilico Lab | 3-Week Virtual Training in Computer-Aided Drug Design**

> **Participant:** Alaa Hamid | **Cohort:** 4 | **Date Submitted:** 30/05/2026

---

## 📋 Project Overview

This capstone project applies a full **Structure-Based Drug Design (SBDD)** pipeline to the SARS-CoV-2 Main Protease (Mpro), PDB ID: **7SFB**, using 5 FDA-approved ligands as candidates. Each task builds on the previous, covering the complete workflow from drug-likeness screening to AI-based structure evaluation.

### Ligands Studied
| # | Compound |
|---|----------|
| 1 | Aspirin |
| 2 | Ibuprofen |
| 3 | Acetaminophen |
| 4 | Naproxen |
| 5 | Diclofenac |

### Pipeline Summary
| # | Task | Tool(s) | Key Output |
|---|------|---------|------------|
| 1 | Drug-likeness Screening | PubChem + SwissADME | Lipinski table (5 ligands) |
| 2 | Protein Preparation | UCSF Chimera / Dockprep | Cleaned 7SFB PDB file |
| 3 | Binding Site Prediction | DoGSiteScorer | Binding pocket + residues |
| 4 | ADMET Prediction | ADMETlab 3.0 | ADMET table (5 ligands) |
| 5 | AlphaFold3 Structure Evaluation | AlphaFold3 + PyMOL | Aligned overlay + RMSD |

---

## Task 1 — Drug-likeness Screening

**Objective:** Evaluate the drug-likeness of all 5 ligands using Lipinski's Rule of Five (Ro5) via SwissADME.

**Tools:** [PubChem](https://pubchem.ncbi.nlm.nih.gov) · [SwissADME](http://www.swissadme.ch) · PyMOL

### Compound Information

| Compound | PubChem CID | Canonical SMILES |
|----------|-------------|-----------------|
| Aspirin | 2244 | `CC(=O)OC1=CC=CC=C1C(=O)O` |
| Ibuprofen | 3672 | `CC(C)CC1=CC=C(C=C1)C(C)C(=O)O` |
| Acetaminophen | 1983 | `CC(=O)NC1=CC=C(C=C1)O` |
| Naproxen | 156391 | `C[C@@H](C1=CC2=C(C=C1)C=C(C=C2)OC)C(=O)O` |
| Diclofenac | 3033 | `C1=CC=C(C(=C1)CC(=O)O)NC2=C(C=CC=C2Cl)Cl` |

### Lipinski Rule of Five Results

| Compound | MW (≤500) | HBD (≤5) | HBA (≤10) | LogP (≤5) | Ro5 Pass? |
|----------|-----------|----------|-----------|-----------|-----------|
| Aspirin | ✅ | ✅ | ✅ | ✅ | **Yes** |
| Ibuprofen | ✅ | ✅ | ✅ | ✅ | **Yes** |
| Acetaminophen | ✅ | ✅ | ✅ | ✅ | **Yes** |
| Naproxen | ✅ | ✅ | ✅ | ✅ | **Yes** |
| Diclofenac | ✅ | ✅ | ✅ | ✅ | **Yes** |

### Justification
All five compounds pass Lipinski's Rule of Five, indicating a high likelihood of successful oral absorption and bioavailability. The absence of any violations across all compounds demonstrates their drug-like physicochemical properties. Compounds with more rule violations typically exhibit lower oral bioavailability, so a clean Ro5 profile for all five candidates is a strong positive indicator for their potential in clinical settings.

---

## Task 2 — Protein Preparation

**Objective:** Download and prepare the 7SFB protein structure for molecular docking using UCSF Chimera.

**Tools:** [RCSB PDB](https://www.rcsb.org) · UCSF Chimera (Dockprep) · PyMOL

### Protein Structure Information

| Property | Details |
|----------|---------|
| PDB ID | 7SFB |
| Protein Name | SARS-CoV-2 Main Protease (Mpro) in Complex with ML101 |
| Experimental Method | X-Ray Diffraction |
| Resolution | 1.90 Å |
| Co-crystallized Ligand | 90U |
| Water Molecules Removed | ✅ Yes |
| Hydrogens Added via Dockprep | ✅ Yes |
| Prepared File Name | Cleaned7SFB |

### Preparation Log

| Step | Action Performed | Observation / Result |
|------|-----------------|---------------------|
| 1 | Opened 7SFB in Chimera | Visualized and analyzed the structure |
| 2 | Selected and deleted 90U | Removed co-crystallized ligand |
| 3 | Selected and deleted all co-factors | Removed from structure |
| 4 | Selected and deleted HOH | Removed all water molecules |
| 5 | Used Dock Prep | Added hydrogens, charges; prepared for docking |
| 6 | Saved the file | File saved as .pdb |

### Justification
Protein preparation is necessary to eliminate issues during molecular docking and ensure reliable, accurate results. Without this step, the presence of water molecules, co-crystallized ligands, and unresolved atoms can introduce artefacts that compromise docking scores. After preparation, 7SFB was free of the co-crystallized ligand and other co-factors, making it computation-ready for the docking pipeline.

---

## Task 3 — Binding Site Prediction

**Objective:** Identify and characterize the primary binding site of 7SFB using DoGSiteScorer.

**Tools:** [DoGSiteScorer (proteins.plus)](https://proteins.plus) · PyMOL

> ⚠️ **Note:** The RAW (unprepared) 7SFB PDB file was used for this task to ensure the prediction reflects the native structure.

### Binding Site Summary

| Parameter | Value |
|-----------|-------|
| Top Pocket ID | P_0 |
| Pocket Volume | 642.94 Å³ |
| Pocket Surface Area | 776.96 Å² |
| Druggability Score | 0.75 |

### Binding Site Residues

| # | Residue Name | Residue Number |
|---|-------------|----------------|
| 1 | THR | 25 |
| 2 | THR | 26 |
| 3 | LEU | 27 |
| 4 | HIS | 41 |
| 5 | VAL | 42 |

### Justification
DoGSiteScorer identified two top-scoring pockets: P_0 (score 0.75) and P_1 (score 0.78). Pocket P_0 was selected despite its slightly lower score because structural analysis confirmed it covers the targeted binding area where the co-crystallized ligand binds. The identified residues (THR25, THR26, LEU27, HIS41, VAL42) will serve as coordinates to define the docking grid box in subsequent docking steps.

---

## Task 4 — ADMET Prediction

**Objective:** Predict the ADMET profiles of all 5 ligands using ADMETlab 3.0.

**Tools:** [ADMETlab 3.0](https://admetlab3.scbdd.com)

### A — Absorption

| Property | Aspirin | Ibuprofen | Acetaminophen | Naproxen | Diclofenac |
|----------|---------|-----------|---------------|----------|------------|
| Caco-2 Permeability | -4.985 | -4.301 | -4.272 | -4.657 | -4.266 |
| Human Intestinal Absorption (HIA) | 0–0.1 | 0–0.1 | 0–0.1 | 0–0.1 | 0–0.1 |
| MDCK Permeability | -4.577 | -4.648 | -4.407 | -4.764 | -4.452 |

### D — Distribution

| Property | Aspirin | Ibuprofen | Acetaminophen | Naproxen | Diclofenac |
|----------|---------|-----------|---------------|----------|------------|
| Blood-Brain Barrier (BBB) | 0.9–1.0 | 0.5–0.7 | 0.5–0.7 | 0–0.1 | 0.9–1.0 |
| Plasma Protein Binding (PPB) | 60.3% | 97.6% | 17.3% | 97.9% | 99.2% |
| Fraction Unbound (Fu) | 40.1% | 2.6% | 65.6% | 1.9% | 0.3% |

### M — Metabolism

| Property | Aspirin | Ibuprofen | Acetaminophen | Naproxen | Diclofenac |
|----------|---------|-----------|---------------|----------|------------|
| CYP1A2 Inhibitor | 0–0.1 | 0–0.1 | 0–0.1 | 0–0.1 | 0–0.1 |
| CYP3A4 Inhibitor | 0–0.1 | 0.9–1.0 | 0–0.1 | 0–0.1 | 0–0.1 |
| CYP2C19 Inhibitor | 0–0.1 | 0–0.1 | 0–0.1 | 0–0.1 | 0–0.1 |

### E — Excretion

| Property | Aspirin | Ibuprofen | Acetaminophen | Naproxen | Diclofenac |
|----------|---------|-----------|---------------|----------|------------|
| Half-life T½ (h) | 0.822 | 1.449 | 1.115 | 1.786 | 1.64 |
| Clearance (CL) | 2.766 | 1.038 | 8.903 | 0.375 | 0.456 |

### T — Toxicity

| Property | Aspirin | Ibuprofen | Acetaminophen | Naproxen | Diclofenac |
|----------|---------|-----------|---------------|----------|------------|
| hERG Inhibition | 0.015 | 0.165 | 0.07 | 0.167 | 0.069 |
| Hepatotoxicity (DILI) | 0.744 | 0.928 | 0.619 | 0.998 | 1.0 |
| AMES Toxicity | 0.256 | 0.046 | 0.55 | 0.175 | 0.094 |

### Justification
**Acetaminophen** has the best overall ADMET profile among the five compounds. It shows the best Caco-2 and MDCK permeability scores (indicating good absorption), the lowest plasma protein binding with the highest fraction unbound (meaning more free drug is available to reach the target), and the lowest DILI (hepatotoxicity) score. Its main weakness is a high clearance rate, suggesting rapid elimination. **Naproxen** is the runner-up, with the best half-life, low clearance, and the lowest BBB penetration — making it a favorable candidate from a safety perspective.

---

## Task 5 — AlphaFold3 AI-Based Structure Evaluation

**Objective:** Predict the 3D structure of 7SFB using AlphaFold3 and evaluate prediction quality by alignment with the experimental structure.

**Tools:** [AlphaFold3](https://alphafoldserver.com) · PyMOL · [RCSB PDB](https://www.rcsb.org)

### FASTA Sequence — Chain A of 7SFB

```
>7SFB_A | Chain A
SGFRKMAFPSGKVEGCMVQVTCGTTTLNGLWLDDVVYCPRHVICTSEDMLNPNYEDLLIRKSNHNFLVQ
AGNVQLRVIGHSMQNCVLKLKVDTANPKTPKYKFVRIQPGQTFSVLACYNGSPSGVYQCAMRPNFTIKGS
FLNGSCGSVGFNIDYDCVSFCYMHHMELPTGVHAGTDLEGNFYGPFVDRQTAQAAGTDTTITVNVLAWLY
AAVINGDRWFLNRFTTTLNDFNLVAMKYNYEPLTQDHVDILGPLSAQTGIAVLDMCASLKELLQNGMNGR
TILGSALLEDEFTPFDVVRQCSGVTFQ
```

### Structural Comparison Summary

| Metric | Value | Interpretation |
|--------|-------|----------------|
| RMSD | **0.641 Å** | Excellent |
| Residues Aligned | 301 vs. 306 | Very high coverage |
| Overall Quality | **Excellent** | — |
| Predicted Structure File | `fold_2026_05_31_20_27_model_0.cif` | — |

### Justification
The AlphaFold3 predicted structure matches the experimental 7SFB structure with high fidelity. An RMSD of 0.641 Å across 301 of 306 residues represents near-complete structural agreement, well below the 2 Å threshold for excellent prediction. AlphaFold3 assigned a pLDDT score exceeding 90% for most residues and a pTM of 0.93, confirming high confidence across the full structure. This demonstrates that AI-based structure prediction has reached a level of accuracy comparable to experimental methods, with strong implications for accelerating early-stage drug discovery when experimental structures are unavailable.

---

## 🏆 Pipeline Complete

This project successfully completed the full Insilico Lab CADD pipeline:

**Drug-likeness Screening → Protein Preparation → Binding Site Prediction → ADMET Profiling → AlphaFold3 Structural Evaluation**

---

*© The Insilico Lab — Capstone Project, Cohort 4*
