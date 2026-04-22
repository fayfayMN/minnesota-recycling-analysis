# Minnesota Recycling Patterns and Financial Trends

> **MIS 380 — Business Intelligence & Analytics | Section 1 | Spring 2026**  
> Feifei Li · Michelle Nguyen · Patrick M. Sanchez
> 
> **Presentation:**
>  [https://mnscu-my.sharepoint.com/:p:/r/personal/qv7121xp_go_minnstate_edu/_layouts/15/Doc.aspx?sourcedoc=%7BA4C5DC92-5DDF-478B-A7BF-C27605DD9E46%7D&file=MIS%20380%2C%20Section%201%20-%20SAP%20Presentation%203.pptx&action=edit&mobileredirect=true&wdExp=TEAMS-TREATMENT&web=1](url)
---

## Overview

This project analyzes recycling patterns, waste management behavior, and financial trends across all 86 Minnesota counties using data provided by the **Minnesota Pollution Control Agency (MPCA)**. The analysis spans 27 years of data (1991–2017) and addresses six research questions covering county-level patterns, expenditure trends, policy impacts, and a national benchmark comparison using EPA data.

---

## Research Questions

| # | Question | Method | Answer |
|---|---|---|---|
| Q1 | Are there different recycling patterns between counties for each waste method? | Stacked bar chart + ANOVA | Yes and no — metro counties dominate; patterns vary by method |
| Q2 | Are there different patterns by recycling category (paper, glass, metal, etc.)? | Heatmap + ANOVA | Yes — Paper leads statewide; Organics concentrated in metro |
| Q3 | What is the average expenditure by category by year, visually and statistically? | Line chart + ANOVA | Yes — Recycling highest ($268K avg); WTE fastest-growing |
| Q4 | Does net income impact total recycling volume? | Scatter plot + Pearson/Spearman correlation | No — r = 0.0145, p = 0.801 (no significant correlation) |
| Q5 | Do policy changes align with visible recycling trends? | Time-series + before/after comparison | Yes — +69.5% recycling after 2014; organics nearly 4× after 2015 |
| Q6 | Is Minnesota's pattern consistent with national EPA data? | Dual-line chart + Pearson correlation | Partly — same direction (r = 0.88); MN above national avg in 12/17 years |

---

## Datasets

All datasets were provided by the **Minnesota Pollution Control Agency (MPCA)** via SCORE annual reports.

| Dataset | Rows | Counties | Year Range | Key Columns |
|---|---|---|---|---|
| `Waste Management by Method` | 2,322 | 86 | 1991–2017 | Recycling, Organics, Onsite, WTE, Landfilled (tons) |
| `Recycling by Category` | 49,019 | 86 | 1991–2017 | Category, Material, Res Tons, CII Tons, Total Tons |
| `Expenditure and Revenue` | 316 | 79 | 2014–2017 | 7 expenditure categories, 3 revenue sources |

**External data source for Q6:**  
National recycling rates from the [EPA Facts and Figures on Municipal Solid Waste](https://www.epa.gov/facts-and-figures-about-materials-waste-and-recycling/national-overview-facts-and-figures-materials) — covers 1960–2018.


---

## Project Structure

```
minnesota-recycling-analysis/
│
├── data/                                 
│   ├── Waste Management by Method.xlsx
│   ├── Recycling by category.xlsx
│   └── Expenditure and Revenue.xlsx
│   └── External data source for Q6
│
├── outputs/                            
│   ├── q1_waste_methods.png
│   ├── q2_heatmap.png
│   ├── q3_expenditure.png
│   ├── q4_scatter.png
│   ├── q5_policy.png
│   ├── q6_chart1_trend.png
│   ├── q6_chart2_materials.png
│   ├── q6_chart3_longterm.png
│  
├── Minnesota_Recycling_Analysis.ipynb    # Main analysis notebook (Python)
├── .gitignore
└── README.md
```

---

## Getting Started

### Prerequisites

Make sure you have the following installed:

- Python 3.8+
- Jupyter Notebook or JupyterLab
- SAP Predictive Analysis software

### Python Dependencies

Install all required packages with:

```bash
pip install pandas numpy matplotlib seaborn scipy openpyxl
```

Or install from the requirements list individually:

```bash
pip install pandas          # Data manipulation
pip install numpy           # Numerical operations
pip install matplotlib      # Base plotting
pip install seaborn         # Statistical visualizations
pip install scipy           # Statistical tests (ANOVA, correlation)
pip install openpyxl        # Reading Excel files
```

---

## How to Run

### Option 1 — Jupyter Notebook (recommended)

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/minnesota-recycling-analysis.git
cd minnesota-recycling-analysis

# 2. Place the three .xlsx files in the /data folder

# 3. Launch Jupyter
jupyter notebook

# 4. Open Minnesota_Recycling_Analysis.ipynb and run all cells
```

### Option 2 — Python Script

```bash
# Place the .xlsx files in the same folder as the script
# Update the file paths at the top of the script if needed
python mn_recycling_analysis.py
```



---

## Key Findings

- **Metro counties dominate all waste methods.** Hennepin County alone accounts for 32.3 million total tons — more than double Ramsey in second place. ANOVA: F = 178.99, p < 0.001.
- **Paper is the most recycled material statewide** (18.9M tons, 43.5% of all recycling), consistent with national EPA data.
- **Recycling spending is the highest expenditure category** at $268K average per county in 2017. Waste-to-Energy grew nearly 9× from 2014 to 2017. ANOVA: F = 98.80, p < 0.001.
- **Net income does not predict recycling volume.** Pearson r = 0.0145, p = 0.801. R² = 0.0002. Funding structure alone does not drive recycling output.
- **2014 and 2015 policy changes produced the largest shifts in the 27-year dataset.** Statewide recycling increased +69.5% after the 2014 75% metro goal, and organics nearly quadrupled after the 2015 legislation.
- **Minnesota outperforms the national EPA average** in 12 of 17 comparable years (1991–2018), with the widest gap in 2014 (+9.5 percentage points). Correlation with national trend: r = 0.88, p < 0.001.

---

## Statistical Methods

| Method | Used For |
|---|---|
| One-way ANOVA | Testing whether county or category means differ significantly (Q1, Q2, Q3) |
| Pearson correlation | Linear relationship between net income and recycling (Q4); MN vs. EPA trend (Q6) |
| Spearman correlation | Rank-order correlation as robustness check for Q4 (handles outliers) |
| Simple linear regression | R² and slope for net income vs. recycling (Q4) |
| Before/after comparison | Mean recycling volumes before and after policy years (Q5) |

---

## Data Cleaning Steps

1. **County name standardization** — Inconsistent naming (e.g. `"Carlton (Partial)"` vs. `"Carlton - partial"`) was resolved by stripping partial-county suffixes before merging datasets.
2. **Missing value handling** — All numeric columns confirmed to contain no meaningful NaN values. Remaining NaN entries filled with `0`.
3. **Net income calculation** — Derived as: `Total Revenue (SCORE + Local + Other) − Total Expenditure (all 7 categories)`.
4. **Dataset merge (Q4)** — Inner join on `Year + County` between Expenditure and Waste Management datasets produced 304 matched county-year records covering 2014–2017.
5. **EPA methodology note** — The EPA changed food waste measurement in 2018, causing the national rate to drop from 35.2% to 32.1%. Analysis uses 2017 as the final comparison year.
   
---
## Data Visualization 
1. ** SAP Predictive Analysis software** - as part of visualization tools in this project
2.  ** Python notebook** - used for statistics findings and questions 6
   
---

## References

- Minnesota Pollution Control Agency. *SCORE Annual Reports*. Retrieved from the MIS 380 course D2L portal.
- U.S. Environmental Protection Agency. *National Overview: Facts and Figures on Materials, Wastes and Recycling*. [https://www.epa.gov/facts-and-figures-about-materials-waste-and-recycling](https://www.epa.gov/facts-and-figures-about-materials-waste-and-recycling)
- Minnesota Statutes §115A.551 — Supplementary Recycling Goals. [https://www.revisor.mn.gov/statutes/cite/115A.551](https://www.revisor.mn.gov/statutes/cite/115A.551)
- Minnesota Statutes §115A.931 — Yard Waste. [https://www.revisor.mn.gov/statutes/cite/115a.931](https://www.revisor.mn.gov/statutes/cite/115a.931)

---

## AI Collaboration Disclosure

This project used AI tools as part of the analysis workflow:

- **ChatGPT** — Baseline research and initial question framing

All AI-generated content was cross-referenced against the primary MPCA datasets and course materials by the authors to verify accuracy and eliminate hallucinations.

---

## License

This project was completed for academic purposes as part of MIS 380 at Metropolitan State University. Data belongs to the Minnesota Pollution Control Agency.
