# Olfactory Receptor Binding Data
## Experimental 
The foundational step of our project was to train a model on a large dataset of known odorant molecules and their corresponding binding affinities to human ORs. I found this
M2OR (6k pairs) with binding data, unification effort will take a significant amount of time to clean and prepare the data
## Simulated
Tried various Docking Simulation models, All too slow, the best performing was GNINA with 22 seconds per pair
Tried prediction models, generic binding models didn't perform well on GPCR benchmarks
Tried GPCR specific model AiGPro, limited to only 231 human GPCRs only 1 was an OR
## Solution
GNINA CNN binding scores (55k pairs) from correspondance with Prof, Shigenori Tanaka author of "A Structure-Based Approach for Predicting Odor Similarity of Molecules via Docking Simulations with Human Olfactory Receptors"
the docking simulation took the team 12 days
# Baseline Model
# Graph Attention Network 
Utilize molecular graph representation to aggregate atom and bond level features in order to predict a 709 dim Odor description embedding with cosine-MSE loss for balanced angular magnitude alignment

Final Test Results:
Mean Cosine Similarity: 0.5134 ± 0.2739 (non-trivial for a complex task)
Median Cosine Similarity: 0.5538 (Median > Mean so there are some poor outliers pulling the average do)
Mean MSE: 0.0004 ± 0.0003 (no major prediction errors)
![[Untitled.png]]
# Receptor response analysis
## Preparing Dravnieks
The GNINA data is based on Dravnieks odor atlas, so I prepared the data to extract odor description for each odorant
Used a consensus algorithm to go from population statistic to one hot encoded descriptors, with binomial test (alpha=0.3, n_eff=150) for statistical significant  and an effective size threshold of 0.5 std to penalize overrepresented descriptor and promote rare ones, to get balanced labels
Column Sums: Odorant per descriptor
![[Untitled 1.png]]
Row Sums: Descriptors per Odorant
![[Untitled 2.png]]
The consensus data yielded better performance in clustering, producing meaningful groups and coherent molecule descriptions 
## Receptor response relation to embedding similarity
*Modest but statistically significant (p<0.01) correlation*
Pearson r:  0.113 (p=2.85e-27) | Spearman ρ: 0.121 (p=9.32e-31)
![[Untitled 4.png]]
AUC suggests a gap between receptor response and verbal smell description, as expected in highly complex non linear system 

TODO ablation against structure similarity AUC
# Model to bridge the gap
TODO