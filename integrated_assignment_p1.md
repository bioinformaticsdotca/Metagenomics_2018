## Integrated Assignment, part I
based on the Module 3 Lab Practical by Gavin Douglas and Morgan Langille

In this part of the integrated assignment, we will be analyzing a new 16S sequencing dataset of mammalian stool microbiome using what we learned in Module 3. We will be starting with the same input files - a FASTA sequence files containing the ASVs, and a TSV file containing the abundance data. The analysis will consist of the same steps:

1. Read placement
2. Hidden state prediction
3. Metagenome prediction
4. Pathway inference

The same tools will be used, but the commands to use them will not be provided! One part of the exercise is to use the correct commands and parameters, and the other is to read and understand the main output files.

We encourage you to not refer back to the commands provided in Module 3, but to read the help functions of each function instead. However, if you're stuck at any point, it will probably help. 

### Activating the conda environment

The picrust2 environment can be activated with the following command:
```
source /usr/local/miniconda3/bin/activate picrust2-dev
```

### Exploring input files

Similar to the Module 3 Lab, we have two starting files
* ```16s_abun.tsv```
* ```16s.fna```

These two files can be downloaded using the following command:
```
wget https://www.dropbox.com/s/ll7qz6ag2y2zydu/16s.tar.gz
```
Note that this will download a compressed file to your current folder, so navigate to your working directory first. The files can be extracted with the following command:

```
tar -xzf input.tar.gz
```

**Question 1: How many ASVs are in this data set?**

## Read placement

First, we will be placing our ASBs within the reference tree using ```place_seqs.py```. The parameters required can be found with ```place_seqs.py --help```

Remember what you name your output file, you'll need it soon! Also, you can use up to 4 threads on Amazon Cloud.

**Question 2: Nodes in .tre output file are in the format X:Y. What do the two numbers mean?**

## Hidden-state prediction

The next step is hidden-state prediction for the abundances of gene families of interest using ```hsp.py```. Refer to the help section! 

Note that we will be running in the maximum parsimony (mp) mode with a fixed seed for both the 16S gene as well as genes in all EC families. The EC predictions will be a bit slow, so it's recommended to do the 16S first. 

**Question 3: For the 16S gene, which ASV has the highest NSTI? What does this mean?**

## Metagenome pipeline

Next, we will be predicting the metagenomes of each sample from the predicted genomes using ```metagenome_pipeline.py```. Both the 16S and EC predictions from the previous step will be needed. Remember to consult the help function! The output will be in ```./metagenome_out```. 

**Question 4: What is in each of the output files?**

**Question 5: Which sample appears to have the highest capacity for the EC family 1.1.1.108? Hint: Use grep**

## Pathway inference

The final step is to infer pathway abundances based on the presence of gene families using ```run_minpath.py``` - a wrapper script for [MinPath](http://omics.informatics.indiana.edu/MinPath/).

The MinPath map file we're using is at ```/usr/local/picrust2/MinPath/ec2metacyc_picrust_prokaryotic.txt```. What should be the input?

**Question 6: How many species in sample iGEM-4 is predicted to contribute to the '12DICHLORETHDEG' pathway?**

Congratulations, you've just analyzed your own 16S sequence data!
