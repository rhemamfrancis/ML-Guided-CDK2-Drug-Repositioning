
# ML-Guided CDK2 Drug Repurposing for Breast Cancer

### Machine Learning and Molecular Docking of Approved Drugs Targeting CDK2 in CDK4/6 Inhibitor-Resistant Breast Cancer

## Project Overview

This project investigates the potential repurposing of approved drugs as inhibitors of cyclin-dependent kinase 2 (CDK2), a potential therapeutic target in hormone receptor-positive (HR+)/HER2-negative breast cancer that has developed resistance to CDK4/6 inhibitors.

Although CDK4/6 inhibitors have significantly improved treatment outcomes, resistance can develop through mechanisms involving cyclin E overexpression and increased CDK2 activity. Targeting CDK2 may provide an alternative strategy for overcoming resistance in tumors that retain functional retinoblastoma protein (Rb).

Using a computational drug discovery workflow, I integrated **ChEMBL bioactivity data, molecular fingerprints, machine learning classification, chemical structure filtering, and molecular docking** to identify approved drugs with potential CDK2 inhibitory activity.


## Tools and Technologies

- **Programming:** Python, Google Colab
- **Cheminformatics:** RDKit, Morgan fingerprints
- **Machine Learning:** Scikit-learn, random forest classification
- **Bioactivity Database:** ChEMBL
- **Protein Structure:** RCSB Protein Data Bank (PDB: 1H01)
- **Molecular Docking:** Smina
- **Molecular Visualization:** PyMOL
- **Data Analysis:** Pandas, NumPy, Matplotlib

## Methodology

### 1. CDK2 Bioactivity Data Collection

Retrieved experimentally measured CDK2 bioactivity data from ChEMBL (CHEMBL301) to develop a machine learning classifier.

The training dataset included:

- 275 active compounds (pChEMBL > 8)
- 962 inactive compounds (potency > 10 µM)

Compounds were represented using Morgan molecular fingerprints to capture structural features associated with CDK2 activity.

### 2. Approved-Drug Library Preparation

Collected 3,417 approved-drug records from ChEMBL and applied chemical preprocessing and quality-control filters.

Processing included:

- Salt removal and structure standardization
- Duplicate removal
- PAINS filtering
- Molecular property filtering
- Removal of 10 previously identified CDK2 binders

This produced a screening library of **1,730 approved drugs**. An additional 51 compounds with measured CDK2 inactivity were excluded from the candidate list, leaving **1,677 drugs for prospective screening**.

### 3. Machine Learning Model Development

Developed a random forest classifier using Morgan count fingerprints to predict potential CDK2 activity.

Model performance was evaluated using five-fold cross-validation with random, scaffold-based, and chemical cluster-based splitting.

| Validation strategy | ROC-AUC |
|---|---:|
| Random split | 0.976 |
| Scaffold split | 0.963 |
| Cluster split | 0.882 |
| Known approved CDK2 binders | 0.650 |

The decrease in performance under cluster-based validation demonstrated the difficulty of generalizing predictions to structurally dissimilar compounds.

The model was used to rank approved drugs according to their predicted probability of CDK2 activity.

### 4. Molecular Docking Validation

Examined the ATP-binding pocket of CDK2 using the crystal structure **PDB 1H01**, and focused on interactions with the hinge residue Leu83.

Docking was performed using Smina and evaluated through:

- Re-docking of the crystallographic ligand FAL
- Comparison of predicted and experimental ligand orientations
- Analysis of Leu83 hinge contacts
- Docking-score discrimination between known active and inactive compounds

Docking validation produced:

- FAL re-docking RMSD: 3.6 Å
- Docking ROC-AUC: approximately 0.54

Because docking did not reliably reproduce the crystallographic binding pose or distinguish active compounds from inactive compounds, docking scores were **not used as the primary ranking method**.

Instead, docking was used to generate exploratory binding-pose hypotheses for compounds prioritized by machine learning.

### 5. Candidate Screening and Shortlisting

The random forest model ranked 1,677 approved drugs for potential CDK2 activity.

The top 100 compounds underwent additional structural filtering:

| Screening stage | Remaining compounds |
|---|---:|
| ML-ranked approved drugs | 1,677 |
| Top ML predictions | 100 |
| Sulfonyl-group exclusion and ≥20 heavy atoms | 24 |
| Hinge-binding motif filtering | 13 |
| Manual structural review | 9 |

Nine candidates were selected for molecular docking and structural evaluation.

### 6. Molecular Docking and Lead Candidate Selection

The nine shortlisted compounds were docked into the CDK2 ATP-binding pocket using three docking seeds. Predicted ligand interactions with Leu83 were examined to assess potential hinge binding orientations.

Four compounds were prioritized for further investigation:

| Compound | Original therapeutic application |
|---|---|
| **Rilpivirine** | HIV non-nucleoside reverse transcriptase inhibitor |
| Etravirine | HIV non-nucleoside reverse transcriptase inhibitor |
| Belumosudil | ROCK2 inhibitor |
| Pralsetinib | RET kinase inhibitor |

**Rilpivirine was selected as the primary lead candidate** because of its structural similarity to the crystallographic ligand FAL and its predicted interactions with both Leu83 hinge atoms.

Its top-scoring docked pose demonstrated:

- Docking score: −9.61 kcal/mol
- Leu83 N contact distance: 3.14 Å
- Leu83 O contact distance: 2.80 Å

These observations support further investigation but do not confirm CDK2 inhibitory activity.

## Key Findings

- Screened **1,677 approved drugs** for potential CDK2 activity using machine learning.
- Achieved a **cluster-split ROC-AUC of 0.882**, compared with 0.976 under random splitting.
- Narrowed the highest-ranked 100 compounds to **nine docking candidates** through chemical and structural filtering.
- Identified **rilpivirine as the lead candidate**, with etravirine, belumosudil, and pralsetinib as alternatives.



## Project Context

**Course:** RBIF110 Final Assignment  
**Program:** M.S. Bioinformatics, Brandeis University  
**Completed:** October 2026

*This project was completed as an academic computational drug discovery exercise. The identified compounds are research candidates and are not established treatments for CDK4/6 inhibitor-resistant breast cancer.*
