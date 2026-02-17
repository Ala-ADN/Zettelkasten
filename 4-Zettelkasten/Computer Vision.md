# [[Computer Vision Template]]
# Tips
### Start Small for Faster Iteration
- Use smaller image resolutions to reduce training time and GPU memory usage, allowing larger batch sizes
- Begin with subsets of data (e.g., 10-20 classes) to validate ideas before scaling to the full dataset
### Optimize Hardware Utilization
- **FP16/Half-Precision Training**: Leverage NVIDIA Tensor Cores for ~50% faster training
- **Use TPUs**: Scale batch sizes by 8x on Kaggle’s free TPUs for accelerated training
### Training Best Practices
- **Learning Rate Schedulers**: Use dynamic learning rates (e.g., `fit_one_cycle` from fastai) for faster convergence
- **LR Warmup**: Start with smaller LRs to stabilize early training (from "Bag of Tricks" paper)
- **Image Augmentations**: Gradually introduce augmentations (e.g., cropping, rotation) to improve generalization. Visualize results to ensure models focus on relevant features
### Transfer Learning Techniques
- **Sequential Unfreezing**: Unfreeze later layers first, then earlier layers during fine-tuning
- **Differential Learning Rates**: Assign higher LRs to later layers (e.g., `10e-3` for output layers, `10e-5` for input layers)
### Architecture & Loss Improvements
- **GeM Pooling + ArcFace**: Replace standard pooling with Generalized Mean (GeM) pooling and use ArcFace loss for imbalanced datasets (critical for large label spaces)
### Progressive Resizing
- Train on smaller images first, then incrementally increase resolution (e.g., 128px → 256px → 512px) for faster convergence
### Label Smoothing
- Add noise to labels to mitigate mislabeled data issues and improve generalization
### Test Time Augmentation (TTA)
- Apply augmentations (e.g., cropping, flipping) during inference to match training conditions and boost robustness
### Depthwise Convolutions
- Replace standard convolutions with depthwise variants (from MobileNet) to reduce parameters and speed up training
### Leverage Libraries
- **Timm/TFimm**: Use optimized pre-trained models and training pipelines from the `timm` library (PyTorch) or `tfimm` (TensorFlow)
### Pseudo-Labeling
- Add high-confidence predictions from your model to the training data (semi-supervised learning)
### [[Visual Grounding]]
