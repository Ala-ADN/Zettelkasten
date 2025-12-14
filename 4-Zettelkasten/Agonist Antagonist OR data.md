## Filtered Datasets: Only Those Containing IC50 and EC50 Data

After comprehensive analysis of the available olfactory receptor bioactivity databases, here are the resources that specifically contain **quantitative IC50 and EC50 potency measurements**:

### **Primary Databases with Extensive EC50/IC50 Data**

**1. M2OR (Molecule to Olfactory Receptor)**[1][2][3]
- **EC50 Content**: Contains **screening concentration or EC50 for all 75,050 experiments**
- **Data Organization**: 
  - *Parameter* column specifies "EC50" for dose-response measurements
  - *Value* and *Unit* columns contain EC50 values and units
  - Raw screening values vs. quantitative EC50 clearly distinguished
- **Access Method**: Filter function allows users to select only dose-response experiments with EC50 data
- **Scale**: Largest database with quantitative potency data available

**2. Human OR Dataset (Mainland et al.)**[4][5]
- **EC50 Content**: **Complete dose-response curves with EC50 values** for 25 validated OR-odorant pairs
- **Quality**: High-quality experimental validation with **standard deviation of fitted log EC50 < 1 log unit**
- **Data Format**: EC50 values listed in supplementary Table 1 with confidence intervals
- **Validation**: Stringent criteria including 95% confidence interval separation and extra sums-of-squares testing

### **Secondary Resources with Limited IC50/EC50 Data**

**3. Individual Research Studies with IC50/EC50 Measurements**
- **OR-EG Antagonism Study**: IC50 ≈ 119 μM for MIEG antagonism of mOR-EG, EC50 ≈ 52 μM for EG activation[6]
- **Large-Scale GPCR-Ligand Pairing**: Proteochemometric models with hit rates up to 58% validated through functional assays, though specific EC50 values not systematically reported[7]
- **Competitive Binding Studies**: Dose-response analysis with EC50 measurements for mixture interactions[8]

**4. GPCRdb (2025 Release)**[9][10][11]
- **Limited EC50 Data**: Contains **Δlog(Emax/EC50)** metrics for some olfactory receptors
- **Integration**: Links to ChEMBL and other databases that may contain quantitative potency data
- **Access**: Available through ligand bioactivity profiles section

### **Databases That Do NOT Contain IC50/EC50 Data**

The following databases were **excluded** as they lack quantitative potency measurements:

- **DoOR**: Drosophila response data without mammalian EC50/IC50 values
- **ORDB/OdorDB**: Qualitative receptor-ligand associations only
- **OlfactionBase**: Limited interaction data without potency measurements
- **Atomevo-odor**: Physicochemical properties without bioactivity values
- **STITCH**: Chemical-protein interactions without specific potency data
- **Pred-O3**: AI predictions without experimental IC50/EC50 validation data
- **iOBPdb**: Binding protein data, not receptor potency measurements

### **Quantitative Data Summary**

| **Database** | **EC50 Experiments** | **IC50 Experiments** | **Total Potency Measurements** | **Data Quality** |
|-------------|-------------------|-------------------|------------------------------|------------------|
| **M2OR** | ~15,000+ experiments | Limited | 75,050 total experiments | High - curated with metadata |
| **Mainland Dataset** | 25 validated pairs | 0 | 25 high-quality measurements | Very High - stringent validation |
| **Research Studies** | Variable per study | Variable per study | <100 total | Study-dependent |
| **GPCRdb** | Limited integration | Limited integration | Integration from external sources | Variable |

### **Recommended Data Acquisition Strategy**

**For Maximum EC50 Coverage:**
1. **Start with M2OR** - Use advanced search to filter for "Parameter = EC50" to access the largest collection of quantitative potency data[2]
2. **Supplement with Mainland Dataset** - Access high-quality, well-validated EC50 measurements for human ORs[4]
3. **Mine Individual Studies** - Search literature for specific IC50/EC50 studies in specialized contexts

**Data Access Instructions:**
- **M2OR**: Use Filter function → Select "Data Quality = EC50" → Download filtered results
- **Mainland Data**: Available in Scientific Data supplementary materials with R analysis scripts
- **GPCRdb**: Navigate to receptor-specific pages → Ligand bioactivity profiles section

### **Critical Limitation**

The olfactory receptor field has a **significant paucity of IC50 data** compared to EC50 measurements. Most studies focus on agonist potency (EC50) rather than antagonist activity (IC50), reflecting the historical emphasis on receptor activation rather than inhibition in olfaction research. This represents a major gap for comprehensive pharmacological characterization of olfactory receptors.[6]

The **M2OR database represents the gold standard** for accessing EC50 data in olfactory receptor research, containing the vast majority of available quantitative potency measurements in a searchable, well-curated format.[3][2]
 [1](https://www.graphpad.com/support/faq/50-of-what-how-exactly-are-ic50-and-ec50-defined/) [2](https://academic.oup.com/nar/article/52/D1/D1370/7327067) [3](https://m2or.chemsensim.fr/resource/how-to) [4](https://www.nature.com/articles/sdata20152) [5](https://journals.plos.org/ploscompbiol/article?id=10.1371%2Fjournal.pcbi.1006175) [6](https://pmc.ncbi.nlm.nih.gov/articles/PMC1271670/) [7](https://pubs.acs.org/doi/10.1021/acscentsci.1c01495) [8](https://www.pnas.org/doi/10.1073/pnas.1813230116) [9](https://gpcrdb.org/protein/Q8TCB6/) [10](https://gpcrdb.org/protein/O60431/) [11](https://pmc.ncbi.nlm.nih.gov/articles/PMC11701689/) [12](https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0131077) [13](https://www.sciencedirect.com/science/article/pii/S0753332224012885) [14](https://github.com/chemosim-lab/M2OR) [15](https://www.nature.com/articles/s41467-025-57034-y) [16](https://www.osti.gov/servlets/purl/1543816) [17](https://pmc.ncbi.nlm.nih.gov/articles/PMC4455932/) [18](https://pmc.ncbi.nlm.nih.gov/articles/PMC9022881/) [19](https://www.biorxiv.org/content/10.1101/2024.09.16.613334v1.full.pdf) [20](https://onlinelibrary.wiley.com/doi/full/10.1002/minf.202400274)
