# Short-Form Content Addiction and Attention Fragmentation Among University Students

**Implications for Deep Learning and Academic Burnout — A Dose–Response Analysis and Acquiescence Diagnosis**

[![Data](https://img.shields.io/badge/data-CSV-blue.svg)](./Short-Form_Content_Addiction_Dataset.csv)
[![Code](https://img.shields.io/badge/code-Jupyter%20Notebook-orange.svg)](./Short_Form_Content_Addiction.ipynb)
[![Instrument](https://img.shields.io/badge/instrument-PDF-red.svg)](./Questionnaire-Short-Form-Content-Addiction.pdf)
[![Python](https://img.shields.io/badge/python-3.10%2B-green.svg)](https://www.python.org/)
[![License: CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

This repository hosts the complete data, survey instrument, and analysis code underpinning the study of how daily short-form video exposure (TikTok, Facebook/Instagram Reels, YouTube Shorts) relates to attention fragmentation, cognitive fatigue, deep learning engagement, and academic burnout among university students. It is provided for **peer review and full reproducibility**.

---

## Table of Contents

- [Study Overview](#study-overview)
- [Repository Contents](#repository-contents)
- [Dataset Description](#dataset-description)
- [Measurement Structure](#measurement-structure)
- [Variable Coding Scheme](#variable-coding-scheme)
- [Analysis Pipeline](#analysis-pipeline)
- [Reproducing the Analysis](#reproducing-the-analysis)
- [Software Requirements](#software-requirements)
- [Key Findings](#key-findings)
- [Ethics Statement](#ethics-statement)
- [Data Availability Statement](#data-availability-statement)
- [How to Cite](#how-to-cite)
- [License](#license)
- [Contact](#contact)

---

## Study Overview

The study pursues two linked objectives:

1. **Substantive.** Estimate whether self-reported daily exposure to short-form video content shows a *dose–response* relationship with maladaptive academic-cognitive outcomes (addiction symptoms, attention fragmentation, notification overload, cognitive fatigue, academic burnout) and adaptive outcomes (deep learning engagement, self-regulation).

2. **Methodological.** Diagnose whether a uniformly worded Likert battery can report strong internal-consistency reliability while failing structural validity — and quantify the degree to which **acquiescent responding (ARS)** inflates, and can even invert, the observed construct relationships.

The analysis is deliberately staged so that measurement validity is *tested* rather than assumed, with each stage conditioning the next.

---

## Repository Contents

| File | Description |
|------|-------------|
| [`Short-Form_Content_Addiction_Dataset.csv`](./Short-Form_Content_Addiction_Dataset.csv) | Raw survey export: 867 respondents × 78 columns (11 metadata/demographic + 67 Likert items). |
| [`Short_Form_Content_Addiction.ipynb`](./Short_Form_Content_Addiction.ipynb) | End-to-end analysis notebook (Python): cleaning, reliability, dimensionality, validity, acquiescence diagnostics, known-groups analysis, and publication figures. |
| [`Questionnaire-Short-Form-Content-Addiction.pdf`](./Questionnaire-Short-Form-Content-Addiction.pdf) | Full survey instrument: consent page, eligibility criteria, demographic items, and all 67 verbatim Likert items with scale anchors. |

---

## Dataset Description

- **Format:** CSV (UTF-8), one respondent per row.
- **Raw responses:** 867
- **Analytic sample:** 863 (after removing 4 respondents aged *Under 16*; filter is reversible in code).
- **Columns:** 78 total
  - **11 metadata/demographic** — `Timestamp`, `Consent`, `Age`, `Gender`, `UnivType`, `AcadLevel`, `Discipline`, `YearStudy`, `DailyTime`, `TopPlatform`, `CheckFreq`
  - **67 Likert items** — `SFCA1–16`, `AF1–12`, `NO1–5`, `CF1–5`, `DLE1–12`, `AB1–12`, `SR1–5`

All 67 items are rated on a 5-point agreement scale and fall within the valid range [1, 5]. No reverse-keyed items were used (relevant to the acquiescence analysis).

> **Privacy note.** The survey collected no personally identifiable information. Consent was recorded as a single affirmative item, and the data contain no names, emails, IP addresses, or free-text identifiers.

---

## Measurement Structure

Seven constructs were measured. Four are theorized as higher-order (reflective–reflective) constructs with lower-order facets; three are unidimensional.

| Code | Construct | Items | Theorized dimensions |
|------|-----------|:-----:|----------------------|
| **SFCA** | Short-Form Content Addiction | 16 | Compulsive Consumption (CC), Psychological Dependence (PD), Study Interference (SI), Cognitive Stimulation Dependency (CSD) |
| **AF** | Attention Fragmentation | 12 | Sustained Attention Difficulty (AD), Cognitive Switching (CS), Deep Focus Erosion (DFE) |
| **NO** | Notification Overload | 5 | Unidimensional |
| **CF** | Cognitive Fatigue | 5 | Unidimensional |
| **DLE** | Deep Learning Engagement | 12 | Reflective Learning (RL), Sustained Academic Engagement (SAE), Deep Reading & Conceptual Processing (DR) |
| **AB** | Academic Burnout | 12 | Emotional Exhaustion (EE), Academic Disengagement (AD), Reduced Academic Efficacy (RAE) |
| **SR** | Self-Regulation (moderator) | 5 | Unidimensional |

Item-to-facet and item-to-construct mappings are defined explicitly in the notebook (`dimensions`, `constructs`, and `facet_map` dictionaries) so the measurement model is fully auditable.

---

## Variable Coding Scheme

**Likert items (all 67):**

| Value | Anchor |
|:-----:|--------|
| 1 | Strongly Disagree |
| 2 | Disagree |
| 3 | Neutral |
| 4 | Agree |
| 5 | Strongly Agree |

**Categorical metadata:**

| Variable | Categories |
|----------|-----------|
| `Age` | Under 16 · 16–20 · 21–23 · 24–26 · Above 26 |
| `Gender` | Male · Female |
| `UnivType` | Public University · Private University · Other |
| `AcadLevel` | Undergraduate · Master's · MPhil · PhD |
| `YearStudy` | 1st Year · 2nd Year · 3rd Year · 4th Year · Graduate Level |
| `DailyTime` | Less than 30 minutes · 30 minutes–1 hour · 1–2 hours · 2–4 hours · More than 4 hours |
| `TopPlatform` | TikTok · Facebook Reels · Instagram Reels · YouTube Shorts · Other *(multi-select)* |
| `CheckFreq` | Never · Rarely · Sometimes · Often · Very Often · Always |

Column renaming, whitespace stripping, and the eligibility filter are applied transparently in the notebook (Cells 2 and 4) and are reproducible from the raw CSV.

---

## Analysis Pipeline

The notebook executes a layered protocol in which each stage conditions the next:

| Cell | Stage | Purpose |
|:----:|-------|---------|
| 1 | Data inspection | Shape, dtypes, missingness, column index |
| 2 | Cleaning & mapping | Rename columns, define construct/dimension structure, validation checks |
| 3 | Sample characteristics | Demographic profile (Table 1) |
| 4 | Eligibility & descriptives | Drop *Under 16*; item-level means, SD, skewness, kurtosis |
| 5 | Reliability | Cronbach's α, McDonald's ω, corrected item–total correlations |
| 6 | Dimensionality | KMO, Bartlett's test, parallel analysis, EFA for the 4 multidimensional constructs |
| 7 | Convergent validity | Standardized loadings, AVE, composite reliability, guided item trimming |
| 8 | Response quality | Careless-responding checks (longstring, intra-individual SD, discrimination) |
| 9 | Correlations & acquiescence | Composite scores, inter-construct correlations, theory-sign check, ARS index |
| 10 | Acquiescence control | Random-intercept common-factor model (semopy) + regression-partialling fallback; validity re-checked |
| 11 | Known-groups validity | ANOVA & Kruskal–Wallis of composites across exposure groups (robust to acquiescence) |
| 12–14 | Figures | Publication-quality figures (600 dpi, serif) |

---

## Reproducing the Analysis

1. **Clone the repository**
   ```bash
   git clone https://github.com/Alamgir-JUST/Short-Form-Content-Addiction-and-Attention.git
   cd Short-Form-Content-Addiction-and-Attention
   ```

2. **Install dependencies** (see below).

3. **Point the notebook at the local CSV.** The notebook was authored in Google Colab and reads from a Google Drive path. Replace that line with the repository copy:
   ```python
   # Original (Colab):
   # df = pd.read_csv("/content/drive/MyDrive/Short Content/Short-Form Content Addiction.csv")

   # Local:
   df = pd.read_csv("Short-Form_Content_Addiction_Dataset.csv")
   ```
   You may also skip the `drive.mount(...)` cell when running locally.

4. **Run** `Short_Form_Content_Addiction.ipynb` top to bottom. Cells are ordered and dependent, so run them sequentially.

---

## Software Requirements

- Python 3.10+
- `pandas`, `numpy`, `scipy`
- `factor_analyzer`
- `semopy`
- `matplotlib`

```bash
pip install pandas numpy scipy matplotlib factor_analyzer semopy
```

---

## Key Findings

- **Dose–response gradient.** Maladaptive constructs rise monotonically with reported daily exposure; addiction shows the largest group effect, followed by attention fragmentation, cognitive fatigue, burnout, and notification overload. The pattern holds under both ANOVA and Kruskal–Wallis.
- **Reliable but not valid.** Constructs show high internal consistency (α ≈ 0.81–0.84) yet fail structural validity: parallel analysis retains a single factor per construct and average variance extracted never exceeds ~0.31.
- **Acquiescence fingerprint.** A person-level agreement factor correlates strongly (≈ 0.59–0.89) with every construct. Partialling it out *inverts* the central relationship (e.g., addiction → deep learning moves from ≈ +0.55 to ≈ −0.49), while the group-based dose–response — which does not depend on item-level correlations — survives.

Full numerical results are produced directly by the notebook.

---

## Ethics Statement

Participation was voluntary, anonymous, and preceded by informed consent. Eligibility required current university enrolment and a minimum age of 16. No personally identifiable information was collected. The study received ethics approval under reference **No. SkillMorph/ES/2026/01(7)**.

---

## Data Availability Statement

The data, survey instrument, and analysis code supporting the findings of this study are **openly available** in this repository and are provided as supplementary files for peer review:

- **Dataset (raw and analysis-ready):** `Short-Form_Content_Addiction_Dataset.csv`
- **Survey instrument:** `Questionnaire-Short-Form-Content-Addiction.pdf`
- **Analysis code, variable definitions, and coding scheme:** `Short_Form_Content_Addiction.ipynb`

Repository: <https://github.com/Alamgir-JUST/Short-Form-Content-Addiction-and-Attention>

There are no restrictions on access. The dataset contains no personally identifiable information, so it is shared in full.

---

## How to Cite

> Hossain, A. (2026). *Short-Form Content Addiction and Attention Fragmentation Among University Students: Implications for Deep Learning and Academic Burnout* [Data set and analysis code]. GitHub. https://github.com/Alamgir-JUST/Short-Form-Content-Addiction-and-Attention

```bibtex
@misc{hossain2026shortform,
  author       = {Hossain, Alamgir},
  title        = {Short-Form Content Addiction and Attention Fragmentation Among
                  University Students: Implications for Deep Learning and Academic Burnout},
  year         = {2026},
  howpublished = {GitHub repository},
  url          = {https://github.com/Alamgir-JUST/Short-Form-Content-Addiction-and-Attention}
}
```

---

## License

- **Code** is released under the [MIT License](https://opensource.org/licenses/MIT).
- **Data and survey instrument** are released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

Please cite the work if you use any part of this repository.

---

## Contact

**Alamgir Hossain**
📧 morphskill@gmail.com
🔗 [github.com/Alamgir-JUST](https://github.com/Alamgir-JUST)

For questions about the data or analysis, please open an [issue](https://github.com/Alamgir-JUST/Short-Form-Content-Addiction-and-Attention/issues) or contact the author by email.
