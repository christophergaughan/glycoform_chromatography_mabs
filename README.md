# Strategic Glycan Engineering Scanner

## Bridging AI Antibody Design and Manufacturing Reality

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

**AntibodyML Consulting LLC**

---

## The Problem

Modern AI tools, such as RFdiffusion, ProteinMPNN, and AntiFold, are revolutionizing antibody design. They can generate thousands of candidate sequences optimized for binding affinity, stability, and developability.

**But they miss something critical: Fab glycosylation risk.**

Current ML antibody design tools screen for:
- ✓ Aggregation propensity
- ✓ Chemical liabilities (oxidation, deamidation)
- ✓ Hydrophobicity patches
- ✓ Charge distribution

But they typically ignore:
- ✗ N-linked glycosylation sequons in variable regions
- ✗ Sites one mutation away from becoming glycosylated
- ✗ The manufacturing consequences of Fab glycosylation

**This creates a 79-86% gap** — the percentage of AI-designed antibodies that pass initial screens but may harbor hidden glycosylation risks that only emerge during manufacturing or clinical development.

---

## What This Scanner Does

The **Strategic Glycan Engineering Scanner** identifies *all possible glycosylation events* that could occur in an antibody sequence — essentially mimicking what happens during somatic hypermutation in human B-cells.

### Biological Context

In human B-cells, antibodies undergo **somatic hypermutation (SHM)** during affinity maturation. This natural process can:

1. **Create new N-X-S/T sequons** — introducing glycosylation sites that weren't in the germline
2. **Remove existing sequons** — eliminating glycosylation sites
3. **Modify glycan accessibility** — changing whether sites get processed

Our scanner asks: **"What glycosylation could happen to this sequence?"**

This includes:
- **Existing sites** — N-X-S/T motifs already present
- **Accessible sites** — One mutation away from N-X-S/T (SHM-accessible)
- **Location context** — CDR vs Framework, proximity to binding interface

---

## Proof of Concept: Scanner → Chromatography Prediction

This repository demonstrates a complete pipeline from sequence analysis to manufacturing risk assessment.

### Pipeline Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│  ANTIBODY SEQUENCE                                                  │
│  (AI-designed, therapeutic, or research antibody)                   │
└─────────────────────────────────────────────────────────────────────┘
                                  ↓
┌─────────────────────────────────────────────────────────────────────┐
│  STRATEGIC GLYCAN ENGINEERING SCANNER                               │
│  ───────────────────────────────────────                            │
│  • Identify existing N-X-S/T sequons                                │
│  • Find sites one mutation away (SHM-accessible)                      │
│  • Classify: CDR vs Framework location                              │
│  • Calculate SHM probability for each site                          │
│  • Prioritize by engineering/risk potential                         │
└─────────────────────────────────────────────────────────────────────┘
                                  ↓
┌─────────────────────────────────────────────────────────────────────┐
│  GLYCOFORM CLASSIFICATION                                           │
│  ───────────────────────────────────────                            │
│  • Fab vs Fc glycosylation                                          │
│  • Glycan type inference (high-mannose, complex, hybrid)            │
│  • ConA affinity multiplier calculation                             │
└─────────────────────────────────────────────────────────────────────┘
                                  ↓
┌─────────────────────────────────────────────────────────────────────┐
│  ConA CHROMATOGRAPHY MODEL                                          │
│  ───────────────────────────────────────                            │
│  • Mechanistic simulation (tanks-in-series, Langmuir kinetics)      │
│  • Predict breakthrough curves                                      │
│  • Calculate retention times and binding capacity                   │
│  • Validated against literature parameters                          │
└─────────────────────────────────────────────────────────────────────┘
                                  ↓
┌─────────────────────────────────────────────────────────────────────┐
│  MANUFACTURING RISK REPORT                                          │
│  ───────────────────────────────────────                            │
│  • LOW: No Fab glycans → Standard QC                                │
│  • HIGH-IMMUNOGENICITY: Complex Fab glycans → α-Gal/NGNA testing   │
│  • HIGH-MANUFACTURING: High-mannose Fab → Batch heterogeneity       │
│  • Recommended analytical methods                                   │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Key Scientific Insight

### Why ConA Chromatography?

Concanavalin A (ConA) is a lectin that binds α-D-mannosyl and α-D-glucosyl residues. The critical insight from literature (Oncotarget 2016, PMC5058747):

> **Fc glycans at Asn297 are internally located and INACCESSIBLE to ConA.**
> **Fab glycans are surface-exposed and ACCESSIBLE to ConA.**

This means ConA chromatography can **selectively separate** antibodies based on Fab glycosylation status:

| Antibody Type | ConA Behavior | Implication |
|---------------|---------------|-------------|
| Standard IgG (no Fab glycan) | Flows through | Normal manufacturing |
| Fab-glycosylated (high-mannose) | Strongly retained | Heterogeneity risk |
| Fab-glycosylated (complex) | Moderately retained | Immunogenicity risk |

### Validation with FDA-Approved Antibodies

| Antibody | Scanner Result | ConA Prediction | Known Reality |
|----------|---------------|-----------------|---------------|
| **Bevacizumab** | 0 Fab sites | Flow-through | ✓ No Fab glycans |
| **Cetuximab** | 2 Fab sites (HC-88, LC-41) | Retained | ✓ Known α-Gal issues |
| **Trastuzumab** | 0 Fab sites | Flow-through | ✓ No Fab glycans |

Cetuximab's Fab glycosylation at positions HC-88 and LC-41 is well-documented and causes immunogenicity issues due to α-Gal epitopes from the SP2/0 production cell line.

---

## Repository Contents

```
Strategic-Glycan-Engineering-Scanner/
│
├── Strategic_Glycan_Engineering_for_RFdiffusion.ipynb
│   └── Core scanner: identifies existing and accessible glycosylation sites
│
├── ConA_Chromatography_Model.ipynb
│   └── Mechanistic model for glycoform-selective separation
│   └── Validated parameters from literature
│   └── Breakthrough curve simulation
│
├── Scanner_Chromatography_Integration.ipynb
│   └── Complete pipeline: Scanner → Chromatography → Risk Report
│   └── Parses scanner output
│   └── Generates manufacturing risk assessment
│
├── docs/
│   ├── literature_parameters_summary.md
│   │   └── Compiled binding constants and kinetic parameters
│   │   └── Data sources and gaps
│   │
│   └── ConA_Simulation_Discussion.md
│       └── Interpretation of simulation results
│       └── Scientific validation
│
└── README.md
```

---

## Installation

```bash
# Clone the repository
git clone https://github.com/christophergaughan/Strategic-Glycan-Engineering-Scanner.git
cd Strategic-Glycan-Engineering-Scanner

# Install dependencies
pip install numpy scipy pandas matplotlib seaborn scikit-learn

# Or use requirements.txt
pip install -r requirements.txt
```

### Google Colab

All notebooks are designed to run in Google Colab. Open in Colab and run all cells (i.e., hit "Run All" in the top menu).

---

## Quick Start

### 1. Run the Scanner

```python
# In Strategic_Glycan_Engineering_for_RFdiffusion.ipynb
# Load your antibody sequence and run analysis

antibody = {
    'name': 'My_AI_Designed_mAb',
    'heavy_chain': 'QVQLVQSGAEVKKPGAS...',
    'light_chain': 'DIQMTQSPSSLSASVG...'
}

results = scanner.analyze(antibody)
```

### 2. Predict Chromatography Behavior

```python
# In Scanner_Chromatography_Integration.ipynb
# Parse scanner output and predict ConA behavior

from scanner_integration import predict_cona_behavior

prediction = predict_cona_behavior(scanner_results)
print(f"Risk Level: {prediction['risk_level']}")
print(f"ConA Retention: {prediction['cona_prediction']}")
```

### 3. Generate Risk Report

```python
# Full manufacturing risk assessment
report = generate_risk_report(results)
print(report)
```

---

## Model Parameters (Validated)

### ConA Binding Parameters

| Parameter | Value | Source |
|-----------|-------|--------|
| IgG KD | 7.2 μM | Scatchard Analysis |
| Non-specific binding | 0.25 mg/g | Cryogel study |
| Max specific binding | 6.7 mg/g | Cryogel study |
| Specificity ratio | 26.8× | Literature |
| Optimal pH | 7.4 | Multiple sources |

### Affinity Multipliers

| Glycoform | ConA K× | Rationale |
|-----------|---------|-----------|
| No Fab glycan | 0.01 | Non-specific only |
| 1 Fab site (high-mannose) | 10.0 | Strong mannose binding |
| 2 Fab sites (high-mannose) | 28.3 | Cooperative/avidity |
| 1 Fab site (complex) | 1.0 | Weak (no terminal mannose) |

---

## Use Cases

### 1. AI-Designed Antibody Screening

Before investing in experimental characterization, screen AI-generated sequences for hidden glycosylation risks.

### 2. Developability Assessment

Add glycosylation risk to your developability scoring alongside aggregation, viscosity, and chemical liabilities.

### 3. Manufacturing QC Method Development

Predict which antibodies will require ConA or boronate chromatography as part of QC.

### 4. Immunogenicity Risk Prediction

Identify antibodies with complex-type Fab glycans that may carry immunogenic epitopes (α-Gal, NGNA).

---

## Limitations & Future Work

### Current Limitations

1. **Proof of concept** — Not yet validated with experimental chromatography data
2. **Glycan type inference** — Assumes CHO-like glycosylation patterns
3. **Single resin focus** — ConA model most developed; boronate is secondary

### Planned Enhancements

1. **Hybrid ML layer** — Gaussian Process correction for model-experiment residuals
2. **Bayesian optimization** — Suggest optimal experiments for model calibration
3. **Multi-resin panel** — Add WGA, RCA, Lentil lectin models
4. **Experimental validation** — Partner with wet lab for breakthrough curve data

---

## Citation

If you use this tool in your research, please cite:

```bibtex
@software{strategic_glycan_scanner,
  author = {Gaughan, Christopher},
  title = {Strategic Glycan Engineering Scanner},
  year = {2024},
  publisher = {GitHub},
  url = {https://github.com/christophergaughan/Strategic-Glycan-Engineering-Scanner}
}
```

---

## Related Publications

1. **Oncotarget 2016** (PMC5058747) — ConA fractionation of Fab-glycosylated IgG
2. **Narayanan et al. 2021** — Hybrid mechanistic/ML models for chromatography
3. **Cytiva Application Notes** — ConA Sepharose specifications

---

## Contact

**Christopher Gaughan, PhD**  
AntibodyML Consulting LLC  
Computational Biology | Antibody Developability | ML for Biopharmaceuticals

- GitHub: [@christophergaughan](https://github.com/christophergaughan)
- LinkedIn: [Connect](https://www.linkedin.com/in/christophergaughan/)

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

*Bridging the gap between AI antibody design and manufacturing reality.*
