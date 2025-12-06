# Arousal-Driven Serial Dependence: The Role of Internal States in Temporal Judgments

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

**MSense Lab**

This repository contains data and code for the manuscript "Arousal-Driven Serial Dependence: The role of Internal States in Temporal Judgments."

---

## Abstract

Emotional experience shapes not only how we perceive the present but also how our past influences our judgments. In this study, we investigated how emotional arousal and valence impact serial dependence in time perception—a phenomenon where previous experiences influence current judgments. Participants viewed affective images to induce either high-arousal (positive or negative) or low-arousal (neutral) emotional states. We observed that participants consistently overestimated durations during high-arousal conditions compared to low-arousal conditions. Importantly, serial dependence effects were significantly stronger during high-arousal states. This effect was most pronounced when high-arousal trials were preceded by similar high-arousal trials, showing a state-dependent increase in sequential bias. Conversely, emotional valence did not significantly affect either temporal distortion or serial dependence. These results indicate that emotional arousal plays a crucial role in how the brain integrates past and present time perceptions, providing new insights into how emotional states influence temporal cognition and serial dependence.

---

## Repository Structure

```
.
├── data_ana.ipynb              # Main analysis notebook
├── Results/                    # Processed data files
│   ├── AllSubCleanBackData1.csv
│   ├── AllSubCleanBackData2.csv
│   ├── AllSubCleanBackData3.csv
│   ├── AllSubCleanBackData-1.csv
│   └── AllSubCleanBackData-2.csv
├── Figures/                    # Individual figure outputs
│   ├── Valence_CT_Error_bar.png
│   ├── Res_CT_bar.png
│   ├── GB_CT_bar.png
│   ├── SD_point.png
│   ├── SD_bar.png
│   ├── GB_bar.png
│   ├── nBack_currentHigh.png
│   └── nBack_currentLow.png
├── manuscript/
│   └── figures/               # Composite manuscript figures
│       ├── paradigm.png
│       ├── image1.png
│       └── image2.png
└── README.md
```

---

## Requirements

### Python Version

- Python 3.8 or higher

### Dependencies

```bash
pip install -r requirements.txt
```

Required packages:

```
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
seaborn>=0.11.0
scipy>=1.7.0
statsmodels>=0.13.0
pingouin>=0.5.0
pillow>=8.0.0
jupyter>=1.0.0
```

---

## Installation

1. Clone this repository:

```bash
git clone https://github.com/yourusername/emotion-and-serial-dependence.git
cd emotion-and-serial-dependence
```

2. Install required packages:

```bash
pip install -r requirements.txt
```

3. Launch Jupyter Notebook:

```bash
jupyter notebook data_ana.ipynb
```

---

## Usage

### Running the Complete Analysis

The main analysis notebook (`data_ana.ipynb`) contains 37 cells organized into four sections:

1. **Setup and Data Loading** (Cells 0-5)

   - Import libraries and configure environment
   - Define color palettes (colorblind-safe, B&W-compatible)
   - Load and preprocess data
2. **Figure 1: Central Tendency Effects** (Cells 6-15)

   - Mixed-effects models for central tendency
   - Bar plots for CT index and general bias
   - Statistical tests (RM-ANOVA, post-hoc)
3. **Figure 2: Serial Dependence Effects** (Cells 16-32)

   - Mixed-effects models for serial dependence
   - N-back analysis across multiple trial lags
   - Statistical tests for SD effects
4. **Composite Figures for Manuscript** (Cells 33-36)

   - Combine individual figures into publication-ready layouts
   - Generate `image1.png` and `image2.png`

---

## Data Description

### Data Files

The `Results/` directory contains preprocessed trial-level data from 21 participants:

- **AllSubCleanBackData1.csv**: Main dataset with n=1 back information
- **AllSubCleanBackData2.csv**: Data with n=2 back information
- **AllSubCleanBackData3.csv**: Data with n=3 back information
- **AllSubCleanBackData-1.csv**: Data with n=-1 (future) trial information
- **AllSubCleanBackData-2.csv**: Data with n=-2 (future) trial information

### Key Variables

| Variable            | Description                                 |
| ------------------- | ------------------------------------------- |
| `SubID`           | Participant ID (1-21)                       |
| `BlockType`       | Emotional valence (Positive, Negative)      |
| `TrialType`       | Emotional arousal (High, Low)               |
| `TimeDur`         | Target duration to reproduce (0.8-1.6s)     |
| `PressTimeError`  | Reproduction error (s)                      |
| `Condition`       | Prior-current arousal code (HH, HL, LH, LL) |
| `nBack_Time`      | Duration from n-back trial                  |
| `nBack_TrialType` | Arousal level of n-back trial               |
| `nBack`           | Trial lag (-2, -1, 1, 2, 3)                 |

### Data Preprocessing

Data has been cleaned to remove:

- Time outliers (`TimeOutlier != 1`)
- Reaction time outliers (`RT_Outlier != 1`)
- Unresolved time judgments (`TimeUnRes != 1`)
- Invalid error values (`PressTimeError != 9999`)

---

## Analysis Pipeline

### 1. Central Tendency Analysis

**Model**: Mixed-effects regression

```python
PressTimeError ~ TimeDur * BlockType * TrialType + (1|SubID)
```

**Key finding**: Negative slope indicates regression to the mean

**Centering**: `TimeDur` centered at 1.2s (midpoint) for intercept interpretation

### 2. Serial Dependence Analysis

**Model**: Mixed-effects regression

```python
PressTimeError ~ nBack_Time * BlockType * TrialType * Condition_Prior + (1|SubID)
```

**Key finding**: Positive slope indicates attraction to previous duration

**Centering**: `nBack_Time` centered at 1.2s for general bias estimation

### 3. N-back Analysis

**Model**: Linear regression for each lag separately

```python
PressTimeError ~ nBack_Time
```

**Note**: `nBack_Time` is **NOT centered** (each lag analyzed independently)

---

### Statistical Tests

All statistical analyses use:

- **Mixed-effects models**: `statsmodels.formula.api.mixedlm`
- **Repeated measures ANOVA**: `pingouin.rm_anova`
- **Post-hoc tests**: `pingouin.pairwise_tests` with Bonferroni correction
- **One-sample t-tests**: `pingouin.ttest`

Significance levels:

- \* p < 0.05
- \*\* p < 0.01
- \*\*\* p < 0.001

---

## Key Results

### Central Tendency

- **Effect of arousal**: Significant (p < 0.01)
- **Effect of valence**: Not significant
- High arousal → more negative general bias (overestimation)

### Serial Dependence

- **Main effect of current arousal**: Significant (p < 0.013)
- **Interaction (prior × current arousal)**: Significant (p < 0.013)
- Strongest SD when high arousal followed by high arousal

### N-back Effects

- **n-1 (immediately preceding trial)**: Strongest effect (p < 0.001)
- **n-2, n-3**: Weaker but significant effects
- **Future trials (n+1, n+2)**: No significant effects

---

## License

This project is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

## Reproducibility

This repository is designed for full reproducibility of the manuscript figures and statistical analyses. All code has been tested with Python 3.8+ on macOS and Linux systems.

### Version Information

- **Last updated**: December 2025
- **Notebook version**: 1.0
- **Data format**: CSV (processed)

---

**Repository maintained by MSense Lab**
