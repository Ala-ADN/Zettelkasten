### **Final Project Plan: Generative Olfactory Modeling**

**A Differentiable Framework for Designing Novel Molecules with Targeted Perceptual and Physical Properties**

### **1. Executive Summary**

This document outlines a phased, technically detailed plan to build a generative system for designing novel molecules with specific odor profiles (e.g., "rose," "sandalwood") and optimized physical properties (e.g., high vapor pressure, sustainability). We will achieve this by creating a fully differentiable machine learning pipeline that connects a target scent, represented by a semantic embedding, to the underlying olfactory receptor (OR) activation pattern, and finally to a continuous latent space of molecular structures. By performing gradient-based optimization in this latent space, we can generate novel, valid molecules that are predicted to elicit a desired sensory experience and meet specified performance criteria. The project's success hinges on a data-driven, learned representation of receptor interactions, which, while a proxy for true biophysical measurements, is the only computationally tractable approach to this problem.

### **2. Core Scientific Rationale**

The central challenge in *de novo* odorant design is bridging the vast gap from abstract perceptual goals to concrete chemical structures. This plan rejects the computationally impossible approach of calculating quantum-level biophysical interactions for all ~400 human ORs. Instead, it proposes a pragmatic, state-of-the-art solution:

1.  **Learned Receptor-Interaction Proxy:** We will train a deep learning model using cross-attention to produce a `412-dimensional` vector that serves as a high-fidelity, learned proxy for the full receptor interaction profile. This vector's meaning is derived from and validated against experimental wet-lab data, capturing the complex patterns of agonism and antagonism that define an odor.
2.  **Manifold Mapping for Perception:** We will map this `412-dim` OR vector to a `~4000-dim` semantic space representing human odor perception. This dimensionality increase is not creating information but learning a complex "rendering" function onto a highly structured, lower-dimensional manifold of known scents, a standard technique in modern generative AI.
3.  **Differentiable Generative Core:** By integrating this predictive machinery with a generative model of molecular structures (a Graph VAE), we create an end-to-end differentiable system. This allows us to use the error between a desired and a predicted scent as a loss signal to directly guide the design of a new molecule, atom by atom, in a continuous latent space.

---

### **Phase 0: Data Curation & Infrastructure Setup**

**Goal:** Establish a canonical, version-controlled dataset and a reproducible computational environment. This phase is foundational and critical for project success.

*   **Key Tasks:**
    1.  Aggregate molecule-odor data from Leffingwell and GoodScents databases.
    2.  Canonicalize all molecules to SMILES format using RDKit, removing salts and standardizing tautomers. Generate InChIKeys for unique identification.
    3.  Create scaffold-based splits (e.g., using Bemis-Murcko scaffolds) for training, validation, and testing to ensure robust model evaluation.
    4.  Collect and standardize the FASTA sequences for the ~412 human ORs.
    5.  Set up the Conda/Docker environment with all necessary packages.

*   **Helpful Links & Resources:**
    *   **RDKit (Canonicalization):** [RDKit Documentation](https://www.rdkit.org/docs/index.html)
    *   **Bemis-Murcko Scaffolds:** [Original Paper (J. Med. Chem.)](https://pubs.acs.org/doi/10.1021/jm9602928), [RDKit Implementation](https://www.rdkit.org/docs/source/rdkit.Chem.Scaffolds.MurckoScaffold.html)
    *   **Human OR Sequences:** [UniProt](https://www.uniprot.org/) (search for "olfactory receptor human")

*   **Technical Stack:**
    *   **Packages:** `rdkit`, `pandas`, `numpy`, `scikit-learn`
    *   **Hardware:** Standard CPU (multi-core recommended for parallel processing).
    *   **Compute Estimate:** 4-8 hours for processing ~5,000-10,000 molecules.

*   **Potential Issues & Mitigation:**
    *   **Issue:** Ambiguous stereochemistry in SMILES strings.
    *   **Mitigation:** Flag molecules with undefined stereocenters. Prioritize training on molecules with explicitly defined stereochemistry.
    *   **Issue:** Inconsistent odor descriptors across datasets.
    *   **Mitigation:** Use pre-computed semantic embeddings (as assumed) to unify descriptor language.

*   **Phase Artifact(s):**
    *   `canonical_molecules.csv` (with InChIKey, SMILES, source IDs)
    *   `scaffold_splits.json`
    *   `or_sequences.fasta`
    *   `environment.yml` or `Dockerfile`

*   **Definition of Done:** All data artifacts are generated, versioned in Git/DVC, and the computational environment is reproducible. An exploratory data analysis (EDA) notebook confirms data quality and split distributions.

*   **Evaluation & Acceptance Criteria:** The canonicalization script successfully processes >99% of input molecules. Scaffold splits show no overlap between train/validation/test sets.

---

### **Phase 1: Olfactory Receptor Feature Engineering**

**Goal:** Generate high-dimensional, per-residue embeddings and identify ligand-binding pockets to prepare the protein side of the model for cross-attention.

*   **Key Tasks:**
    1.  Use a pre-trained ESM-2 model to extract per-residue embeddings for each of the 412 OR sequences.
    2.  Generate 3D protein structures for all ORs using ESMFold or AlphaFold.
    3.  Use a geometric tool like fpocket on the predicted 3D structures to identify the coordinates of likely ligand-binding pockets.
    4.  Create a fixed-length "pocket token" sequence for each OR by selecting the per-residue embeddings of the top-K residues lining the predicted pocket. Augment these tokens with structural features (e.g., 3D coordinates relative to pocket centroid, pLDDT score).

*   **Helpful Links & Resources:**
    *   **ESM-2 Model:** [Hugging Face Hub](https://huggingface.co/facebook/esm2_t33_650M_UR50D), [ESM GitHub](https://github.com/facebookresearch/esm)
    *   **ESMFold/ColabFold:** [ESMFold Paper](https://www.science.org/doi/10.1126/science.ade2574), [ColabFold for easy execution](https://github.com/sokrypton/ColabFold)
    *   **fpocket (Pocket Detection):** [fpocket Documentation](http://fpocket.sourceforge.net/)

*   **Technical Stack:**
    *   **Packages:** `torch`, `huggingface-hub`, `transformers`, `biopython`, `MDAnalysis`
    *   **Hardware:** GPU with **≥16 GB VRAM** (NVIDIA V100, A100, or RTX 3090/4090) is essential for ESMFold and batch processing with ESM-2.
    *   **Compute Estimate:**
        *   ESM-2 Embeddings: 2-4 hours on a V100 GPU for 412 proteins.
        *   ESMFold Structures: 5-10 hours on a V100 GPU (approx. 1-2 min per protein).
        *   fpocket: 1-2 hours on a multi-core CPU.

*   **Potential Issues & Mitigation:**
    *   **Issue:** ESMFold produces low-confidence structures (low pLDDT scores), especially in loop regions.
    *   **Mitigation:** Focus on the transmembrane helices, which are typically predicted with higher confidence and form the core of the binding pocket. Use pLDDT scores as an additional feature channel to let the model learn to weight residue information by confidence.
    *   **Issue:** fpocket identifies multiple pockets or no plausible pocket.
    *   **Mitigation:** Filter pockets by volume and known biological constraints (e.g., must be within the transmembrane domain). Manually inspect a subset of 10-20 ORs for sanity checking.

*   **Phase Artifact(s):**
    *   `or_per_residue_embeddings.h5`
    *   Directory of PDB files: `or_structures/`
    *   `or_pocket_residues.json`

*   **Definition of Done:** All ORs have per-residue embeddings, a predicted 3D structure, and a corresponding pocket annotation file. A validation notebook visualizes a sample pocket on its structure.

*   **Evaluation & Acceptance Criteria:** >95% of ORs have a predicted structure with an average pLDDT > 70 in the transmembrane regions. >90% of ORs have a plausible pocket identified.

---

### **Phase 2: Molecular Generative Model & Property Predictors**

**Goal:** Build and train a generative model for molecules and auxiliary models for predicting physical properties, establishing the foundation for guided generation.

*   **Key Tasks:**
    1.  Implement a Graph Variational Autoencoder (GraphVAE) for molecules. The encoder will map a molecular graph to a continuous latent vector `z`, and the decoder will reconstruct the graph from `z`.
    2.  Train the GraphVAE on a large corpus of diverse molecules (e.g., a subset of the ZINC dataset) to learn a smooth and meaningful latent space.
    3.  Separately, train a simple MLP-based property predictor. This model takes a latent vector `z` from the trained VAE encoder as input and predicts target properties like vapor pressure and a synthesizability score (SAscore).
    4.  For the predictive pipeline, develop a separate molecular encoder using a 3D-aware GNN (e.g., SchNet, DimeNet++) as this provides a richer representation for the initial training of the OR predictor.

*   **Helpful Links & Resources:**
    *   **Graph VAEs:** [Original Paper (J. of Cheminformatics)](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-018-0279-9), [PyTorch Geometric Implementation Examples](https://pytorch-geometric.readthedocs.io/en/latest/notes/autoencoder.html)
    *   **ZINC Dataset:** [Official Website](https://zinc.docking.org/)
    *   **SchNet/DimeNet:** [Implementations in PyG](https://pytorch-geometric.readthedocs.io/en/latest/modules/nn.html#models)
    *   **SAscore:** [Original Paper (J. Cheminform.)](https://jcheminf.biomedcentral.com/articles/10.1186/1758-2946-1-8), [RDKit Implementation](https://www.rdkit.org/docs/source/rdkit.Chem.QED.html)

*   **Technical Stack:**
    *   **Packages:** `torch`, `dgl` or `torch-geometric`, `rdkit`, `scikit-learn`
    *   **Hardware:** High-end GPU (V100/A100, 32GB+ VRAM) is highly recommended for training the GraphVAE on a large dataset.
    *   **Compute Estimate:**
        *   GraphVAE Training: 12-48 hours on an A100 GPU, depending on dataset size.
        *   Property Predictor Training: 1-2 hours on a standard GPU.

*   **Potential Issues & Mitigation:**
    *   **Issue:** VAE suffers from "posterior collapse," where the latent space is ignored.
    *   **Mitigation:** Use techniques like KL-annealing during training or adopt a more advanced VAE architecture.
    *   **Issue:** The generated molecules are often invalid (chemically nonsensical).
    *   **Mitigation:** Implement validity checks in the decoder and fine-tune the VAE with a reinforcement learning objective that rewards valid outputs.

*   **Phase Artifact(s):**
    *   Trained GraphVAE checkpoints: `graph_vae_encoder.pt`, `graph_vae_decoder.pt`
    *   Trained property predictor checkpoint: `property_predictor.pt`

*   **Definition of Done:** The GraphVAE can reconstruct molecules from the test set with >95% validity and high Tanimoto similarity (>0.7 on average). The property predictor achieves a strong correlation (R² > 0.8) with true values on a held-out test set.

*   **Evaluation & Acceptance Criteria:** Quantitative metrics on reconstruction, validity, and novelty for the VAE. Predictive accuracy (R², MAE) for the property predictor.

---

### **Phase 3: Differentiable OR-Response Predictor**

**Goal:** Create the differentiable bridge between the molecular latent space and the biological OR response vector.

*   **Key Tasks:**
    1.  Design a hierarchical cross-attention model.
        *   **Input A (Molecule):** A molecular representation derived from the pre-trained GraphVAE's latent space `z` (passed through a small MLP).
        *   **Input B (Receptor):** The augmented "pocket token" sequences for all 412 ORs.
    2.  The model will predict a single `412-dimensional` vector representing the activation logits for each receptor in parallel.
    3.  Train this model on your experimental dataset of `(molecule, OR activation)` pairs. Use the rich 3D GNN encoder from Phase 2 first to ensure the model learns robustly, then transition to using the VAE latent space.
    4.  Incorporate biological priors, such as a competition layer (e.g., softmax over logits) and an L1 sparsity penalty on the output vector to encourage realistic activation patterns.

*   **Helpful Links & Resources:**
    *   **Cross-Attention:** ["Attention Is All You Need" (Vaswani et al.)](https://arxiv.org/abs/1706.03762), [Illustrated Transformer Blog Post](http://jalammar.github.io/illustrated-transformer/)
    *   **Protein-Ligand ML:** [CAPLA Paper (for inspiration)](https://pubmed.ncbi.nlm.nih.gov/36688724/)

*   **Technical Stack:**
    *   **Packages:** `torch`, `einops` (for tensor manipulation)
    *   **Hardware:** GPU with ≥16 GB VRAM (V100/A100).
    *   **Compute Estimate:** 8-24 hours for training, depending on the size of the experimental dataset.

*   **Potential Issues & Mitigation:**
    *   **Issue:** Experimental data is sparse and noisy, leading to poor model convergence.
    *   **Mitigation:** This is the primary project risk. Mitigate with: (1) Pre-training interaction layers on larger, public protein-ligand datasets (e.g., PDBbind). (2) Heavy use of regularization (dropout, weight decay). (3) Committing to the active learning loop in Phase 5 to iteratively improve the model.

*   **Phase Artifact(s):**
    *   Trained OR-response predictor checkpoint: `or_predictor.pt`

*   **Definition of Done:** The model is trained and its performance is benchmarked on the held-out test set. An analysis notebook is created to inspect attention maps for interpretability.

*   **Evaluation & Acceptance Criteria:** Mean ROC AUC per receptor > 0.75 (for receptors with sufficient data). The cosine similarity between predicted and true OR vectors should be significantly better than a baseline model.

---

### **Phase 4: Perceptual Decoder & Generative Optimization Loop**

**Goal:** Map the OR-space to the perceptual space and implement the gradient-based optimization loop to generate novel molecules.

*   **Key Tasks:**
    1.  Train a "perceptual decoder" model (e.g., a small Transformer or MLP) that maps the `412-dim` OR vector to the `~4000-dim` semantic odor embedding.
    2.  Develop the core optimization script. This script will:
        *   Take a target semantic embedding and a set of property constraints as input.
        *   Find the corresponding target OR vector (e.g., by training a small inverse model or using optimization).
        *   Initialize a random latent vector `z`.
        *   Iteratively update `z` via gradient ascent to minimize a multi-objective loss function (OR vector similarity + property satisfaction + validity regularizer).
    3.  Decode the final optimized `z` using the VAE decoder to produce the final SMILES string.

*   **Helpful Links & Resources:**
    *   **PyTorch Optimization:** [torch.optim Documentation](https://pytorch.org/docs/stable/optim.html)
    *   **Multi-Objective Optimization:** [A good overview of techniques](https://en.wikipedia.org/wiki/Multi-objective_optimization)

*   **Technical Stack:**
    *   **Packages:** `torch`, `numpy`
    *   **Hardware:** Standard GPU is sufficient for the optimization loop.
    *   **Compute Estimate:** 1-5 minutes per molecule generation.

*   **Potential Issues & Mitigation:**
    *   **Issue:** The optimization gets stuck in poor local minima, producing repetitive or uninteresting molecules.
    *   **Mitigation:** Run the optimization from multiple random starting points in the latent space. Add a novelty term to the loss function that penalizes similarity to known molecules.
    *   **Issue:** Balancing the different components of the loss function is difficult.
    *   **Mitigation:** Treat the loss weights as key hyperparameters. Perform a sensitivity analysis to understand their impact.

*   **Phase Artifact(s):**
    *   Trained perceptual decoder checkpoint: `percept_decoder.pt`
    *   Optimization script: `generate_molecule.py`

*   **Definition of Done:** The full pipeline is connected. The script can take a text descriptor (e.g., "woody"), find its embedding, and generate a novel, valid SMILES string predicted to have that odor and meet property constraints.

*   **Evaluation & Acceptance Criteria:** The decoder achieves a high cosine similarity (>0.8) between predicted and true embeddings on a test set. The generative loop produces valid molecules for >90% of runs. Qualitative evaluation of generated molecules for a few target scents shows they are chemically reasonable.

---

### **Phase 5: Active Learning, Validation, and Iteration**

**Goal:** Close the loop by using the model to guide wet-lab experiments, feeding the results back to create a progressively smarter system.

*   **Key Tasks:**
    1.  Use the generative model to produce a list of 50-100 candidate molecules that are maximally informative. The acquisition function should prioritize molecules that:
        *   Are predicted to have desirable properties (high target scent score, good physical props).
        *   Lie in regions where the model is most uncertain (e.g., high variance in predictions from a model ensemble).
        *   Are chemically diverse to explore new areas of chemical space.
    2.  Synthesize and test these molecules experimentally (heterologous assays for OR activation, sensory panel for perception).
    3.  Ingest the new, high-quality data into your training set.
    4.  Retrain or fine-tune the entire pipeline (especially the OR predictor and perceptual decoder).
    5.  Repeat the cycle.

*   **Helpful Links & Resources:**
    *   **Active Learning in ML:** [A comprehensive literature survey](https://arxiv.org/abs/1807.04823)
    *   **Bayesian Optimization:** [A practical guide](https://distill.pub/2020/bayesian-optimization/)

*   **Technical Stack:**
    *   This phase is primarily process-driven, relying on the tools from previous phases.

*   **Potential Issues & Mitigation:**
    *   **Issue:** The wet-lab cycle is slow and expensive.
    *   **Mitigation:** This is an inherent constraint. Maximize the information gain from each batch by using a sophisticated acquisition function. Start with small, targeted batches.
    *   **Issue:** Experimental results contradict model predictions.
    *   **Mitigation:** This is not a failure, but a success of active learning. These data points are the most valuable for improving the model. Analyze them carefully to understand the model's blind spots.

*   **Phase Artifact(s):**
    *   `candidate_batch_N.csv` (list of molecules for synthesis)
    *   `experimental_results_batch_N.csv`
    *   Versioned model checkpoints after each retraining cycle.

*   **Definition of Done:** The first batch of molecules has been proposed, the experimental data has been successfully ingested, and the models have been retrained, showing an improvement in predictive accuracy on a held-out control set.

*   **Evaluation & Acceptance Criteria:** The model's performance (AUC, cosine similarity) improves with each cycle of active learning. The "hit rate" of finding molecules with desired properties increases over time.

---

### **Bibliography**

1.  Sanchez-Lengeling, B., et al. "Generative Models for De Novo Drug Design." *Journal of Cheminformatics*, 2018.
2.  Jumper, J., et al. "Highly accurate protein structure prediction with AlphaFold." *Nature*, 2021.
3.  Lin, Z., et al. "Evolutionary-scale prediction of atomic-level protein structure with a language model." *Science*, 2023.
4.  Stärk, H., et al. "3D Infomax improves GNNs for Molecular Property Prediction." *ICML*, 2022.
5.  Vaswani, A., et al. "Attention Is All You Need." *NeurIPS*, 2017.
6.  Ertl, P., & Schuffenhauer, A. "Estimation of synthetic accessibility score of drug-like molecules based on molecular complexity and fragment contributions." *Journal of Cheminformatics*, 2009.
   
   # BULLET POINT REFERENCES AND THEIR LOCATIONS IN THE REPORT

## EXECUTIVE SUMMARY:
-  **** A deep position-encoding model for predicting olfactory perception - Nature npj Systems Biology (2024) - *AUROC 0.813 for olfactory perception prediction*[1]
-  **** De Novo Molecular Generation Augmentation for Drug Discovery (2024)[2]
-  **** PIGNet2: a versatile deep learning-based protein–ligand interaction predictor (2024) - *84.09% accuracy on Posebusters benchmark*[3]
-  **** Atomevo-odor: A database for understanding olfactory receptor interactions (2024)[4]
-  **** A novel molecule generative model of VAE combined with Transformer (2024)[5]
-  **** Interformer: an interaction-aware model for protein-ligand docking (2024)[6]
-  **** Mapping the combinatorial coding between olfactory receptors and perception (2024)[7]
-  **** Language models of protein sequences at the scale of evolution (ESM-2, 2022)[8]
-  **** CAPLA: improved prediction of protein–ligand binding affinity by cross-attention (2023)[9]

## **CORE SCIENTIFIC RATIONALE:**
-  **** De Novo Molecular Generation Augmentation for Drug Discovery (2024)[2]
-  **** PIGNet2: a versatile deep learning-based protein–ligand interaction predictor (2024)[3]
-  **** A novel molecule generative model of VAE combined with Transformer (2024)[5]
-  **** Mapping the combinatorial coding between olfactory receptors and perception (2024)[7]
-  **** Expanding Molecular Design with Graph Variational Autoencoders (2024)[10]
-  **** CAPLA: improved prediction of protein–ligand binding affinity by cross-attention (2023)[9]
-  **** CheapNet: Cross-attention on Hierarchical representations for protein-ligand binding (2025)[11]
-  **** Transformer graph variational autoencoder for generative molecular design (2024)[12]
-  **** Exploring the space between word and odor perception embeddings (2024)[13]
-  **** The Semantic Organization of the English Odor Vocabulary (2024)[14]
-  **** Grounding Semantics in Olfactory Perception (2015)[15]

## **PHASE 0: DATA CURATION & INFRASTRUCTURE SETUP:**
-  **** Molecular Sets (MOSES): A Benchmarking Platform for Molecular Generation Models (2018)[16]
-  **** Prediction of Compound Synthesis Accessibility (2022)[17]
-  **** Molecular Sets (MOSES): A Benchmarking Platform - Frontiers in Pharmacology (2020)[18]
-  **** Scaffold Splits Overestimate Virtual Screening Performance (2024)[19]
-  **** Human olfactory receptor OR51E2 bound to propionate (2022)[20]
-  **** Bemis-Murcko scaffolds and their variants - GitHub (2023)[21]
-  **** Human Consensus Olfactory Receptor OR52c (2022)[22]
-  **** Some Thoughts on Splitting Chemical Datasets (2024)[23]

## **PHASE 1: OLFACTORY RECEPTOR FEATURE ENGINEERING:**
-  **** Language models of protein sequences at the scale of evolution (ESM-2, 2022)[8]
-  **** Protein language models learn evolutionary statistics of interacting sequences (2024)[24]
-  **** Protein A-like Peptide Design Based on Diffusion and ESM2 Models (2024)[25]
-  **** Protein ligand binding site prediction using graph transformer neural networks (2024)[26]
-  **** Efficient generation of protein pockets with PocketGen (2024)[27]
-  **** Protein Language Models Enable Accurate Cryptic Ligand Binding Site Prediction (2024)[28]

## PHASE 2: MOLECULAR GENERATIVE MODEL & PROPERTY PREDICTORS:
-  **** De Novo Molecular Generation Augmentation for Drug Discovery (2024)[2]
-  **** A novel molecule generative model of VAE combined with Transformer (2024)[5]
-  **** Expanding Molecular Design with Graph Variational Autoencoders (2024)[10]
-  **** Molecular Sets (MOSES): A Benchmarking Platform (2018)[16]
-  **** Molecule Generation of Drugs Using VAE[29]
-  **** Emin: A First-Principles Thermochemical Descriptor for Predicting Synthesizability (2024)[30]
-  **** Retrosynthetic Accessibility (RA) score (2020)[31]
-  **** Benchmarking structure-based three-dimensional molecular generation (2024)[32]
-  **** Prediction of Compound Synthesis Accessibility (2022)[17]
-  **** An evaluation of unconditional 3D molecular generation methods (2024)[33]
-  **** Molecular Sets (MOSES): A Benchmarking Platform - Frontiers (2020)[18]

## **PHASE 3: DIFFERENTIABLE OR-RESPONSE PREDICTOR:**
-  **** PIGNet2: a versatile deep learning-based protein–ligand interaction predictor (2024)[3]
-  **** Atomevo-odor: A database for understanding olfactory receptor (2024)[4]
-  **** Mapping the combinatorial coding between olfactory receptors and perception (2024)[7]
-  **** Recent advances in AI-driven protein-ligand interaction predictions (2024)[34]
-  **** CAPLA: improved prediction of protein–ligand binding affinity by cross-attention (2023)[9]
-  **** CheapNet: Cross-attention on Hierarchical representations (2025)[11]
-  **** LABind: identifying protein binding ligand-aware sites (2024)[35]

## **PHASE 4: PERCEPTUAL DECODER & GENERATIVE OPTIMIZATION LOOP:**
-  **** Mapping the combinatorial coding between olfactory receptors and perception (2024)[7]
-  **** Targeted molecular generation with latent reinforcement learning (2024)[36]
-  **** Sample efficient reinforcement learning with active learning for molecular design (2024)[37]
-  **** Exploring the space between word and odor perception embeddings (2024)[13]
-  **** Optimal Molecular Design: Generative Active Learning (2024)[38]
-  **** The Semantic Organization of the English Odor Vocabulary (2024)[14]
-  **** Grounding Semantics in Olfactory Perception (2015)[15]
-  **** Ensembling methods for protein-ligand binding affinity prediction (2024)[39]

## **PHASE 5: ACTIVE LEARNING, VALIDATION, AND ITERATION:**
-  **** Mol-PECO (2024): A deep position-encoding model for predicting olfactory perception[1]
-  **** Atomevo-odor: A database for understanding olfactory receptor (2024)[4]
-  **** Mapping the combinatorial coding between olfactory receptors and perception (2024)[7]
-  **** Sample efficient reinforcement learning with active learning (2024)[37]
-  **** Optimal Molecular Design: Generative Active Learning (2024)[38]
-  **** Finding the most potent compounds using active learning (2024)[40]
-  **** Deep Batch Active Learning for Drug Discovery (2023)[41]
-  **** Optimal Molecular Design: Generative Active Learning Combining REINVENT (2024)[42]

***

## **COMPLETE REFERENCE URLs:**

**** https://www.nature.com/articles/s41540-024-00401-0[1]
**** https://publishing.emanresearch.org/CurrentIssuePDF/EmanPublisher_1_5730angiotherapy-8109996.pdf[2]
**** https://pubs.rsc.org/zh-tw/content/articlelanding/2024/dd/d3dd00149k[3]
**** https://pubmed.ncbi.nlm.nih.gov/39977983/[4]
**** https://arxiv.org/pdf/2402.11950.pdf[5]
**** https://www.nature.com/articles/s41467-024-54440-6[6]
**** https://www.biorxiv.org/content/10.1101/2024.09.16.613334v1[7]
**** https://www.nature.com/articles/s41598-025-99785-0[36]
**** https://pubmed.ncbi.nlm.nih.gov/39999605/[34]
**** https://www.biorxiv.org/content/10.1101/2022.07.20.500902v1.full.pdf[8]
**** https://chemrxiv.org/engage/api-gateway/chemrxiv/assets/orp/resource/item/6829051ee561f77ed46d15e9/original/expanding-molecular-design-with-graph-variational-autoencoders-a-comparative-study-of-pair-encoding-and-character-tokenization.pdf[10]
**** https://academic.oup.com/bioinformatics/article/39/2/btad049/6998204[9]
**** https://www.biorxiv.org/content/10.1101/2024.01.30.577970v1.full-text[24]
**** https://openreview.net/forum?id=A1HhtITVEi[11]
**** https://pmc.ncbi.nlm.nih.gov/articles/PMC11510650/[25]
**** https://www.sciencedirect.com/science/article/pii/S0006349525000359[12]
**** https://www.nature.com/articles/s41467-025-62899-0[35]
**** https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0308425[26]
**** https://pubs.rsc.org/doi/d3sc04653b[37]
**** https://ceur-ws.org/Vol-3064/mdk1.pdf[13]
**** https://www.nature.com/articles/s42256-024-00920-9[27]
**** https://chemrxiv.org/engage/api-gateway/chemrxiv/assets/orp/resource/item/662a030e418a5379b0aa3994/original/optimal-molecular-design-generative-active-learning-combining-reinvent-with-absolute-binding-free-energy-simulations.pdf[38]
**** https://onlinelibrary.wiley.com/doi/10.1111/cogs.13205[14]
**** https://www.beilstein-journals.org/bjoc/articles/20/185[40]
**** https://aclanthology.org/P15-2038.pdf[15]
**** https://openreview.net/forum?id=SFCHv2G33F[28]
**** https://elifesciences.org/reviewed-preprints/89679v1[41]
**** https://pubs.acs.org/doi/10.1021/acs.jctc.4c00576[42]
**** https://github.com/molecularsets/moses[16]
**** https://www.frontiersin.org/journals/pharmacology/articles/10.3389/fphar.2020.565644/full[18]

[1] https://www.nature.com/articles/s41540-024-00401-0
[2] https://publishing.emanresearch.org/CurrentIssuePDF/EmanPublisher_1_5730angiotherapy-8109996.pdf
[3] https://pubs.rsc.org/zh-tw/content/articlelanding/2024/dd/d3dd00149k
[4] https://pubmed.ncbi.nlm.nih.gov/39977983/
[5] https://arxiv.org/pdf/2402.11950.pdf
[6] https://www.nature.com/articles/s41467-024-54440-6
[7] https://www.biorxiv.org/content/10.1101/2024.09.16.613334v1.full-text
[8] https://www.biorxiv.org/content/10.1101/2022.07.20.500902v1.full.pdf
[9] https://academic.oup.com/bioinformatics/article/39/2/btad049/6998204
[10] https://chemrxiv.org/engage/api-gateway/chemrxiv/assets/orp/resource/item/6829051ee561f77ed46d15e9/original/expanding-molecular-design-with-graph-variational-autoencoders-a-comparative-study-of-pair-encoding-and-character-tokenization.pdf
[11] https://openreview.net/forum?id=A1HhtITVEi
[12] https://www.sciencedirect.com/science/article/pii/S0006349525000359
[13] https://ceur-ws.org/Vol-3064/mdk1.pdf
[14] https://onlinelibrary.wiley.com/doi/10.1111/cogs.13205
[15] https://aclanthology.org/P15-2038.pdf
[16] https://github.com/molecularsets/moses
[17] https://pmc.ncbi.nlm.nih.gov/articles/PMC8838603/
[18] https://www.frontiersin.org/journals/pharmacology/articles/10.3389/fphar.2020.565644/full
[19] https://arxiv.org/pdf/2406.00873.pdf
[20] https://www.rcsb.org/structure/8F76
[21] https://github.com/rdkit/rdkit/discussions/6844
[22] https://www.rcsb.org/structure/8HTI
[23] http://practicalcheminformatics.blogspot.com/2024/11/some-thoughts-on-splitting-chemical.html
[24] https://www.biorxiv.org/content/10.1101/2024.01.30.577970v1.full-text
[25] https://pmc.ncbi.nlm.nih.gov/articles/PMC11510650/
[26] https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0308425
[27] https://www.nature.com/articles/s42256-024-00920-9
[28] https://openreview.net/forum?id=SFCHv2G33F
[29] https://www.atlantis-press.com/article/126002037.pdf
[30] https://chemrxiv.org/engage/api-gateway/chemrxiv/assets/orp/resource/item/65b7fe07e9ebbb4db93b12a1/original/e-min-a-first-principles-thermochemical-descriptor-for-predicting-molecular-synthesizability.pdf
[31] https://github.com/reymond-group/RAscore
[32] https://arxiv.org/abs/2407.04424
[33] https://arxiv.org/html/2505.00518v1
[34] https://academic.oup.com/bib/article/25/3/bbae145/7641193
[35] https://www.nature.com/articles/s41467-025-62899-0
[36] https://www.nature.com/articles/s41598-025-99785-0
[37] https://pubs.rsc.org/doi/d3sc04653b
[38] https://chemrxiv.org/engage/api-gateway/chemrxiv/assets/orp/resource/item/662a030e418a5379b0aa3994/original/optimal-molecular-design-generative-active-learning-combining-reinvent-with-absolute-binding-free-energy-simulations.pdf
[39] https://www.nature.com/articles/s41598-024-72784-3
[40] https://www.beilstein-journals.org/bjoc/articles/20/185
[41] https://elifesciences.org/reviewed-preprints/89679v1
[42] https://pubs.acs.org/doi/10.1021/acs.jctc.4c00576