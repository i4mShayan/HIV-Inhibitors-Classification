# HIV Inhibitors Classification
HIV inhibitor molecules are special compounds that block the virus from growing and spreading. They work by targeting key proteins in the virus, such as those needed for entry, copying genetic material, or making new virus particles. By stopping these steps, the molecules help control HIV infection and keep the immune system strong.

![Protease-Inhibitors-prevent-new-HIV-from-becoming-a-mature-virus-that-can-infect-other-CD4-Cells](https://github.com/user-attachments/assets/ac80b9ac-5ca9-4f51-8a8b-abb3a68e5a91)
> This image shows how protease inhibitors stop HIV from maturing. Normally, HIV leaves a CD4 cell as an immature virus, and the protease enzyme processes its proteins to make it infectious. Protease inhibitors block this step, preventing HIV from maturing and stopping its spread in the body.

In this project, we have developed multiple Graph Neural Network (GNN) models with different message-passing mechanisms to classify whether a given molecule is an HIV inhibitor or not. These models analyze molecular structures as graphs, learning relationships between atoms and bonds to identify potential drug candidates for HIV treatment.

# Feature Extraction Pipeline
## 1. Get Graph Representation of Molecules
![image](https://github.com/user-attachments/assets/d4e236b6-5a20-4d43-ace6-7f831d6f2448)
![image](https://github.com/user-attachments/assets/10886c1b-cf53-4db0-a580-8e22d7c999d2)


## 2. Get Node and Edge Features
![image](https://github.com/user-attachments/assets/036479e7-f6de-4e88-8918-b266dbea4586)
![image](https://github.com/user-attachments/assets/e5023e01-5c82-4539-8eb6-73888ac9e023)

# Dataset Split
Total Distribution of Dataset:


# TODO:
Using Differentiable Pooling (e.g. SAGPooling) instead of TopK Pooling mechanism.
