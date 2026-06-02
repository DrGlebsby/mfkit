# MFKit: RDKit-based Monomer Featurizer

MFKit is a lightweight RDKit-based Python package for physically grounded and interpretable featurization of monomers for polymer informatics and Quantitative Structure–Property Relationship (QSPR) modeling.

The package converts monomer SMILES strings into descriptor vectors that can be used as input features for machine learning models, data analysis, structure–property interpretation, and monomer design.

MFKit expects monomer SMILES strings to contain exactly two polymerization sites marked by `*` symbols.

## Citation

If you use MFKit in your research, please cite the following paper:

Gleb Averochkin, Ivan Zlobin, Ivan Bespalov, Eugeny Alexandrov,  
Machine learning with physically grounded, interpretable descriptors for polymer property prediction and monomer design,  
Chemical Engineering Science, Volume 328, 2026, 123732.  
https://doi.org/10.1016/j.ces.2026.123732

## Features

MFKit computes interpretable molecular descriptors based on RDKit and custom rule-based structural analysis, including:

- classification of atoms by main-chain and side-chain membership;
- classification of atoms by predominant intermolecular interaction type, including van der Waals, dipole-dipole, ionic, hydrogen-bond donor, aromatic carbon, and aromatic heteroatom groups;
- van der Waals volumes of atom groups in the main and side chains;
- SMARTS-based functional group counts in the main and side chains;
- selected RDKit molecular descriptors;
- chemical bond statistics for the main and side chains;
- distance between polymerization sites estimated from RDKit-generated molecular geometry.

## Dependencies

- Python >= 3.10
- RDKit
- NumPy
- pandas

## Installation

Install directly from GitHub:

```bash
pip install git+https://github.com/DrGlebsby/mfkit.git
```


Quick start:
-----------

```python
# 1) Featurization

# To featurize a monomer, you need a SMILES string with two polymerization sites.
# Polymerization sites must be marked by * symbols.
smiles = '*C(C(=O)N)(C*)C'

# Import the featurize function from the mfkit package.
from mfkit import featurize

# Generate descriptors for one monomer.
print(featurize(smiles))  # Returns a dictionary of feature : value.

# You can also generate a dictionary with full human-readable descriptor names.
from mfkit import featurize_to_dict

print(featurize_to_dict(smiles))  # Returns a dictionary of full descriptor name : value.


# 2) Batch featurization

# To featurize multiple monomers, use featurize_many.
from mfkit import featurize_many

smiles_list = [
    '*C(C(=O)N)(C*)C',
    '*CC*',
    'FC(C(C(F)(F)F)(c1ccc2c(c1)C(=O)N(C2=O)c1ccc(cc1)OCCOCCOCCOc1ccc(cc1)N1C(=O)c2c(C1=O)cc(cc2)*)*)(F)F'
]

features_df = featurize_many(smiles_list)

print(features_df)  # Returns a pandas DataFrame with SMILES and descriptor columns.


# 3) Featurization from a pandas DataFrame

import pandas as pd

df = pd.DataFrame({
    'SMILES-Polymer': [
        '*C(C(=O)N)(C*)C',
        '*CC*',
        'FC(C(C(F)(F)F)(c1ccc2c(c1)C(=O)N(C2=O)c1ccc(cc1)OCCOCCOCCOc1ccc(cc1)N1C(=O)c2c(C1=O)cc(cc2)*)*)(F)F'
    ]
})

features_df = featurize_many(df['SMILES-Polymer'])

# Combine the original dataframe with calculated descriptors.
result = pd.concat(
    [df.reset_index(drop=True), features_df.drop(columns=['SMILES'])],
    axis=1
)

print(result)
```

