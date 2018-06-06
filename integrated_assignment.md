# Introduction

By now you've been introduced to a few pipelines for analyzing both amplicon
and shotgun metagenomics sequencing data. For this assignment we'll be
re-visiting these pipelines, but we will not be providing all of the commands
for you to copy-and-paste. Two key skills for bioinformaticians to have is to be able
to adapt existing pipelines and to quickly learn how to use new tools. You'll need to
showcase both of these skills to complete this assignment!

The dataset we'll be running for this assignment is sequencing data from
mammalian stool samples from the Shubenacadie Wildlife Park in Nova Scotia,
Canada. This data was published as part of [this paper](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0189404).
We'll be processing the raw 16S data using the commands you learned yesterday.
To save time, we'll be starting with output files from each pipeline rather than raw FASTQs.

# 16S rRNA Gene Analyses

In this part of the integrated assignment, we will be analyzing a new 16S sequencing dataset of human stool microbiome using what we learned in module 3. We will be starting with a FASTA file containing the ASV sequences, and a tab-delimited file containing the ASV abundance data. The same tools will be used, but the commands to use them will not all be provided! One part of the exercise is to use the correct commands and parameters, and the other is to read and understand the main output files.

We encourage you to not refer back to the commands provided in the presented modules, but instead to read the help functions of each function (unless you're stuck!).

### Exploring input files

Similar to the Module 3 Lab, we have two starting files
* ```16S_abun.tsv```
* ```16S.fna```

These two files can be downloaded using the following command:
```
wget  -O int_assignment_input.tar.gz https://www.dropbox.com/s/zk4j0exhgkcuy0k/int_assignment_input.tar.gz?dl=1
```
Note that this will download a compressed file to your current folder, so navigate to your working directory first. The files can be extracted with the following command:

```
tar -xzf int_assignment_input.tar.gz
```

**Q1: How many ASVs are in this data set?**

Now we'll read these files into QIIME2.

First, activate the QIIME2 environment:
```
source /usr/local/miniconda3/bin/activate qiime2-2018.4
```

Then to convert ("import") the BIOM table (in tab-delimited format) to a QIIME2 artifact, use these commands:

To convert from tab-delimited to HDF5 BIOM:
```
biom convert -i 16S_abun.tsv -o 16S_abun.biom --to-hdf5
```

```
qiime tools import \
  --input-path 16S_abun.biom \
  --type 'FeatureTable[Frequency]' \
  --source-format BIOMV210Format \
  --output-path 16S_abun.qza
 ```
Similarly, to convert the FASTA file to a QIIME2 artifact, you can use this command:
``` 
qiime tools import \
  --input-path 16S.fna \
  --output-path 16S_sequences.qza \
  --type 'FeatureData[Sequence]'
```

We wont be doing much with QIIME2, but to let you explore the basic analysis output you can run this command:

```
qiime diversity core-metrics   --i-table 16S_abun.qza   --p-sampling-depth 3220   --m-metadata-file sample_species_links.txt   --output-dir core-metrics-results
```

Take a look at the Bray-Curtis output visualization file.

**Q2 What percent of variation is explained by PC1 on this plot?**

Convert observed_otus_vector.qza output file to a textfile with the ```qiime tools export``` command.

**Q3 Which sample has the highest richness (i.e. number of independent ASVs) and how many ASVs does it have?**

Next we'll run PICRUSt2 on this dataset. This analysis will consist of the same steps as module 3:

1. Read placement
2. Hidden state prediction
3. Metagenome prediction
4. Pathway inference

### Activating the PICRUSt2 conda environment

The picrust2 environment can be activated with the following command:
```
source /usr/local/miniconda3/bin/activate picrust2-dev
```

## Read placement

First, we will be placing our ASVs within the reference tree using ```place_seqs.py```. The parameters required can be found with ```place_seqs.py --help```

Remember what you name your output file, you'll need it soon! Also, you can use up to 4 threads on Amazon Cloud.

**Q4: Nodes in the newick (.tre) output file are in the format X:Y. What do these two numbers mean?**

## Hidden-state prediction

The next step is hidden-state prediction for the abundances of gene families of interest using ```hsp.py```. Refer to the help section! 

Note that we will be running in the maximum parsimony (mp) mode with a fixed seed for both the 16S gene as well as genes in all EC families. The EC predictions will be a bit slow, so it's recommended to do the 16S first. 

**Q5: For the 16S gene, which ASV has the highest NSTI? What does this mean?**

## Metagenome pipeline

Next, we will be predicting the metagenomes of each sample from the predicted genomes using ```metagenome_pipeline.py```. Both the 16S and EC predictions from the previous step will be needed. Remember to consult the help function! The output will be in ```./metagenome_out```. 

**Q6: What is in each of the output files?**

**Q7: Which sample appears to have the highest capacity for the EC family 1.1.1.108? Hint: Use grep**

## Pathway inference

The final step is to infer pathway abundances based on the presence of gene families using ```run_minpath.py``` - a wrapper script for [MinPath](http://omics.informatics.indiana.edu/MinPath/).

The MinPath map file we're using is at ```/usr/local/picrust2/MinPath/ec2metacyc_picrust_prokaryotic.txt```. What should be the input?

**Q8: How many species in sample iGEM-4 contribute to the community-wide abundance of the '12DICHLORETHDEG' pathway?**

Congratulations, you've completed the 16S section of this assignment! You can celebrate by deactivating the PICRUSt2 conda environment:
```
source /usr/local/miniconda3/bin/deactivate
```

# Shotgun metagenomics

Several of the samples we analyzed above were also sequenced using shotgun
metagenomics sequencing. This assignment will expand on some of the questions
in the module 4 tutorial and help you become comfortable running basic analyses on this data.
Rather than have you run these files through the pipeline
yourself, we've already run the data and you can copy the output files to
your working directory with this command:

```
cp -R /home/ubuntu/CourseData/metagenomics/integrated_assignment/mgs_output/ ./
```

You won't have permissions to write to this directory currently. Run this command to fix the
permissions:

```
chmod -R 755 mgs_output/
```

Take a look at the files in this directory. You should see the log output files
for kneaddata, the merged MetaPhlAn2 table, and the three key HUMAnN2
output files.

**Q1: Take a look at the kneaddata output first - which sample has the fewest reads
after pre-processing and how many paired-end reads does it have?**

**Q2: Now inspect the MetaPhlAn2 output file - how many species were identified?**

We'll read the MetaPhlAn2 table into R now to better explore the species
abundances. Since we're only interested in the species-level abundances, we'll
make a new table with only the species rows.

The header can be written to this new file with this command:

```
head -n 1 metaphlan2_merged.txt > metaphlan2_merged_species.txt
```

Write the rows containing species abundances to this new file (note that ```>>```
means append, whereas using ```>``` would overwrite the existing file).

Once this file is created, you can read it into RStudio with this **R command**:

```{r }
mgs_sp <- read.table("/path/to/mgs_output/metaphlan2_merged_species.txt",
                     header=T, sep="\t", comment.char = "", row.names=1)
```

At all taxonomic levels in the MetaPhlAn2 output the relative abundance should
sum to 100.

**Q3: Use the ```ColSums``` R function on this input dataframe. Which sample's
species relative abundances are furthest from summing to 100?**

Some post-processing must have already been done on this table! You can use
this R command to re-normalize the table so that all columns sum to 100:

```{r }
mgs_sp_relab <- data.frame(sweep(mgs_sp, 2, colSums(mgs_sp), `/`)) * 100
```

One of your first steps whenever you start using a new dataset should be to
explore the data and identify outlier samples. R allows some easy ways to
explore this dataset. For instance, the below command will return the number of species
with abundance over 25% in each sample.

```{r }
colSums(mgs_sp_relab > 25)
```

**Q4: Which sample has the fewest observed species?**

**Q5: In this sample with the fewest observed species what genus explains the vast
majority of taxonomically annotated reads?** Note: you could either use R or you could
open the species abundance table you created in Excel to answer this question.

We would expect that stool samples from the same mammalian species would have
more similar microbial profiles. This command will make a scatterplot between
two Artic Wolf samples:

```{r }
plot(mgs_sp_relab$A1, mgs_sp_relab$A2, pch=16, xlab="A1", ylab="A2")
```

Similarly, the Spearman's correlation between these two samples can be calculated
with this command:

```{r }
cor.test(mgs_sp_relab$A1, mgs_sp_relab$A2, method="spearman")
```

**Q6: What's the Spearman's correlation coefficient between the coyote samples C1 and C2? Between C1 and the beaver sample B1?**

Now let's take a look at the HUMAnN2 output files. Using similar commands as in
the Module 4 tutorial normalize the pathway abundances to be relative abundances.
Then create a PCL table for this dataset (using the metadata file here: ```/home/ubuntu/CourseData/metagenomics/integrated_assignment/sample_species_links.txt```)

Hints: 
* The above metadata file is **tab**, not space, delimited.
* You can get the overlapping columns between the pathway abundance and metadata file with this command: ```overlap_col <- colnames(in_meta_t)[which(colnames(in_meta_t) %in% colnames(in_path))]```
* The pathway abundance columns don't end in "\_Abundance" in this example.

If you're really having trouble with this step you can download the PCL file [here](https://www.dropbox.com/s/wyzvo787w0wvmss/humann2_pathabundance_relab_meta.pcl?dl=1).

Using this PCL file you can run some analyses on the pathway abundances using built-in HUMAnN2 functions.

**Q7: Make a stacked barchart of ```PWY-6609: adenine and adenosine salvage III``` with samples sorted by mammalian species. Is there evidence that this pathway differs in abundance in the Canidae compared to the other mammals?**

HUMAnN2 also provides an in-house way to test for statistical associations between metadata and the features with the command ```humann2_associate```.
As before, take a look at the description of inputs to this command with ```humann2_associate --help | less```. Run associations with this program on the PCL file
we made above.

**Q8: What Q-value was returned for ```ARGININE-SYN4-PWY: L-ornithine de novo  biosynthesis```?**
