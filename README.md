# Learning Ligand Preferences for Human Acetylcholinesterase

This project started because I liked molecular docking and wanted to do something that reminded me of it, but from a machine learning angle.

I kept thinking about whether a model trained on known ligands for one protein could learn the chemical profiles that protein appeared to favor. I was also interested in what would happen when the model was given compounds outside of the dataset it learned from: would its predictions still make sense, and would the compounds it favored also produce convincing docking poses?

I first thought about doing this for several proteins, but building the full workflow for one target was already much more involved than I expected. I focused on human acetylcholinesterase (AChE) and treated the project as a larger continuation of an earlier HIV protease machine learning project.

The final study includes QSAR model comparison, scaffold-aware validation, external compound screening, molecular docking, and pose geometry analysis. The point was not to replace docking or prove that any compound is an AChE inhibitor. I mostly wanted to see what the models learned, how well that held up outside of a random split, and whether the highest-priority external compounds remained interesting after docking.

## What I did

Bioactivity data for human AChE were collected from ChEMBL and standardized before modeling. After removing duplicates and unusable records, the final dataset contained 5,830 unique ligands.

Several model types were tested, including fingerprint-based tree models, descriptor models, and graph neural networks. A random forest using Morgan fingerprints performed best overall, so it was used for the external screen.

The selected model was then trained on the complete curated dataset and applied to compounds from a Broad Repurposing Hub sample. Compounds were filtered using model uncertainty, similarity to the training domain, and basic chemical profile checks before docking.

For the structure-based portion, docking was performed against human AChE using PDB structure 4EY7, chain A. The docking setup was checked by redocking donepezil into its crystallographic site. The redocked pose had an RMSD of 0.905 Å.

The final candidates were not ranked by one number. I considered predicted pIC50, random forest uncertainty, similarity to the training set, docking score, binding geometry, and consistency across multiple poses.

## Main results

The random forest reached an R² of 0.703 on a random test split with an RMSE of about 0.715 pIC50 units. Performance dropped to an R² of 0.321 on a scaffold split, which was expected to be the harder and more realistic test.

The external screen began with 22,612 records. After structure standardization, duplicate removal, removal of training-set overlaps, and applicability filtering, 1,531 compounds remained eligible for prioritization.

The final ranking was:

1. Irbesartan
2. FK-866
3. RS-67506
4. VU0238429
5. ML277
6. Lomerizine
7. Efletirizine
8. ML141

Irbesartan had the strongest overall combination of model support, low random forest uncertainty, docking score, and stable dual-site geometry. ML141 received the highest model prediction of the eight, but it was deprioritized because its uncertainty was high, its docking was weaker, and its best-pose geometry did not agree well with the rest of its pose ensemble.

## Repository contents

```text
ache-ligand-learning/
├── notebooks/
│   └── AChE_QSAR_and_Docking.ipynb
├── results/
│   ├── model_performance.csv
│   ├── external_pose_ensemble_stability.csv
│   └── final_candidate_prioritization.csv
├── data/
│   └── README.md
├── models/
│   └── README.md
├── requirements.txt
├── .gitignore
└── LICENSE
```

The notebook was developed in Google Colab and uses Google Drive checkpoints so longer steps can be resumed after a runtime disconnect. Some paths still point to the original Drive workspace and should be changed before running the notebook under a different account.

Large downloaded datasets, trained model files, prepared receptor files, and full docking pose folders are not included in this repository. The notebook contains the code used to generate them, while the smaller final result tables are included.

## Running the notebook

The easiest way to run the project is in Google Colab.

1. Upload the notebook from `notebooks/`.
2. Mount Google Drive when prompted.
3. Change the project directory near the beginning of the notebook.
4. Run the sections in order.

Several stages restore saved files when they already exist. This was intentional because ChEMBL collection, model fitting, and docking can take longer than one Colab session.

## Interpretation

This is a computational, hypothesis-generating study. Predicted pIC50 values and docking scores do not confirm biological activity. The scaffold-split result also shows that the model is much less reliable on unfamiliar chemical scaffolds than the random split alone would suggest.

The final compounds are candidates for follow-up, not experimentally validated AChE inhibitors.
