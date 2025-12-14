a [[Convolutional Neural Network|CNN]] that takes a [[Graph]] as an input
edges and nodes are associated with feature vectors
# Propagation rule
$$
H^{(k+1)} = \sigma\left(\tilde{D}^{-1/2} \tilde{A} \tilde{D}^{-1/2} H^{(k)} W^{(k)}\right)
$$
where:
- $\tilde{A}=A+I$ is the adjacency matrix A with added self-loops I.
- $\tilde{D}$ is the degree matrix of $\tilde{A}$ ($d_{ii}$ = num of neighbors of node $i$ )
- $H^{(k)}$ is the matrix of node features at layer k.
- $W^{(k)}$ is the learnable weight matrix at layer k.
- σ is an [[Activation Function]].
# Advantages
- **Isomorphism**
- **Dynamic size**
- **Non-euclidean shape**