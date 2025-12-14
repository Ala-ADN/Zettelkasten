# Failures
❌Scarsity of OR binding data, only 20k known ligands, 
-> binding simulations
❌Binding simulatiosn are not differentiable, can't be later used in molecular generation
-> develope a serrogate model that maps the simulation output data
❌Simulating 2milion combinations will take weeks of compute time

<h3>1. Executive Summary</h3>

This report details the progress and significant challenges encountered over the past two weeks on the project to develop a model that efficiently predicts odor description vectors using binding data from 409 known olfactory receptors (ORs). The primary goal was to leverage existing OR binding data to build a predictive model. However, a series of critical roadblocks emerged, necessitating a strategic pivot in our approach.

The initial phase of the project was marked by three significant "failures," which, upon closer examination, have provided invaluable insights and a clearer path forward. These challenges were:

Scarcity of Olfactory Receptor (OR) Binding Data: A thorough investigation of available databases revealed a critical lack of comprehensive and quantitative binding data for a sufficient number of ligands across the 409 target ORs.

Non-Differentiable Nature of Binding Simulations: To overcome the data scarcity, we explored the use of molecular docking simulations. However, these simulations are computationally intensive and, crucially, non-differentiable, making them unsuitable for direct integration into gradient-based machine learning models for molecular generation.

Prohibitive Computational Cost of Large-Scale Simulations: The sheer scale of simulating the binding interactions for a vast chemical space (e.g., 2 million molecules) with our 409 receptors proved to be computationally infeasible within a reasonable timeframe.

This report will elaborate on each of these challenges, the steps taken to address them, and the revised strategy moving forward. While the initial goal was not immediately achievable, the work conducted has been instrumental in defining a more robust and viable research direction.

<h3>2. Detailed Elaboration of Challenges and Mitigation Strategies</h3>

<h4>2.1. Failure 1: Scarcity of Olfactory Receptor Binding Data</h4>

Challenge: The foundational step of our project was to train a model on a large dataset of known odorant molecules and their corresponding binding affinities to the 409 identified human ORs. A comprehensive search of publicly available databases was conducted to aggregate this crucial data.

Findings: Our investigation, supported by the provided documents, revealed a significant bottleneck in the availability of suitable data. While databases like M2OR exist, they primarily contain EC50 data (agonist potency) and have a notable scarcity of IC50 data (antagonist activity).[1] The provided analysis of databases confirms that most resources lack the quantitative, high-quality binding data (IC50 and EC50) across a diverse chemical space and our specific 409 receptors needed to train a robust predictive model. Many databases offer qualitative associations or focus on a very limited number of receptor-ligand pairs. This scarcity of comprehensive binding data for a large and diverse set of molecules across our target receptors makes it impossible to directly train a supervised machine learning model to predict odor descriptions.

Mitigation Strategy: Recognizing the insufficiency of existing experimental data, the logical next step was to generate this data computationally. The proposed solution was to employ molecular binding simulations to predict the binding affinities of a large library of molecules to our 409 ORs.

<h4>2.2. Failure 2: Non-Differentiable Binding Simulations</h4>

Challenge: With the decision to generate binding data through simulations, we turned to established methods like molecular docking.[2] These methods simulate the physical interaction between a ligand (odorant molecule) and a receptor to predict their binding affinity. However, a critical limitation of these simulation techniques is their non-differentiable nature.

Explanation: In the context of our long-term goal to use this model for de novo molecular generation (designing new molecules with specific odor profiles), differentiability is a key requirement. Gradient-based optimization methods, which are at the heart of modern machine learning and generative models, require the model's components to be differentiable. This means we need to be able to calculate the derivative of the output (e.g., a specific odor description) with respect to the input (the molecular structure). Because the output of a traditional binding simulation is a score derived from a complex, non-continuous scoring function, we cannot directly backpropagate gradients through the simulation process. This effectively prevents the seamless integration of these simulations into an end-to-end generative model.

Mitigation Strategy: To circumvent the non-differentiability issue, we proposed the development of a surrogate model. A surrogate model is a machine learning model trained to approximate the output of a complex, computationally expensive, or non-differentiable function.[3] In our case, the plan was to train a neural network to learn the mapping from a molecule's structure to the binding affinity predicted by the simulation. This surrogate model would then be differentiable by design, allowing it to be integrated into a larger generative framework. Computational modeling approaches, including machine learning-based predictions, have become essential for investigating OR structure and ligand binding.[4]

<h4>2.3. Failure 3: Prohibitive Computational Cost of Large-Scale Simulations</h4>

Challenge: The final and most significant hurdle was the immense computational cost associated with generating the necessary data to train the surrogate model.

Analysis: To train an effective surrogate model, we would need a large and diverse dataset of molecule-receptor binding simulations. Our plan was to simulate the binding of a library of approximately 2 million commercially available molecules against each of our 409 olfactory receptors. A rough estimation of the computational time required for this endeavor revealed a significant problem:

Simulations per Molecule: Each molecule needs to be docked against 409 different receptors.

Total Simulations: 2,000,000 molecules * 409 receptors = 818,000,000 individual docking simulations.

Time per Simulation: Even with optimistic estimates for the time required for a single docking simulation, the total compute time would be on the order of weeks, if not months, utilizing our available computational resources.

This prohibitive computational cost made the brute-force approach of simulating all combinations impractical and financially unviable. Recent research has highlighted the use of large-scale docking simulations for predicting odor similarity, but even these studies often focus on a smaller subset of odorants and receptors due to the computational demands.[2][5]

<h3>3. Path Forward and Revised Strategy</h3>

While the initial plan faced significant obstacles, the insights gained have been crucial in refining our project's direction. The past two weeks of work have not been in vain; they have allowed us to identify the core challenges in this domain and formulate a more strategic and feasible approach.

Our revised strategy will focus on the following key areas:

Active Learning and Targeted Simulations: Instead of a brute-force simulation of millions of molecules, we will employ an active learning approach. This involves starting with a small, diverse set of simulated data and iteratively selecting the most informative molecules to simulate next. This will allow us to build a robust surrogate model with a fraction of the computational cost.

Exploring Alternative Differentiable Models: We will investigate emerging differentiable alternatives to traditional molecular docking.[6] These methods aim to directly predict binding affinities in a way that is compatible with gradient-based optimization, potentially eliminating the need for a separate surrogate model.

Leveraging Transfer Learning and Pre-trained Models: We will explore the use of pre-trained models on larger, more general chemical datasets. By fine-tuning these models on our smaller, targeted simulation data, we may be able to achieve high predictive accuracy with less data.

Focus on a Reduced Receptor Set: Initially, we may focus our efforts on a smaller, more well-characterized subset of the 409 olfactory receptors to validate our methodology before scaling up.

<h3>4. Conclusion</h3>

The initial phase of this project has been a valuable learning experience. The "failures" we encountered were not setbacks but rather essential discoveries that have illuminated the complexities of predicting odor from molecular structure. By confronting the challenges of data scarcity, non-differentiable simulations, and computational cost, we have been able to develop a more sophisticated and practical plan for achieving our ultimate goal. The work of the past two weeks has laid a strong foundation for a more targeted and efficient research path, and I am confident that our revised strategy will lead to a successful outcome.