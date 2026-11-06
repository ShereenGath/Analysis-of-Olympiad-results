# Olympiad Results Analysis

Analysis of the olympiad dataset: **8,000 participants**, **10 regions**, **grades 5–11**, **12 tasks**, **590 schools**. The goal was to clean the data, quantify task difficulty, and check whether results depend on grade, region, or device.

---

## Repository structure

```
.
├── data/
│   ├── olympiad_results.csv          # Raw dataset
│   ├── olympiad_data_dictionary.csv  # Column descriptions
│   └── olympiad_results_clean.csv    # Cleaned dataset (7,928 rows)
├── notebook/
│   └── analysis-olympiad.ipynb       # Main analysis notebook
├── images/                           # Charts and figures
├── presentation/
│   ├── olympiad_summary.pptx         # 12-slide summary deck
│   └── olympiad_summary.pdf          # PDF export
├── generate_presentation.py          # Builds the PPTX summary
├── requirements.txt
└── README.md
```

## Data cleaning

| Step | Result |
|---|---|
| Raw rows | 8,037 |
| Unique `student_id` | 8,000 |
| Duplicates removed (by `student_id`) | 37 |
| Region name variants normalized | 20 → 10 |
| Sessions < 1 min removed | 72 (0.9%) |
| Final clean dataset | **7,928 participants** |
| `total_score` vs sum of `*_correct` | 0 mismatches |

Additional checks:

- No extra spaces or case variants in `student_id`, `region`, `device_type`.
- School numbers range 1–59, valid in every region.
- Grades 5–11, all present.
- `total_score` recomputed and verified.

---

## Key findings

**1. Grade is the strongest predictor of score.**
Average score rises monotonically from **4.50** in grade 5 to **6.62** in grade 11 (+2.12 points). The trend holds in all 10 regions.

**2. Regions differ only slightly.**
Regional averages range from **5.38 (Perm)** to **5.73 (Novosibirsk)** — a spread of just **0.35 points**. A stable regional ranking cannot be built from these data.

**3. Device type does not affect performance.**
Desktop 59.4%, mobile 35.5%, tablet 5.1%. Average scores: **5.53 / 5.57 / 5.60**. Tasks answered and disconnects are nearly identical across devices.

**4. Tasks 1, 2, 3 and 9 are the hardest.**
Solve rate range: **29.8%–37.8%**, versus **63.9%** for the easiest task (Task 5).

| Task | Solve rate | Avg time, s |
|---|---|---|
| Task 1 | 29.8% | 64.1 |
| Task 9 | 32.0% | 65.9 |
| Task 3 | 33.9% | 66.0 |
| Task 2 | 37.8% | 69.1 |
| Task 6 | 45.1% | 71.7 |
| Task 11 | 46.7% | 72.8 |
| Task 10 | 53.3% | 75.9 |
| Task 12 | 57.2% | 77.8 |
| Task 7 | 58.7% | 79.1 |
| Task 4 | 59.2% | 78.8 |
| Task 8 | 59.8% | 79.5 |
| Task 5 | 63.9% | 81.2 |

**5. Solve rate and average time are almost perfectly correlated (r = 0.998).**
Tasks that take longer on average have higher solve rates; fast tasks are systematically harder. This points to content difficulty, not avoidance.

**6. Skip rate is uniform (3.6%–4.3%).**
Students did not selectively skip the hardest tasks, which reinforces the case for reviewing the formulation of Tasks 1, 2, 3 and 9.

**7. 88 schools had fewer than 10 participants.**
For school rankings, only the **502 schools with ≥10 participants** should be used.

---

## Recommendations

- **Education team:** review Tasks 1, 2, 3 and 9 — difficulty and wording.
- **Education team:** consider separate tests or age-adjusted reporting for grades 5–11, since grade explains most of the variance.
- **Product team:** add an automatic check for sessions under 1 minute.
- **Product team:** keep mobile support — about one third of participants use mobile, and results match desktop.
- **Analytics:** apply a ≥10 participant threshold for school-level rankings.

---

## How to run

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run the analysis
jupyter notebook analysis-olympiad.ipynb


`requirements.txt`:

```
pandas
matplotlib
jupyter
```

## Outputs

- `data/olympiad_results_clean.csv` — cleaned dataset (UTF-8, BOM).
- `presentation/olympiad_summary.pptx` — 12-slide summary deck.
- `images/*.png` — charts used in the deck.

---

## Notes

- Sessions under 1 minute (72 total) were excluded as invalid attempts: all 12 answers were made in under 5 seconds, and average score (2.78/12) is close to random guessing.
- Regional and school comparisons are descriptive, not causal. Grade distribution differs slightly by region, which partly explains regional gaps.