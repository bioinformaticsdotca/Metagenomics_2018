# UNDER CONSTRUCTION

## Introduction

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
To save time, we'll be analyzing the output of HUMAnN2 and MetaPhlAn2 instead
of processing the raw shotgun metagenomics data.

## Shotgun metagenomics

Several of the samples we analyzed above were also sequenced using shotgun
metagenomics sequencing. Rather than have you run these files through the pipeline
yourself, we've already run the data and you can copy the output files to
your working directory with this command:

```
cp -R /home/ubuntu/CourseData/metagenomics/integrated_assignment/mgs_output/ ./
```

You won't have permissions to write to thi directory currently. Run this command to fix the
permissions:

```
chmod -R 755 mgs_output/
```

Take a look at the files in this directory. You should see the log output files
for kneaddata, the merged MetaPhlAn2 table, and the three key HUMAnN2
output files.

**QX: Take a look at the kneaddata output first - which sample has the fewest reads
after pre-processing and how many paired-end reads does it have?**

**QX: Now inspect the MetaPhlAn2 output file - how many species were identified?**

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

**QX: Use the ```ColSums``` R function on this input dataframe. Which sample's
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

**QX: Which sample has the fewest observed species?**

**QX: In this sample with the fewest observed species what genus explains the vast
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

**QX: What's the Spearman's correlation coefficient between the coyote samples C1 and C2? Between C1 and the beaver sample B1?**

A heatmap is also an excellent way to take a first look at your data. Fortunately
MetaPhlAn2 provides this functionality out of the box with the script ```metaphlan_hclust_heatmap.py```.

Take a look at the help description of this tool with (**back on the Linux command-line**):

```
metaphlan_hclust_heatmap.py --help | less
```

**QX: Make a heatmap of the _genera_ relative abundances across your samples.
Keep all other options of this script as default.**

Now let's take a look at the HUMAnN2 output files. Using similar commands as in
the Module 4 tutorial make a PCL table for this dataset (using the metadata file here: ```/home/ubuntu/CourseData/metagenomics/integrated_assignment/sample_species_links.txt```)

Using this PCL file you can run some analyses on the pathway abundances using built-in HUMAnN2 functions. However, first you'll need to normalize the pathway abundances as done in the Module 4 tutorial.

**QX: Make a stacked barchart of ```ARGININE-SYN4-PWY: L-ornithine de novo  biosynthesis``` with samples sorted by mammalian species.**

HUMAnN2 also provides an in-house way to test for statistical associations between metadata and the features with the command ```humann2_associate```.
As before, take a look at the description of inputs to this command with ```humann2_associate --help | less```. Run associations with this program on the PCL file
we made above.

**QX: Is the pathway ```VALSYN-PWY: L-valine biosynthesis``` significantly different depending on the species metadata?**
