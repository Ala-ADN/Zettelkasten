DTA: drug target affinity
DGDTA: dynmic graph DTA

Gradient-enhanced regression
- basic
- need to extract many features
- need for feature engineering
- cannot identify new features that are not given

Convolution Neural Network (DeepDTA character based | WideDTA word based)
- needs 1D representation of compound (SMILES / DeepSMILES)
- extracts all the features from their original source the structure
- fixed convolution window => can't get the full context of each
- explainable using Saliency map to find the key 

Graph Attention Network (DGDTA)
- Convert SMILES to a graph
- attention aggregates each the feature vectors (atomic symbols, adjacent hydrogens, adjacent atoms, implicit valence, total energy, gap energy \[DeepChem]) from each node to get the full context of the molecule 
- State of the art with 90% accuracy score
- most explainable

Genetic Algorithm for further enhancements(EvoMol):
- uses the predictions as a fitness function to further enhance the batch
- optimise the batch into more suitable candidates

Explainable AI:
- Local Interpretable Model-agnostic Explanations for graphs
- Occlusion sensitivity to show which part of the SMILES contributed the most to the prediction
- Attention weights from GAT can provide significant insight