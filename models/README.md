# Models

The trained model files are not committed because they are large and can be recreated from the notebook.

The final production model was a random forest trained on Morgan fingerprints:

- Radius: 2
- Fingerprint size: 2,048 bits
- Training compounds: 5,830
- Random split R²: 0.703
- Random split RMSE: approximately 0.715 pIC50
- Scaffold split R²: 0.321

The notebook saves model checkpoints to Google Drive and restores them when available.
