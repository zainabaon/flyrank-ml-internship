# Refresh Priority: Ranking Content for Review Using Search Signals

**📄 [Read the live research paper →](https://zainabaon.github.io/flyrank-ml-internship/)**

A completed capstone from the **FlyRank ML Internship** — Applied Search Intelligence track (Lane 2: Refresh / Content Opportunity Scoring).

---

## What this is

A decision-support system that ranks content pages by refresh priority, built and validated on real anonymized FlyRank search performance data (up to 79M rows in the full warehouse release). A transparent, hand-written baseline rule is compared against three ML models (Logistic Regression, Decision Tree, Random Forest) under a client-holdout validation split, with explicit leakage audits at every stage — including a self-audit that re-tested the whole pipeline under both a naive and an honest split to check the result wasn't an artifact of leaked validation data.

## Key result

Logistic Regression beat the transparent baseline rule on Precision@50 (**0.660 vs. 0.560**) — while a more complex Random Forest did not, showing that added model complexity doesn't automatically translate into a better ranked output. This held up under a repeated, honest before/after validation check.

The final output is a **decision-support ranking, not an automated fix**: 17 real pages flagged for human review, each with a reason code, a confidence note, and an explicit no-go list for what should never be automated from it.

---

## Repo structure

| Path | What it is |
|---|---|
| `docs/` | The deployed research paper (served via GitHub Pages) |
| `work/notebooks/` | Every weekly notebook — research framing → data contract → baseline → model → validation audit → action playbook → capstone summary |
| `work/outputs/` | Generated queues and metrics (regenerated fresh on each notebook run; data files are deliberately not committed) |
| `submission/paper_url.txt` | One line — the exact live URL of the deployed paper |
| `data/raw/` | The small anonymized starter dataset (~30k pages) used for baseline/model iteration |
| `scripts/` | The original reference pipeline (prepare → baseline → train → evaluate → PDF) |
| `skills/` | Instruction library used by AI assistants while building this project |

## The notebooks, in order

| Week | Notebook | Covers |
|---|---|---|
| 1 | `w01_research_question.ipynb` | Lane choice, research question, initial evidence from the starter data |
| 2 | `w02_ml_task_framing.ipynb` | ML task type (scoring/ranking), target/proxy, success metric |
| 3 | `w03_data_contract.ipynb` | Real warehouse data contract (Hugging Face + DuckDB), verification queries, a deliberate leakage trap |
| 3 | `w03_feature_leakage_check.ipynb` | Feature vector build, leakage hunt on the final feature set |
| 4 | `w04_signal_audit.ipynb` | Distribution checks, four signal tests, one flag-linked test |
| 4 | `w04_baseline_score.ipynb` | Transparent baseline rule, signal checks, ranked queue, full top-17 review |
| 5 | `w05_model.ipynb` | Logistic Regression / Decision Tree / Random Forest vs. baseline, client-holdout split |
| 6 | `w06_validation_audit.ipynb` | Honest-split before/after, leakage audit, claim rewrite, audit of two published FlyRank paper findings |
| 7 | `w07_action_playbook.ipynb` | Ranked actions, human-review rules, no-go list, monitoring/retrain triggers, paper exports |
| 8 | `capstone.ipynb` | Full summary mirroring the deployed paper — question, data, methodology, results, limitations, recommendations |

## Reproducing this work

Every notebook runs top to bottom in Google Colab with zero local setup — each one clones this repo and installs what it needs automatically. Open any notebook above via Colab's **File → Open notebook → GitHub tab**, paste this repo's URL, and run all cells.

Prefer local?
```bash
git clone https://github.com/zainabaon/flyrank-ml-internship
cd flyrank-ml-internship
pip install -r requirements.txt
python scripts/run_all.py
```

---

## Data safety

- Only anonymized/pseudonymized data is used throughout — no client names, domains, raw URLs, titles, or keywords appear anywhere in this repo or the deployed paper.
- FlyRank's own product decision fields (`health_score`, `priority_score`, `action_type`, refresh flags) were never available and never used as features or labels.
- Every claim in the paper uses careful language — observed, measured, directional, decision-support — never a claim to have reverse-engineered a search ranking algorithm or proven causation.
- CI (`data-leak-check`) automatically blocks any commit that includes a raw dataset or an unrun/output-less notebook.

---

## Acknowledgments

Built on the **FlyRank ML Internship dataset** — [flyrank.ai](https://flyrank.ai)

---

*A completed project from the FlyRank ML Internship, Applied Search Intelligence track. Code under MIT (see `LICENSE`); data under `DATA_USE.md`.*
