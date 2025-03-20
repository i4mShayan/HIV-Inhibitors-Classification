# HIV Inhibitors Classification

HIV inhibitor molecules are special compounds that block the virus from growing and spreading. They work by targeting key proteins in the virus, such as those needed for entry, copying genetic material, or making new virus particles. By stopping these steps, the molecules help control HIV infection and keep the immune system strong.

![Protease-Inhibitors-prevent-new-HIV-from-becoming-a-mature-virus-that-can-infect-other-CD4-Cells](https://github.com/user-attachments/assets/ac80b9ac-5ca9-4f51-8a8b-abb3a68e5a91)
> This image shows how protease inhibitors stop HIV from maturing. Normally, HIV leaves a CD4 cell as an immature virus, and the protease enzyme processes its proteins to make it infectious. Protease inhibitors block this step, preventing HIV from maturing and stopping its spread in the body.

In this project, we have developed multiple Graph Neural Network (GNN) models with different message-passing mechanisms to classify whether a given molecule is an HIV inhibitor or not. These models analyze molecular structures as graphs, learning relationships between atoms and bonds to identify potential drug candidates for HIV treatment.

We have used the HIV dataset available at [MoleculeNet](https://moleculenet.org/datasets-1).

## Table of Contents
- [Feature Extraction Pipeline](#feature-extraction-pipeline)
  - [1. Get Graph Representation of Molecules](#1-get-graph-representation-of-molecules)
  - [2. Get Node and Edge Features](#2-get-node-and-edge-features)
- [Distribution and Split Analysis](#distribution-and-split-analysis)
- [Models and Training Config](#models-and-training-config)
- [Results and Analysis](#results-and-analysis)
- [Future Work](#future-work)
- [References](#references)

## Feature Extraction Pipeline

First, we converted the SMILES text to molecular structures, and then transformed these structures into graph representations. We convert molecular structures into graph representations and extract detailed features from both nodes (atoms) and edges (bonds) to capture the chemical characteristics essential for classification.

### 1. Get Graph Representation of Molecules

| Sample 1 | Sample 2 |
| ------------------------ | ------------------------ |
| ![Graph Rep 1](https://github.com/user-attachments/assets/d4e236b6-5a20-4d43-ace6-7f831d6f2448) | ![Graph Rep 2](https://github.com/user-attachments/assets/10886c1b-cf53-4db0-a580-8e22d7c999d2) |

### 2. Get Node and Edge Features

Each molecule is represented as a graph where nodes (atoms) have chemical properties and edges (bonds) indicate connectivity. The table below provides a quick reference of the extracted features used for training the models.

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

Some examples with 3 randomly selected nodes and edges along with their features:

| ![Example Node 1](https://github.com/user-attachments/assets/d785f8d6-c0fc-4900-b66e-4c927bc9d622) | ![Example Node 2](https://github.com/user-attachments/assets/465d6c46-c39b-4bed-be57-147b9fae13bb) |
|-----------|-----------|
| ![Example Edge 1](https://github.com/user-attachments/assets/1b43f136-b94e-494c-89d9-f4eaf94e412d) | ![Example Edge 2](https://github.com/user-attachments/assets/04377b34-388d-40f0-9908-306f387bbf90) |

## Distribution and Split Analysis

We analyzed the distribution of our dataset to ensure that the training and test sets share similar chemical spaces. This helps in building models that generalize well to unseen data.

| Overall Dataset Distribution <br> ![Overall](https://github.com/user-attachments/assets/4070c12e-1e9c-4d49-b63c-146e32450f9d) | Train Set Distribution <br> ![Train](https://github.com/user-attachments/assets/55c2d46e-4b7b-40f2-9107-6f3f0247fb84) |
|---------------------------------------------------------------|----------------------------------------------------------|

| Balanced Train Set Distribution <br> ![Balanced](https://github.com/user-attachments/assets/1c499258-f6b1-4f87-ad4c-d5ea0487eb3a) | Test Set Distribution <br> ![Test](https://github.com/user-attachments/assets/3fbe5c86-18a6-4c86-baf4-f68f3bbc7a25) |
|---------------------------------------------------------------|----------------------------------------------------------|

![Scatter Plot](https://github.com/user-attachments/assets/f1379d35-912f-4517-b222-9a3a1b257a3d)
> **(a) Molecular Weight vs. Aqueous Solubility (logS):**  
> This plot shows the distribution of molecular weight and aqueous solubility, demonstrating a consistent chemical space across the training and test sets, with a few outliers in the test data.  
>
> **(b) Principal Component Analysis (PCA):**  
> PCA was applied to molecular descriptors, with the first two components capturing nearly all the variance. The overlapping clusters confirm that the test set is well-represented within the training set.

## Models and Training Config

We tested four nearly identical models, differing only in their message passing mechanisms (GCN, GAT, GIN, and TransformerConv) to compare their strengths for the HIV inhibitor classification task. All models were trained using the same configuration, ensuring a fair comparison of the message passing methods.

```python
config = {
    "epochs": [30],
    "batch_size": [128],
    "learning_rate": [0.05],
    "weight_decay": [0.0005],
    "sgd_momentum": [0.8],
    "scheduler_gamma": [0.95],
    "pos_weight": [1.3],
    "model_embedding_size": [128],
    "model_attention_heads": [4],
    "model_layers": [3],
    "model_dropout_rate": [0.2],
    "model_top_k_ratio": [0.5],
    "model_top_k_every_n": [1],
    "model_dense_neurons": [256],
    "ignore_edge_features": [False],
    "model_edge_dim": [2],
}
```

## Results and Analysis

We evaluated the performance of each model using key metrics such as accuracy, precision, recall, and F1 score. The table below summarizes the results:

| **Method**          | **Accuracy** | **Precision** | **Recall** | **F1 Score** |
|---------------------|--------------|---------------|------------|--------------|
| **GCN**             | 0.7645       | 0.7580        | 0.7831     | 0.7704       |
| **GAT**             | **0.7726**   | **0.7628**    | **0.7914** | **0.7768**   |
| **GIN**             | 0.6812       | 0.6877        | 0.6638     | 0.6755       |
| **TransformerConv** | 0.7581       | 0.7526        | 0.7689     | 0.7607       |

- **GCN:** Offers balanced performance with strong accuracy and F1 score.
- **GAT:** Achieves the best results across all metrics, thanks to its effective attention mechanism.
- **GIN:** Shows lower performance, suggesting further tuning is required.
- **TransformerConv:** Performs competitively, though slightly below GAT.

## Future Work

- [ ] Explore using Differentiable Pooling (e.g., SAGPooling) as an alternative to the TopK Pooling mechanism.
- [ ] Investigate more effective chemical features for both nodes and edges.
- [ ] Experiment with more advanced GNN architectures and perform comprehensive hyperparameter tuning.

## References

[1] [Classiﬁcation of HIV‑1 Protease Inhibitors by Machine Learning Methods](https://pubs.acs.org/doi/epdf/10.1021/acsomega.8b01843)

[2] [Graph Neural Network for Molecular Structure: Application in HIV Inhibitor Molecule Prediction](https://osf.io/preprints/osf/c3mqy_v1)
