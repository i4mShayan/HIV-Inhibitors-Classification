# HIV Inhibitors Classification
HIV inhibitor molecules are special compounds that block the virus from growing and spreading. They work by targeting key proteins in the virus, such as those needed for entry, copying genetic material, or making new virus particles. By stopping these steps, the molecules help control HIV infection and keep the immune system strong.

![Protease-Inhibitors-prevent-new-HIV-from-becoming-a-mature-virus-that-can-infect-other-CD4-Cells](https://github.com/user-attachments/assets/ac80b9ac-5ca9-4f51-8a8b-abb3a68e5a91)
> This image shows how protease inhibitors stop HIV from maturing. Normally, HIV leaves a CD4 cell as an immature virus, and the protease enzyme processes its proteins to make it infectious. Protease inhibitors block this step, preventing HIV from maturing and stopping its spread in the body.

In this project, we have developed multiple Graph Neural Network (GNN) models with different message-passing mechanisms to classify whether a given molecule is an HIV inhibitor or not. These models analyze molecular structures as graphs, learning relationships between atoms and bonds to identify potential drug candidates for HIV treatment.


# Feature Extraction Pipeline

## 1. Get Graph Representation of Molecules

| Sample 1 | Sample 2 |
| ------------------------ | ------------------------ |
| ![Graph Rep 1](https://github.com/user-attachments/assets/d4e236b6-5a20-4d43-ace6-7f831d6f2448) | ![Graph Rep 2](https://github.com/user-attachments/assets/10886c1b-cf53-4db0-a580-8e22d7c999d2) |

## 2. Get Node and Edge Features
Each molecule is represented as a graph where nodes (atoms) have chemical properties and edges (bonds) indicate connectivity. See the table below for a quick reference of extracted features.

| **Feature**                | **Type** | **Description**                                                                                          | **Possible Values / Notes**                                  |
|----------------------------|----------|----------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| **Atomic Number**          | Node     | The atomic number of the atom as provided by RDKit.                                                    | e.g., 1 for Hydrogen, 6 for Carbon, etc.                     |
| **Period**                 | Node     | The period (row) of the element in the periodic table, obtained via the periodictable library.            | Positive integers (1, 2, 3, …)                               |
| **Group**                  | Node     | The group (column) of the element in the periodic table.                                               | Positive integers (1, 2, 3, …)                               |
| **Combined Periodic Index**| Node     | Computed as `(Period - 1) * 18 + (Group - 1)`, providing a unique index based on period and group.       | Non-negative integer; -1 if period/group info is missing     |
| **Atom Degree**            | Node     | The number of bonds connected to the atom.                                                              | Integer (e.g., 1, 2, 3, …)                                   |
| **Formal Charge**          | Node     | The formal charge of the atom.                                                                           | Integer (0, +1, -1, etc.)                                    |
| **Hybridization**          | Node     | The hybridization state of the atom, mapped to an integer code.                                           | 0: sp, 1: sp2, 2: sp3, -1: unknown                           |
| **Aromaticity**            | Node     | Indicates if the atom is part of an aromatic system.                                                    | 0 (non-aromatic) or 1 (aromatic)                             |
| **Total Hydrogens**        | Node     | Total number of hydrogen atoms attached to the atom.                                                    | Integer                                                    |
| **Radical Electrons**      | Node     | Number of unpaired (radical) electrons on the atom.                                                     | Integer                                                    |
| **In Ring**                | Node     | Indicates whether the atom is part of a ring structure.                                                 | 0 (not in ring) or 1 (in ring)                               |
| **Chirality**              | Node     | The chirality tag of the atom as assigned by RDKit.                                                     | RDKit chirality tag (e.g., CHI_UNSPECIFIED, etc.)            |
| **Bond Type**              | Edge     | Represents the bond type as a floating point value (e.g., 1.0 for single, 2.0 for double bonds).           | Float (e.g., 1.0, 2.0, 3.0; aromatic bonds ~1.5)             |
| **Bond in Ring**           | Edge     | Indicates whether the bond is part of a ring.                                                           | 0.0 (not in ring) or 1.0 (in ring)                           |

Some examples with 3 random selected nodes and edges with their features:

| ![Feature 1](https://github.com/user-attachments/assets/c7edce2d-aa65-41e8-bc48-5472479fb840) | ![Feature 2](https://github.com/user-attachments/assets/3ad5b698-fc0c-4be4-8bcd-bc06f1c662b8) |
|-----------|-----------|
| ![Feature 3](https://github.com/user-attachments/assets/f145f78a-f6d0-4d4f-b2b0-43d9361da618) | ![Feature 4](https://github.com/user-attachments/assets/d8ce294a-7d1d-4aa3-b6b2-a17a9d5d753d) |




# Dataset Split
![image](https://github.com/user-attachments/assets/f1379d35-912f-4517-b222-9a3a1b257a3d)
> **(a) Molecular Weight vs. Aqueous Solubility (logS)**: The first plot presents the scattered distribution of molecular weight (MW) and aqueous solubility (logS) for both the training and test sets. MW and logS are global molecular descriptors that help define the chemical space. The two sets largely overlap, indicating a consistent distribution of chemical properties between training and test data. However, a few test set molecules appear at extreme values, suggesting the presence of structurally unique compounds.

> **(b) Principal Component Analysis (PCA)**: To further investigate the chemical space, Principal Component Analysis (PCA) was applied to a set of molecular descriptors. The first two principal components (PC1 and PC2) explain most of the variance in the dataset. PC1 accounts for 94.00% of the variance, while PC2 explains 5.95%, giving a total variance coverage of 99.95%. The training and test sets nearly completely overlap in this space, confirming that the test set is well-represented within the training set’s chemical space.


# TODO
- [ ] Using Differentiable Pooling (e.g. SAGPooling) instead of TopK Pooling mechanism.
- [ ] Research More On the most effective chemical features for nodes and edges
- [ ] Experimenting with more advanced GNN architectures and hyperparameter tuning

# Refrences
[1] [Classiﬁcation of HIV‑1 Protease Inhibitors by Machine Learning Methods](https://pubs.acs.org/doi/epdf/10.1021/acsomega.8b01843)

[2] [Graph Neural Network for Molecular Structure: Application in HIV Inhibitor Molecule Prediction](https://osf.io/preprints/osf/c3mqy_v1)
