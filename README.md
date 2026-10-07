# Reproducing Nakamae et al. 2025 — CBE RNA off-target risk prediction

A partial reproduction of the tissue-specific RNA off-target risk analysis from:

> Nakamae K, Suzuki T, Yonezawa S, Yamamoto K, Kakuzaki T, Ono H, Naito Y, Bono H.
> *Risk Prediction of RNA Off-Targets of CRISPR Base Editors in Tissue-Specific
> Transcriptomes Using Language Models.* Int J Mol Sci 2025;26(4):1723.
> [doi:10.3390/ijms26041723](https://doi.org/10.3390/ijms26041723)

Original code by the authors, MIT licensed:
[PiCTURE](https://github.com/KazukiNakamae/PiCTURE) ·
[PROTECTiO](https://github.com/KazukiNakamae/PROTECTiO).
Fine-tuned models: [STL](https://huggingface.co/KazukiNakamae/STLmodel) ·
[SNL](https://huggingface.co/KazukiNakamae/SNLmodel).

**This is a reproduction, not original research.** The pipeline, models and data
are the authors'. What is mine is the verification: re-running their released
analysis and checking whether the published claims follow from it.

Reproduced against PROTECTiO commit `[FILL: hash from section 1]`.

## Scope

| Tier | Status |
|---|---|
| Tissue-specific ESD analysis (Fig. 5) | reproduced |
| Loading and running STL/SNL classifiers | reproduced |
| ACW/WCW/NNN motif classifiers reimplemented and verified | reproduced |
| Fine-tuning Tables 1–2 | **not reproduced** — splits not in the public repos |
| PiCTURE from raw RNA-seq | **not attempted** — 13 SRA datasets, STAR + GATK on hg38 |

The last row is the honest ceiling. This reproduces the downstream risk analysis
starting from the authors' released intermediate data, not the pipeline that
generated it.

## What I found

[FILL: did the claim checks pass? e.g. "Brain, ovary and heart reproduced as
significantly lower ESD across all four classifiers; colon highest, consistent
with Figure 5." And any MISS, with your explanation.]

![Tissue ESD](results/reproduced_figure5_tissue_esd.png)

Per-tissue means, Welch's two-tailed t-tests against the reference group:
[`results/reproduced_tissue_esd_stats.csv`](results/reproduced_tissue_esd_stats.csv)

## Methodological notes

Two things in the published tables are worth more than the headline numbers.

**The NNN baseline earns its place.** In the paper's Table 1, a classifier that
predicts "positive" for everything reaches 0.766 accuracy — above the WCW motif
classifier and within 0.037 of their STL model — purely because positives
outnumber negatives 3.1:1. Its precision is 0.383. Including that row is what
exposed the first dataset and drove the rebuild for Table 2, where NNN accuracy
correctly falls to 0.500.

**Table 1 and Table 2 accuracies are not comparable.** The SNL model's 0.726
looks worse than the STL model's 0.803 and is the better result: Table 2 uses a
balanced, de-duplicated test set while Table 1's sets are imbalanced and overlap
between train and test. On the harder evaluation the SNL model beats WCW on every
metric, by roughly 0.009 — with ~20,500 test sequences the standard error on
accuracy is around 0.003, so the gap is likely real but small.

(Both tables are the authors' published values, reproduced here for context only.)

## Running it

Open `reproduction.ipynb` in Colab with a T4 GPU and Run All. Sections 1–4 need
no GPU; sections 5–6 load the fine-tuned models.

## Course context

Transcriptomics in Bioinformatics (BINF6430), Northeastern University.
Group 4: Jahnavi Manjunath Chowdhary, Lakshmikanth Reddy Chavva, Lavanya Suresh,
Mohan Yashaswi Sreepada, Nisha Madavaprasad.

[FILL: one line on who did what, or "reproduction work in this repository is my own".]

## License

Analysis code in this repository: MIT. The reproduced pipeline and models remain
under their authors' MIT license and should be cited as above.
