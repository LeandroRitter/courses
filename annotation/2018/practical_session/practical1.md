# Foreword:

We will for all exercises use data for the fruit fly, Drosophila melanogaster, as that is one of the currently best annotated organisms and there is plenty of high quality data available. However, working on eukaryotes can be time consuming. Even a small genome like Drosophila would take too long to run within the time we have for this course. Thus to be sure to perform the practicals in good conditions, we will use the smallest chromosome of the drosophila (chromosome 4) like it was a whole genome.
An annotation project requires numerous tools and dependencies, which can take easily many days to install for a neophyte. For your convenience and in order to focus on the art of the ANNOTATION most of the tools are already installed on your machine (Thank you Hadrien :) ).

# First of all

Before going into the exercises below you need to connect to your virtual machine Ubuntu 16.04 following the instruction we will provide you.
Once connected you will move into the **annotation\_course** folder, where all the magic will happen.
```
cd ~/annotation_course
```

**Now you need the data !!** You must download the archive of the data and uncompress it (it could take few minutes).
```
wget https://u-ip-81-109.hpc2n.umu.se/tickets/La34or2kms3wMdf1Gp5HdbsmT4fFCIfayQeHvew8kaU/data.tar.gz/download
tar xzvf download
rm download
```

Now move into the **practical1** folder and you are ready to start for this morning !
```
cd ~/annotation_course/practical1
```

# 1. Assembly Check

Before starting an annotation project, we need to carefully inspect the assembly to identify potential problems before running expensive computes.
You can look at i) the Fragmentation (N50, N90, how many short contigs); ii) the Sanity of the fasta file (Presence of Ns, presence of ambiguous nucleotides, presence of lowercase nucleotides, single line sequences vs multiline sequences); iii) completeness using BUSCO; iv) presence of organelles; v) Others (GC content, How distant the investigated species is from the others annotated species available).
The two next exercices will perform some of these checks.

## 1.1 Checking the gene space of your assembly

BUSCO provides measures for quantitative assessment of genome assembly, gene set, and transcriptome completeness. Genes that make up the BUSCO sets for each major lineage are selected from orthologous groups with genes present as single-copy orthologs in at least 90% of the species.

***Note:*** In a real-world scenario, this step should come first and foremost. Indeed, if the result is under your expectation you might be required to enhance your assembly before to go further.

**_Exercise 1_ - BUSCO -:**

You will run BUSCO on chromosome 4 of Drosophila melanogaster.

First create a busco folder where you work:
```
mkdir busco
cd busco
```

Then visit the [busco website](http://busco.ezlab.org) and choose the best data set among the vast choice. Once you know which one you want to use, right click on it and copy the link to it. Then download the dataset.
/!\ Replace **http://busco.ezlab.org/datasets/metazoa_odb9.tar.gz** link by the link to the dataset you have chosen.
```
wget http://busco.ezlab.org/datasets/metazoa_odb9.tar.gz
tar xzvf metazoa_odb9.tar.gz
```

Now you are ready to launch BUSCO on our genome (reminder: the genome is the chormosome 4 called 4.fa).
```
BUSCO.py -i ~/annotation_course/data/genome/4.fa -o 4_dmel_busco -m geno -c 8 -l metazoa_odb9
```

While BUSCO is running, start the exercise 2.
When done, check the short\_summary\_4\_dmel\_busco. How many proteins are reported as complete? Does this sound reasonable?
**Tips**: the chromosome 4 corresponds to less than 1% of the real size of the genome.

## 1.2 Various Check of your Assembly

**_Exercise 2_ :**
Launching the following script will provide you some useful information.

```
fasta_statisticsAndPlot.pl -f ~/annotation_course/data/genome/4.fa
```

If you don't see any peculiarities, you can then decide to go forward and start to perform your first wonderful annotation.

# 2. Running an ab initio gene finder

Now we are satisfied by the quality of the assembly we can start the annotation.

<u>**Ab initio gene finders:**</u> These methods have been around for a very long time, and there are many different programs to try. We will in this exercise focus on the gene finder Augustus. These gene finders use likelihoods to find the most likely genes in the genome. They are aware of start and stop codons and splice sites, and will only try to predict genes that follow these rules. The most important factor here is that the gene finder needs to be trained on the organism you are running the program on, otherwise the probabilities for introns, exons, etc. will not be correct. Luckily, these training files are available for Drosophila.

**_Exercise 3_ - Augustus:**

Run Augustus on your genome file using:  
```
cd ~/annotation_course/practical1
mkdir augustus
cd augustus
augustus --species=fly ~/annotation_course/data/genome/4.fa --gff3=yes > augustus_drosophila.gff
```
if you wish to annotate isoforms too, use the following command:
```
augustus --species=fly ~/annotation_course/data/genome/4.fa --gff3=yes --alternatives-from-sampling=true > augustus_drosophila_isoform.gff
```

Take a look at the gff result file using the command ‘less augustus_drosophila.gff’. What kinds of features have been annotated? Does it tell you anything about UTRs?

To better understand what contains your gff file you may use a script that will provide you some statistics like this one:
```
gff3_sp_statistics.pl --gff augustus_drosophila.gff
```
How many genes have you annotated ?

It if of interest to view your annotation in a genome browser, this is more concrete and much nicer. A visual inspection is often the most effective way to assess the quality o your annotation.

Transfer the augustus\_drosophila.gff3 to your computer using scp in a new terminal:   
```
scp -i ~/.ssh/azure_rsa student@__IP__:/home/student/annotation_course/practical1/augustus/augustus_drosophila.gff .
```

We have made a genome browser called Webapollo available for you on the address [http://annotation-prod.scilifelab.se:8080/NBIS_course/](http://annotation-prod.scilifelab.se:8080/NBIS_course/).
Load the file in into the genome portal called **drosophila\_melanogaster\_chr4**. [Here find the WebApollo instruction](UsingWebApollo.md)
<br/>The official Ensembl annotation is available in the genome browser.
How does the Augustus annotation compare with the Ensembl annotation? Are they identical?

**_Exercise 4 -_ Augustus with yeast models:**  
Run augustus on the same genome file but using settings for yeast instead (change species to Saccharomyces).

Load this result file into Webapollo and compare with your earlier results. Can you based on this draw any conclusions about how a typical yeast gene differs from a typical Drosophila gene?

# Closing remarks

We have seen how to assess the quality of the assembly and how to launch a quick annotation using an abinitio tool.
We have also seen the importance to use a species specific hmm model into the ab initio tool. Thus, the limitation of this approach is linked to the pre-trained species that are available.
