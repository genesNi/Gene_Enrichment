## Objective: 
I want to compare normal fertile female with endometriosis in Follicular and Luteal Phases for that I have taken 2 high throughput datasets GSE98386 and GSE282532, since the techniques in both of them differs one has whole transcriptome analysis and other with only Bulk Rna Sequencing so I will conduct separate analysis and then compare these two datasets. I started with my Primary question for first dataset  GSE282532 , What are the differential expression patterns of mRNAs, microRNAs, and lncRNAs between ectopic and eutopic endometrium in ovarian endometriosis. How do microRNAs and lncRNAs regulate mRNA expression in endometriotic lesions? What regulatory networks (ceRNA networks) are dysregulated in endometriosis? Which altered genes/RNAs could serve as therapeutic targets or biomarkers?

# what is endometriosis ?

# step 1: Download all the samples
I use ENA (European Nucleotide Archive) repository to directly download Fastq files using downloading wget links which I pasted in a txt file and looped the file to download 2 samples at a time and to check or wait if internet throughs any issue.

# step 2: Lets see the quality of fastq report 
I ran tool fastqc for each sample but reading all the reports one by one is not possible so lets combine them 

# step 3: combine all Fastqc reports using Multiqc 
I ran multiqc which combined all my fastqc reports in one Multiqc report.
This report consists of


one of the thing which was disrupted was GC content in each sample due to Whole Transcriptome , the graphs did not follow normal distribution.

# step 4: Lets Align the samples with an aligner called Hisat2
I used hisat2 for its efficiency and lesser ram requirements but it has to be done one sample at a time so that your laptop does not crash

# step 5: Lets count the read using Feature Counts
I cleaned the final txt files which kept only columns I needed.

# step 6: Merge all your txt files from feature counts in R studio
Bunch of packages are required
 ` Use this syntax for installing any packages which is part of Bioconductor 
 if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager") `

(biomaRt)
(rappdirs)
(dplyr)
(tidyverse)
(DESeq2)
(ggplot2)
(patchwork)
(org.Hs.eg.db)
(clusterProfiler)
(ggrepel)
(AnnotationDbi)

# step 7: I wanted to separate mrna counts, mirna counts, lncrna counts using package biomaRt
now after separating lets find out DEGS in each counts
so I went for DESeq2 looking at the sample size and data
# step 8: lets check p value histograms and decide 
check MA plot which is an horizontal scatter plot to analyse your result 
check PCA plot ,the variance and whether the clusters are different or clubbing , look for batch effects
ectopic and eutopic have different clusters , no batch effects but eutopic cluster has one cluster a bit far which I consider to be difference of days when samples are collected.
make a heatmap and check the gene expression in each sample

# step 9: lets compare different padj values and check for degs 
usually more citated papers go for 5/100 false positive result which is padj<0.05 but for your own anaysis can go for 0.01 
and log2fold change to be 1 or 2

# step 10: lets do functional enrichment because now you know all your differentially expressed genes 
it includes Gene Ontology (Biological Pathway, Molecular Pathway, Cellular Component ) , Pathway Analysis (KEGG, WIKI), GSEA (Gene set enrichment analysis) 
