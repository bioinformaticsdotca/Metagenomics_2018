These answers are for the CBW Metagenomics [integrated assignment](https://github.com/gavinmdouglas/Metagenomics_2018/blob/master/integrated_assignment.md).

### 16S:

1. 371

2. 42.62% of the variation is explained by PC1

3. Sample D2 has the highest richness (134 ASVs)

4. 16S rRNA ID and Branch Lenght, respectively

5. Sequence 140dbee0860b80c6b3a86816d06dd24c (has an NSTI of 0.804256). This means that this sequence is distant/dissimilar to known taxa. It could be an uncharacterized taxa, but often sequences like this are simply garbage sequences.

6. Sample B1 has the highest capacity for EC 1.1.1.108
 * ```pred_metagenome_strat.tsv```: predicted metagenomes stratified by which sequence is contributing each function.
 * ```pred_metagenome_unstrat.tsv```: predicted metagenomes over all sequences.
 * ```seqtab_norm.tsv```: sequence table normalized by predicted number of marker genes.
 * ```weighted_nsti.tsv```: weighted NSTI values per sample (over all sequences weighted by the abundance in that sample).

7. 18 species in sample iGEM-4 contribute to the community-wide abundance of the '12DICHLORETHDEG' pathway

### MGS:

1. Sample A2 has the fewest filtered reads (2548811 pairs).

2. 69 independent species were identified.

3. Sample C2's relative abundane values are the furthest from summing to 100.

4. Sample B1 has the fewest observed species (3), which you could figure out by typing: ```colSums(mgs_sp_relab > 0)```.

5. _Pseudomonas_.

6. Between C1 and C2: 0.6504148. Between C1 and B1: -0.159787 (and not significant!)

7. Yes this pathway is at high relative abundance in both artic wolves and coyotes, but is low or absent in the other mammals.

7. 0.1038
