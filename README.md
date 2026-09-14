# esol-solubility-qsar
Predicting Aqueous Solubility from Molecular Structure (QSAR)
A machine learning project predicting the water solubility of small molecules directly from their chemical structure, using the ESOL (Delaney) dataset — a benchmark of 1,128 real, experimentally measured compounds.

Motivation

Aqueous solubility is one of the earliest properties checked in drug development — a molecule can be a highly effective binder to its biological target and still fail as a drug if it cannot dissolve in the body. This project explores whether simple, interpretable molecular descriptors can predict solubility accurately, and compares a linear model against a non-linear one to understand what each approach reveals about the underlying chemistry.

Dataset
Source: Delaney, J.S. (2004), "ESOL: Estimating Aqueous Solubility Directly from Molecular Structure," J. Chem. Inf. Comput. Sci.
1,128 compounds, each with a SMILES string (structure) and a measured log solubility value (mol/L)
Hosted publicly via DeepChem
Method
Featurization: Each molecule's SMILES string was parsed using RDKit and converted into 7 molecular descriptors:
Molecular weight (MolWt)
LogP (octanol-water partition coefficient — a measure of lipophilicity)
Number of hydrogen bond donors (HDonors)
Number of hydrogen bond acceptors (HAcceptors)
Topological polar surface area (TPSA)
Number of rotatable bonds (RotatableBonds)
Number of rings (RingCount)
Train/test split: 80/20 split (902 training molecules, 226 held-out test molecules), ensuring evaluation is done only on molecules the model never saw during training.
Modeling: Two models were trained and compared:
Linear Regression — a simple, fully interpretable baseline
Random Forest (100 trees) — a non-linear ensemble model capable of capturing feature interactions
Results
Model	RMSE (log solubility units)	R²
Linear Regression	1.07	0.758
Random Forest	0.80	0.864

Random Forest reduced prediction error by ~25% and explained an additional 10 percentage points of variance, indicating meaningful non-linear structure in the data that a simple linear model cannot capture.

Key findings
LogP is the dominant predictor of solubility in both models (largest coefficient in linear regression; 81.6% of feature importance in Random Forest). This is consistent with established medicinal chemistry — lipophilicity is widely known as the primary driver of aqueous solubility.
Molecular weight told two different stories depending on the model. Linear regression assigned it a near-zero coefficient, effectively ignoring it. Random Forest ranked it as the second most important feature (10.2%). This discrepancy suggests molecular weight's effect on solubility is not purely additive — it likely interacts with other structural features (e.g., high molecular weight combined with high LogP may be worse than either alone) in a way only a non-linear model can represent.
The linear model systematically underestimated the solubility of the most water-insoluble compounds (visible in the predicted-vs-actual plot as a cluster of points sitting above the ideal diagonal at the low end). This is a known limitation of linear regression at distribution extremes, and is consistent with Random Forest's improved performance.
Limitations
Only 7 descriptors were used; richer molecular representations (e.g., Morgan fingerprints, graph neural networks) could likely improve performance further.
Linear model coefficients should be interpreted cautiously, since the 7 descriptors are correlated with one another (e.g., larger molecules tend to have both higher molecular weight and more rings), which can distort individual coefficient estimates.
The dataset, while a well-established benchmark, is relatively small (1,128 compounds) and may not generalize to all chemical classes.
Tech stack

Python, RDKit, pandas, scikit-learn, matplotlib

How to run

Open esol_qsar_full.py in Google Colab (or any Python environment with the packages above installed) and run sequentially — see inline comments marking each stage of the pipeline.
