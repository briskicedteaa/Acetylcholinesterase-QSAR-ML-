# Data

The raw ChEMBL activity download and the external compound library are not stored here because they are larger generated/downloaded files.

The notebook retrieves and standardizes the ChEMBL records, removes duplicate structures, converts activity values to pIC50, and saves resumable copies to Google Drive.

Final dataset used for modeling:

- Target: human acetylcholinesterase
- ChEMBL target ID: CHEMBL220
- Unique curated ligands: 5,830
- Activity endpoint used for the main model: pIC50

The external screening set was derived from a Broad Repurposing Hub sample. Of 22,612 starting records, 7,488 unique compounds remained after standardization and removal of training-set overlaps. Applicability and profile filtering reduced this to 1,531 eligible compounds.
