---
layout: default
title:  'Index'
---

# Welcome to NBIS scRNA-seq tutorial packages

This page contains links to different tutorials that are used in the scRNA-seq analysis course.

For many of the packages we recommend that you follow the tutorials supplied by the different packages. But if you have brought your own data to the tutorials, we have included some examples on how to run these tutorials based on your own dataset and also some suggestions on which parts of the tutorials to focus on.


### Converting gene names to Ensembl IDs

For those not familiar with working with biomaRt, we suggest that you have a look at this example code for how to convert between different formats using biomaRt. 
 
*	[Tutorial for biomaRt](biomart) 

### scater package

Tutorial with the scater package for QC of scRNA-seq data

*	[Tutorial for scater](scater_ilc)

### SC3 package

Tutorial with the SC3 consensus clustering package

*	[Tutorial for SC3](sc3_ilc)

### Pagoda package

Pagoda patway wPCA for clustering of cells. OBS! several steps in this tutorial takes hours to run if you work with your own dataset, a good suggestion is to start with the first steps, knn.error.model, pagoda.varnorm and pagoda.pathway.wPCA and let it run while working on other tutorials. You can also run it with more than one core to speed things up.
 
*	[Tutorial for Pagoda](pagoda_ilc)

### Seurat package

Tutorial for Seurat package with normalization, dimensionality reduction and clustering.

*       [Tutorial for Seurat](seurat_analysis)

## UPPMAX
 
 One example of a sbatch script
 
 *  [sbatch scripts](sbatchScript)   
  
 
## Caveat

We will try to keep these tutorials up to date. If you find any errors or things that you think should be updated please contact Asa (asa.bjorklund@scilifelab.se) 
  		
