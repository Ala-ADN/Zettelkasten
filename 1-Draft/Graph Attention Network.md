# Definition
A [[Graph Neural Network|GNN]] utilizing [[Attention]], every node's feature vector is recalculated by aggregating it's neighbors
# Uses in Bioinformatics
GAN can assign weights to each part of the molecule correlating to it's bioactivity, this can be exploited as a [[Explainable Neural Network]] to find key features
# Method
## Importance of Neighbors
first concatenate the feature vectors on neighboring nodes, linearly map it from $\mathbb{R}^{2d}$ to $\mathbb{R}^{d'}$, apply non linear activation function to finally map it to a scaler
$$ e_{ij} = \mathbf{a}^\top \cdot \text{LeakyReLU} \left( \mathbf{W} [\mathbf{d}_i \parallel \mathbf{d}_j] \right)$$
Where:
- $\mathbf{d}_i\mathbf{d}_j$​ are the feature vectors of nodes i and j respectively ($d$)
- $\mathbf{W}$ is a trainable weight matrix 
- $[\mathbf{d}_i \parallel \mathbf{d}_j]$ denotes the concatenation of the feature vectors 
- LeakyReLU is the Leaky Rectified Linear Unit activation function
- $\mathbf{a}$ is a learnable attention vector (outside the $\text{LeakeyReLU}$ for dynamic attention)
- $e_{ij}$ represents the attention coefficient between nodes i and j

## Softmax Normalization
$$
a_{ij} = \frac{\exp(e_{ij})}{\sum\limits_{k \in \mathcal{N}_i} \exp(e_{ik})}
$$
Where:
-  $\mathcal{N}_i$ set of neighbors of node i
- $a_{ij}$ normalized attention weight from node j to i