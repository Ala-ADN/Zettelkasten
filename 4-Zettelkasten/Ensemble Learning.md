### Definition
a [[Machine Learning]] technique that aggregates two or more learners in order to produce better predictions
### Methods
##### Bagging (Bootstrap Aggregating) 
creates multiple subsets of the original dataset using random sampling with replacement. A model, like a [[Decision Tree]], is trained on each subset, and the final prediction is obtained by averaging the predictions (for regression) or taking a majority vote (for classification)
eg. [[Random Forests]]
##### Boosting
builds models sequentially, each trying to correct the error made by the previous one. The models are combined either by contributions weighted by accuracy or adding the predictions while minimizing a loss function
eg. AdaBoost, Gradient Boosting, and XGBoost.
##### Stacking (Stacked Generalization)
involves training multiple different types of models (e.g., decision trees, support vector machines, neural networks) on the same dataset and then using a meta-model to combine their predictions. The meta-model learns how to best combine the base models' outputs to improve overall performance.
##### Voting
In this method, multiple models (which could be of different types) are traineindependently, and their predictions are combined through a voting mechanism. In harvoting, the final prediction is the one with the most votes, while in soft voting, the probabilitiepredicted by each model are averaged, and the class with the highest average probability ichosen.