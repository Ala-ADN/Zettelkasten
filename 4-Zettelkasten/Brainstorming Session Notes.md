**Date:** June 25, 2025  
### Goals Discussed
- **Controlling Volatility:** Exploring the idea of designing molecular analogs with a shared scent but different **vapor pressures**, which could function as top, middle, and base notes.
- **Sustainability & Efficiency:** generate sustainable and cheap alternatives to animal based fragrances.
- **Engineering Specific Scents:** Could we one day input a desired **olfactory experience** and have the system generate candidate molecules?
- **Designing for Perceptual Variance:** Can we intentionally design single molecules that are likely to be perceived differently across individuals by targeting known variances in the human **olfactory receptor (hOR) repertoire**? (Molecule O1 case study)
### Explored Methodologies & Technologies
- **Current Representation Methods:** This included traditional **symbolic fingerprints** (e.g., ECFP/Morgan fingerprints) and **Graph Neural Network (GNN) embeddings**. It was noted that these methods only describe the molecule in isolation.
- **Interaction-Centric" Embedding:** A novel, more sophisticated embedding that **combines information from both the ligand (molecule) and the protein (hOR)**. The goal would be to create a vector representation that captures the unique characteristics of the **binding interaction itself**, providing a much richer, more biologically relevant signal.
- **Modeling Receptors:** The concept of creating vector embeddings of hORs and clustering them by **homology** was proposed as a way to map the biological "logic" of scent.
- **Predicting Interactions:** Leveraging molecular docking and **QSAR** models to predict binding affinity was identified as a potentially crucial feature for predicting a molecule's scent profile.
- **Analyzing Chemical Space:** We talked about the need for non-linear dimensionality reduction (**UMAP, t-SNE**) to meaningfully visualize the complex relationships between molecules, as linear methods like PCA would be insufficient.
### Hypotheses to Explore
- Is the perceived smell of a compound correlated to its proximity to clusters of known natural molecules in a high-dimensional space
- Is the specific **combinatorial pattern** of receptor activation the primary determinant of a molecule's perceived scent?
- Can we find direct correlations between functionally-related receptor clusters and distinct scent families?
### Anticipated Challenges
- **Computational Cost:** The sheer scale of docking simulations was recognized as a major bottleneck and in need of **GPU acceleration**
### Ambitious Ideas
A particularly ambitious, long-term avenue discussed was investigating the role of **Single-Nucleotide Polymorphisms** (SNPs) in olfactory genes to understand the genetic basis of why individuals perceive scents differently.