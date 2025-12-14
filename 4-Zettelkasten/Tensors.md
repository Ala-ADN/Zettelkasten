A tensor is an extension of the concepts of scalars, vectors, and matrices
- **Rank 0:** **scalar**
- **Rank 1:** **vector**
- **Rank 2:** **matrix**
- **Rank 3:** **3D tensor** (stack of matrices)
- **Rank 4:** **4D tensor** (images BatchxRGBxWidthxHeight)
- **Rank 5: 5D tensor** (videos)
the most common error in machine learning is tensor shape mismatch
## Keras
each tensor has a **shape** and **dtype**
### Padded tensor
adds padding to attain a fixed shape
### Ragged tensor
handles variable shapes internally
### Disjoint tensor
batches are concatenated in a flat tensor, need additional tensors to specify ids and counts