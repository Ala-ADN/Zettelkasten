**Objective:** To develop and validate computational models for predicting molecular scent profiles and analyzing olfactory receptor interactions, with the goal of accelerating fragrance R&D and enabling the discovery of novel molecules.
## I. Summary of Accomplishments

Key achievements include the curation of a scent database, the development of a nuanced semantic model for odor description, analysis of embeddings as a means to infer scent, baseline predictive model from molecule structure, and a critical analysis of receptor interaction data that identified both opportunities and computational bottlenecks.
## II. Tasks & Key Results

### Centralized Scent Dataset
A comprehensive dataset was created to serve as the "ground truth" for all modeling efforts.
*   **Task:** Aggregated and unified over 4,000 scent data points with 140 scent descriptors from industry-standard sources, including Goodscents, Leffingwell, and the Dravnieks Atlas.
![[Pasted image 20250917010142.png]]
### Context-Aware Scent Representation
To move beyond simple categorical labels, a continuous and nuanced representation of scent was developed.
*   **Task:** Created context-rich numerical representations (**embeddings**) of odor descriptions. These embeddings were enhanced with data from Fragrantica, grounding abstract scent terms in real-world context. An embedding is a numerical fingerprint that allows AI to understand the relationship between concepts.
*   **Results:** Enables the AI to understand scent on a spectrum (e.g., how "rose" relates to "geranium"). This allows for the prediction of subtle scent notes rather than just broad categories.
![[cool description embedding plot.png]]
the graph shows how molecules (points) are related in the semantic embedding space, with distinct boundaries of scents groups exhibiting meaningful overlap
### Baseline Model for Scent Prediction (QSAR)
A predictive model was built to establish a performance baseline for predicting a scent profile from a molecule's structure.
*   **Task:** Developed a Graph Attention Network (GAT) model using atom and bond features to predict a 709-dimension odor embedding.
*   **Result:** The model achieved a **Mean Cosine Similarity of 0.5134**. This result is non-trivial for a complex task and confirms the model's ability to predict scent profiles.
![[Untitled.png]]
### Evaluation of AI-Based Interaction Models (Shortcut Approach)
*   **Task:** Experimented with a molecule-protein cross-encoder model to capture molecule-receptor interactions. Instead of simulating a physical "dock," this model generates a unified numerical vector (**embedding**) that represents the interaction itself.
*   **Result:** Analysis of the model's output showed two key things:
    1.  The interaction embedding alone is not yet sufficient to predict the full, nuanced scent profile.
    2.  However, it showed clear promise in protein function differentiation and predicting singular scent descriptors accurately, meaning it could be enhanced to provide better results
*   **Business Value:** This is a critical proof-of-concept. It puts forward that embedding-based approaches are a promising and efficient path to understanding molecule-receptor interactions, skipping complex simulations.
### Olfactory Receptor Interaction Analysis (Simulation Approach)
The biological mechanism of olfaction was analyzed to link smell to receptor response using conventional methods.
*   **Task:** Analyzed a dataset of 55,000 simulated binding scores between 135 molecules and 409 human Olfactory Receptors (ORs).
*   **Result:** A **statistically significant correlation** was found between receptor activation patterns and perceived scent descriptors, higher than the purely structure based approach. 
*   **Critical Finding:** This process exposed a major computational bottleneck. The initial docking simulation to generate this data required **12 days of continuous computation**. This method is not commercially viable for high-throughput screening.
![[Pasted image 20250917011143.png]]
The graphs show higher scent differentiation using receptor data as opposed to structure, meaning some similarly structured molecules can have different odor profiles, but receptor responses are more biologically tied to perceived smell 

---
## Potential ROI

1.  **R&D Cycle Acceleration:**
    *   **Problem:** molecule design is a slow, iterative process of synthesis and sensory evaluation.
    *   **Solution:** AI models can virtually screen thousands of candidate molecules, identifying high-potential candidates before any costly synthesis occurs.
    *  Caveat: Due to the virtually endless mixtures of already existing compounds, molecular design isn't at the forefront of perfumery at the moment

2.  **Novel & Patentable Molecules:**
	- **Challenging Perfumery Norms** Perfumery has always operated within known constraints (light molecules for top notes, heavy molecules for base notes). Generative AI allows us to treat these constraints as variables to be optimized, enabling us to create molecules that defy traditional classifications.
		- **Example**: A fresh, volatile citrus note like bergamot has a low molecular weight and evaporates quickly. We can task AI to design a molecule that activates the same olfactory receptors as bergamot, but has the higher molecular weight and low volatility, this would be a completely new class of ingredient; a **linear fresh note** that does not fade, fundamentally altering the traditional scent pyramid

3. **Personalization and Universal Design through Genetic Targeting:*
	- **Human scent perception is not uniform:** it varies across populations due to genetic differences (polymorphisms) in olfactory receptor (OR) genes. Generative design allows us to leverage this data in two powerful, complementary ways:
	- **Personalization for Regional Markets:** We can fine-tune a fragrance for a specific demographic. By incorporating data on OR gene variants prevalent in that population, the AI can design a suitable molecule for a potentially polarizing note, ensuring optimization for local preferences.
	- **Maximizing Universal Appeal for Global Launches:** The same technology can be used to achieve the opposite goal: maximizing a molecule's appeal across the widest possible demographic.

---
## Next Steps

1. **Integrate more sophisticated molecular features:**
	such as the Coulomb matrix, which research suggests is highly effective for scent prediction.
2.  **Develop a Receptor Interaction Shortcut (Surrogate Model):**
    *   **Objective:** Overcome the computational bottleneck identified in the simulation analysis
    *   **Action:** Build a lightweight AI surrogate model that learns to predict docking simulation results in seconds, not days
3.  **Deepen Semantic Understanding with Ontologies:**
    *   **Objective:** Prevent models from learning circular definitions and improve the grounding of scent descriptions.
    *   **Action:** Implement a scent ontology that links odor descriptors to a knowledge graph of related actions, places, materials, and emotions (e.g., "smoky" is linked to "fire," "wood," "warmth"). This provides a richer, more stable target for the AI to learn.
4.  **Build a Generative Framework:**
    *   **Objective:** Move from prediction to creation.
    *   **Action:** Leverage the enhanced predictive models and surrogate receptor model to build a generative AI capable of designing novel molecules that meet specific scent and performance (e.g., longevity, molecular weight)