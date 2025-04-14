# Definition
**Attention** determines the relative importance of each component in a sequence relative to the other components in that sequence, 
$$
Attention(Q, K, V) = \text{softmax}\left({QK^T}/{\sqrt{d}}\right)V.
$$
# QKV variant of an Attention Network 
$$
\color{white} {\textrm {Context}}=(XW_{v})^{T}\times \mathrm {softmax} \left((W_{k}X^{T})\times ({\underline {x}}W_{q})^{T}/{\sqrt {d_{k}}}\right)
$$
- **X** denotes a matrix sized ***m x dk*** consisting of the embeddings of all m words.
- **x** denotes the embedding vector size ***n*** of the current word.
- The attention head includes sub-networks, each having ***dk*** neurons, being **Wq, Wk and Wv** their respective weight matrices, all them sized ***n x dk***.
- **q** ("query") is a vector sized ***dk*** 
- **K** ("key") and **V** ("value") are ***m x dk*** matrices.
- **SoftMax** result is a vector sized ***m*** that later on is multiplied by the matrix V=XWv to obtain the context vector sized .
- Rescaling by √dk prevents a high variance in qWkT
# Multi-head