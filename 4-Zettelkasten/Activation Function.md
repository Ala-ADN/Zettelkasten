### 1. Rectified Linear Unit (ReLU): $f(x)=max(0, x)$
- **Use Case:** Most commonly used in hidden layers of neural networks.
- **Advantages:** Addresses vanishing gradient problem, computationally efficient.
- **Disadvantages:**
  - **Dying ReLU Problem:** Neurons can get stuck at zero for all inputs and stop learning.
  - **Unbounded Output:** Outputs can grow indefinitely, which may lead to instability.

### 2. Sigmoid (Logistic): $f(x)=\frac{1}{1 + e^{-x}}$
- **Use Case:** Typically used in the output layer for binary classification problems.
- **Advantages:** Outputs probabilities, useful for interpreting binary outcomes.
- **Disadvantages:**
  - **Vanishing Gradient Problem:** Gradients can become very small, hindering backpropagation and slowing down learning.
  - **Output Not Zero-Centered:** Can cause gradients to have different magnitudes, which may slow down convergence.

### 3. Hyperbolic Tangent: $f(x)=tanh(x)$
- **Use Case:** Used in hidden layers, especially in RNNs.
- **Advantages:** Zero-centered output, useful for handling strongly negative and positive inputs.
- **Disadvantages:**
  - **Vanishing Gradient Problem:** Similar to the sigmoid, gradients can become very small.
  - **Computationally Intensive:** More expensive to compute than ReLU.

### 4. Leaky ReLU $f(x) = \begin{cases} x & \text{if } x > 0 \\ \alpha x & \text{if } x \leq 0 \end{cases}$   *(typically $\alpha$ = 0.01)*
- **Use Case:** Used in hidden layers to avoid dying ReLU problem.
- **Advantages:** Prevents neurons from becoming inactive, maintains small gradients for negative inputs.
- **Disadvantages:**
  - **Unbounded Output:** Like ReLU, outputs can grow indefinitely.
  - **Fixed Negative Slope:** The slope for negative inputs is fixed and may not be optimal for all problems.

### 5. Swish $f(x)=\frac{x}{1 + e^{-x}} = x \cdot segmoid(x)$
- **Use Case:** Effective in various deep learning tasks, including computer vision and NLP.
- **Advantages:** Smooth and non-monotonic, helps achieve better performance and faster convergence.
- **Disadvantages:**
  - **Computationally Intensive:** More complex to compute than ReLU.
  - **Unproven in Some Cases:** Newer function with less empirical evidence compared to more established functions like ReLU and sigmoid.

![[mermaid-diagram-2024-07-04-215821.svg]]