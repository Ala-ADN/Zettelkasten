OR5AN1 
OR2T11 

rod belek ml over engineering

TN DD code


DATA
combined leffingwell and goodscents expert labaled single molecule oderants with 4k molecules and 140 descriptors

Experiment One
I want to tacle the fundamental challenge of odor description using natural language, the human ability to express scent perception is exceptionally poor compared to other senses, using recent vector embeddings can standardize scent descriptions into potentially continuous space

part one: naiive coorelation clustering of descriptors as baseline
using rudimentary NLP techniques, I constructed a matrix with descriptor semantic distance by their co-occurance and got a clustering of scent groups using hierarchical clustering
the custers are decently coherent for the small corpus of only 4k molceules however rare descriptors are lumped together in a miscellaneous cluster

part 2: embedding based clustering
I compared embedding descriptors alone vs adding "smell" suffix vs adding domain specific descriptor definitions
the comparison was done with HDBSCAN, hierarchical and affinity propagation clustering algorithms
the basic embeddings performed very poorly as the generic openai embedding model lacks scent context
adding smell improved the performance (closer vectors, more coherent clusters) but it is still unsatisfactory even compared to correlation based embeddings as it still lacks scent comprehension
adding the definition scraped from fragrantica performed the best with nuanced scent comprehension and subtle distinction
this serves as the foundation of the merits of embeddings in guiding scent predictions instead of labels 