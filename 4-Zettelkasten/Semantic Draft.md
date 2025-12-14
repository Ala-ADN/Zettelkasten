### **1. Introduction**

The human ability to perceive and differentiate a vast array of smells stands in stark contrast to our limited capacity to describe them. Unlike vision or hearing, which are supported by well-defined vocabularies (e.g., color, pitch), the language of scent is notoriously inconsistent and subjective. This "lexical gap" poses a fundamental challenge for perfumers, chemists, and data scientists. The goal of this research is to move beyond ambiguous labels and establish a standardized, continuous space for olfactory descriptors.

To achieve this, we harness the power of vector embeddings, a technique from natural language processing that converts words or phrases into high-dimensional numerical vectors. The core hypothesis is that the geometric relationships between these vectors (e.g., distance, direction) can represent the semantic relationships between the scents they describe. This report outlines various expriments to set the foundation for using embeddings for odor prediciton tasks

# DATA
in this report I made use of goodscents luffingwell combined dataset, uniprot data for olfactory recpetors, GNINA cnn score gratously provided by porfessor tanaka, and darvniks atlas of odor
# Semantic validity of the embeddings

#### **2.2. Experiment One: Baseline Co-occurrence Clustering**

using Co-occurrence Matrix ( each cell (i,j) represents the frequency with which descriptor i and descriptor j appear together)  to perform Hierarchical Clustering we Obtained 28 clusters with decent semantic coherence (clustering method reflects the heirarchical nature of odor descritors (fragrantica odor wheel ) )

a fatal flaw lies in descriptors that appear alone and lack meaningful relationship with the rest, in this example they are grouped in the same cluster 20 containing among others grapefruit, jasmin, ketonic, natural, odorless which have semantic ties to other descriptors but are not represented in the data, this stands as a problem of the combinatorial nature of descriptors and the huge number of datapoints needed to fully capture descriptor relation

Complete clustering results:
Cluster 22: alcoholic, brandy, cognac, ethereal, fermented, rummy, solvent, winey
Cluster 6: aldehydic, citrus, clean, fresh, fruit skin, lemon, orange, orangeflower, ozone, soapy
Cluster 1: alliaceous, garlic, onion, savory, sulfurous
Cluster 11: almond, bitter, cherry
Cluster 16: amber, cedar, dry, musk, sandalwood, vetiver, woody
Cluster 19: animal, aromatic, leathery, tea, tobacco, warm
Cluster 14: anisic, hawthorn, powdery, sweet, vanilla
Cluster 21: apple, banana, fruity, juicy, pear, pineapple, ripe, tropical
Cluster 17: apricot, peach
Cluster 13: balsamic, cinnamon, clove, spicy
Cluster 2: beefy, cooked, meaty, roasted
Cluster 5: bergamot, lavender
Cluster 18: berry, grape, plum, raspberry, strawberry
Cluster 20: black currant, celery, chamomile, gassy, grapefruit, jasmin, ketonic, natural, odorless, sharp
Cluster 26: burnt, caramellic, coffee, hazelnut, nutty, popcorn
Cluster 23: buttery, creamy, dairy, milky
Cluster 28: cabbage, potato, pungent, radish, tomato, vegetable
Cluster 15: camphoreous, cooling, herbal, mint, pine, terpenic
Cluster 27: cheesy, fishy, sour, sweaty
Cluster 25: chocolate, cocoa, malty
Cluster 24: coconut, coumarinic, hay, lactonic
Cluster 8: cortex, geranium, grassy, green, leafy, metallic, weedy
Cluster 10: cucumber, melon, orris, violet
Cluster 7: earthy, mushroom, musty
Cluster 9: fatty, oily, waxy
Cluster 4: floral, honey, hyacinth, rose
Cluster 3: lily, muguet
Cluster 12: medicinal, phenolic, smoky

