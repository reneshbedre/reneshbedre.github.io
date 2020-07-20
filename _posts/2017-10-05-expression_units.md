---
title: "Gene expression units explained: RPM, RPKM, FPKM, TPM, <i>DESeq</i>, TMM, SCnorm, GeTMM, and ComBat-Seq"
date:   2017-10-05
author_profile: true
permalink: blog/expression_units.html
classes: wide
---

<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML" async></script>

<p>
{% include  share.html %}
</p>

- In RNA-seq gene expression data analysis, we come across various expression units such as RPM, RPKM, FPKM, TPM, TMM, and raw reads counts.
- Most of the times it's difficult to understand the basic underlying  methodology to calculate these units from mapped sequence data.
- I have seen a lot of posts of such normalization questions and their confusion among readers. Hence, I attempted here to explain these units
  in a much simpler way (avoided complex mathematical expressions).


## <span style="color:#33a8ff">Why different normalized expression units?</span> ##

The expression units provide a digital measure of the abundance of transcripts. Normalized expression units are necessary to remove
technical biases in sequenced data such as depth of sequencing (more sequencing depth produces more read count for gene expressed at
same level) and gene length (differences in gene length generate unequal reads count for genes expressed at the same level; longer the
gene more the read count).


## <span style="color:#33a8ff"> Gene expression units and calculation </span> ##

 **<span style="color:#060606">RPM or CPM (Reads per million mapped reads or Counts per million mapped reads) </span>**

 <p align="center">
  \(  \text{RPM or CPM} = \frac{ \text{Number of reads mapped to gene}  \times 10^6}{\text{Total number of mapped reads}}  \)
  </p>

<!--
 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;RPM&space;=&space;\frac{Number&space;\&space;of&space;\&space;reads&space;\&space;mapped&space;\&space;to&space;\&space;gene&space;\times&space;10^6}{Total&space;\&space;number&space;\&space;of&space;\&space;mapped&space;\&space;reads}" />
-->

For example, You have sequenced one library with 5 million(M) reads. Among them, total 4 M matched to the genome sequence and 5000 reads matched to a given gene.

 <p align="center">
  \(  \text{RPM or CPM} = \frac{ 5000 \times 10^6}{4 \times 10^6} = 1250 \)
  </p>

<!--
 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;\large&space;RPM&space;=&space;\frac{5000&space;\times&space;10^6}{4&space;\times&space;10^6}&space;=&space;1250" />
-->

Notes:

 - RPM does not consider the transcript length normalization.
 - RPM Suitable for sequencing protocols where reads are generated irrespective of gene length


**<span style="color:#060606">RPKM (Reads per kilo base per million mapped reads)</span>**

 <p align="center">
  \(  \text{RPKM} = \frac{ \text{Number of reads mapped to gene}   \times 10^3  \times 10^6 }{\text{Total number of mapped reads} \times \text{gene length in bp }}  \)
  </p>

<!--
 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;RPKM&space;=&space;\frac{Number&space;\&space;of&space;\&space;reads&space;\&space;mapped&space;\&space;to&space;\&space;gene&space;\times&space;10^3&space;\times&space;10^6}{Total&space;\&space;number&space;\&space;of&space;\&space;mapped&space;\&space;reads&space;\times&space;gene&space;\&space;length&space;\&space;in&space;\&space;bp}" />
-->

 <p>Here, \( 10^3 \) normalizes for gene length and 10^6 for sequencing depth factor.</p>

FPKM (Fragments per kilo base per million mapped reads) is analogous to RPKM and used especially in paired-end RNA-seq experiments. In paired-end RNA-seq experiments,  
two (left and right) reads are sequenced from same DNA fragment. When we map paired-end data, both reads or only one read with high quality from a fragment can map to  
reference sequence. To avoid confusion or multiple counting, the fragments to which both or single read mapped is counted and represented for FPKM calculation.

For example, You have sequenced one library with 5 M reads. Among them, total 4 M matched to the genome sequence and 5000 reads matched to a given gene with a length of 2000 bp.
  <p align="center">
  \(  \text{RPKM} = \frac{ 5000 \times 10^3 \times 10^6}{4 \times 10^6 \times 2000} = 625 \)
     </p>

<!--
 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;RPKM&space;=&space;\frac{5000&space;\times&space;10^3&space;\times&space;10^6}{4&space;\times&space;10^6&space;\times&space;2000}&space;=&space;625" />
-->
Notes:

 - RPKM considers the gene length for normalization
 - RPKM is suitable for sequencing protocols where reads sequencing depends on gene length
 - Used in single-end RNA-seq experiments (FPKM for paired-end RNA-seq data)
 - RPKM/FPKM can be biased towards identifying the differentially expressed genes (Bullard et al., 2010)


**<span style="color:#060606">TPM (Transcript per million)</span>**

<p align="center">
  \(  \text{TPM} = A \times \frac{1}{\sum(A)} \times 10^6 \)
    <br><br>
    \(  \text{Where A} =  \frac{\text{total reads mapped to gene} \times 10^3}{\text{gene length in bp}} \)
  </p>

<!--
 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;TPM&space;=&space;\frac{Number&space;\&space;of&space;\&space;reads&space;\&space;mapped&space;\&space;to&space;\&space;gene&space;\times&space;read&space;\&space;length&space;\times&space;10^6}{Total&space;\&space;number&space;\&space;of&space;\&space;transcripts&space;\&space;sampled&space;\times&space;gene&space;\&space;length&space;\&space;in&space;\&space;bp}" />


Here, read length refers to the average number of nucleotides mapped to a gene.

For example, You have sequenced one library with 5M 100 bp reads. Among them, total 4M matched to the genome sequence and 5000 reads matched to a given gene  
with a length of 2000 bp. There were 10K transcripts were sampled from a genome sequence i.e. reads mapped to 10K genes. Suppose all 100 bp mapped from 5000 reads.

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;TPM&space;=&space;\frac{5000&space;\times&space;100&space;\times&space;10^6}{10000&space;\times&space;2000}&space;=&space;25000" />
-->
Notes:

 - TPM considers the gene length for normalization
 - TPM proposed as an alternative to RPKM due to inaccuracy in RPKM measurement (Wagner et al., 2012)
 - TPM is suitable for sequencing protocols where reads sequencing depends on gene length


**<span style="color:#060606">TMM (Trimmed Mean of M-values)</span>**
- TMM is a between-sample normalization method in contrast to within-sample normalization methods (RPM, RPKM/FPKM, or TPM)
- TMM normalization method assumes that most of the genes are not differentially expressed
- TMM normalize the total RNA output among the samples and does not consider gene length or library size for normalization
- TMM considers sample RNA population and effective in normalization of samples with diverse RNA repertoires (e.g. samples from
  different tissues). TMM will be good choice to remove the batch effects while comparing the samples from different tissues or genotypes or in cases
  where RNA population would be significantly different among the samples.
- To calculate TMM,
    - get the library size normalized read count for each gene in each sample
    - calculate the log2 fold change between the two samples (M value)

        <p align="center">
        <br>
            \(  \text{M} = log_2 \frac{\text{treated sample count}}{\text{control sample count}} \)
        </p>
    - get absolute expression count (A value)
        <p align="center">
            <br>
          \(  \text{A} =  \frac{log_2(\text{treated sample count})+log_2(\text{control sample count})}{2} \)
          </p>
    - Now, double trim the upper and lower percentages of the data (trim M values by 30% and A values by 5%)
    - Get weighted mean of M after trimming and calculate normalization factor ( see Robinson et al., 2010 for details)
- TMM is implemented in edgeR and performs better for between-samples comparisons
- edgeR does not consider gene length for normalization as it assumes that the gene length would be constant 
  between the samples

TMM normalization using edgeR,

```r
# I am using R version 3.6.3 (2020-02-29) 
# load library
library(edgeR)
# load sugarcane RNA-seq expression dataset (Published in Bedre et al., 2019)
x <- read.csv("https://reneshbedre.github.io/assets/posts/gexp/df_sc.csv",row.names="gene")
# delete last column (gene length column)
x <- x[,-7]
head(x)
                      ctr1 ctr2 ctr3 trt1 trt2 trt3
Sobic.001G000200.v3.1  338  324  246  291  202  168
Sobic.001G000400.v3.1   49   21   53   16   16   11
Sobic.001G000700.v3.1   39   49   30   46   52   25
Sobic.001G000800.v3.1  530  530  499  499  386  264
Sobic.001G001000.v3.1   12    3    4    3   10    7
Sobic.001G001132.v3.1    4    2    2    3    4    1
# comparing groups
group <- factor(c('c','c', 'c', 't', 't', 't'))
y <- DGEList(counts=x, group=group)
# normalize for library size by cacluating scaling factor using TMM (default method)
y <- calcNormFactors(y)
# normalization factors for each library
y$samples
     group lib.size norm.factors
ctr1     c  3357347    1.0290290
ctr2     c  3185467    0.9918449
ctr3     c  3292872    1.0479952
trt1     t  3141934    0.9651681
trt2     t  2720231    0.9819187
trt3     t  1762881    0.9864858

# count per million read (normalized count)
norm_counts <- cpm(y)
head(norm_counts)
                            ctr1        ctr2        ctr3        trt1       trt2        trt3
Sobic.001G000200.v3.1  97.834690 102.5482223  71.2854613  95.9605999  75.625815  96.6040717
Sobic.001G000400.v3.1  14.183135   6.6466440  15.3582498   5.2761842   5.990164   6.3252666
Sobic.001G000700.v3.1  11.288618  15.5088361   8.6933489  15.1690295  19.468032  14.3756059
Sobic.001G000800.v3.1 153.409425 167.7486352 144.5993708 164.5509944 144.512696 151.8063984
Sobic.001G001000.v3.1   3.473421   0.9495206   1.1591132   0.9892845   3.743852   4.0251697
Sobic.001G001132.v3.1   1.157807   0.6330137   0.5795566   0.9892845   1.497541   0.5750242
``` 


**<span style="color:#060606"><i>DESeq</i> or <i>DESeq2</i> normalization (median-of-ratios method)</span>**
- The <i>DESeq</i> (and also <i>DESeq2</i>) normalization method is proposed by Anders and Huber, 2010 and is similar to TMM
- <i>DESeq</i> normalization method  also assumes that most of the genes are not differentially expressed
- The <i>DESeq</i> calculates size factors for each sample to compare the counts obtained from different samples with
  different sequencing depth
- <i>DESeq</i> normalization uses the median of the ratios of observed counts to calculate size factors. Briefly,
  the size factor is calculated by first dividing the observed counts for each sample by its geometric mean. The size factor
  is then calculated as the median of this ratio for each sample. This size factor then used for normalizing raw
  count data for each sample.
- <i>DESeq</i> or <i>DESeq2</i> does not consider gene length for normalization as it assumes that the gene length
  would be constant between the samples.
- <i>DESeq</i> or <i>DESeq2</i> performs better for between-samples comparisons  


**<span style="color:#060606">SCnorm for single cell RNA-seq (scRNA-seq)</span>**
- The normalization units explained above works best for bulk RNA-seq and could be biased for scRNA-seq due to
  abundance of non-zero expression counts, variable count-depth relationship (dependence of gene expression on sequencing depth),
  and other unwanted technical variations
- Bacher et al., 2017 proposed a SCnorm, a robust and accurate between-sample normalization unit for scRNA-seq
- Steps involved in SCnorm normalization;
  - SCnorm requires the raw expression counts (not-normalized), which can be obtained from RSEM or HTSeq
  - Genes with low expression counts are filtered out (keep the genes with atleast 10 non-zero expression counts)
  - estimate the count-depth relationship using quantile regression
  - Cluster genes into groups with similar count-depth relationship
  - A scale factor is calculated from each group and used for estimation for normalized expression
- <a href="https://bioconductor.org/packages/devel/bioc/html/SCnorm.html" target="_blank">SCnorm</a>
  is implemented in R package and is available on Bioconductor

**<span style="color:#060606">ComBat-Seq method</span>**
- Zhang et al., 2020 proposed a ComBat-Seq (batch  effect  adjustment  method) approach to addresses the large variance of
  batch effects present in RNA-seq count data (the paper is still in preprint)
- The benefit of ComBat-Seq is that it adjusts the batch effects for raw counts data and provide the output
  as integer counts in contrast to other normalization methods which can produce fraction count values as described above (e.g. RPKM, TPM, TMM)
- The resulting batch adjusted integer counts can be directly used with <i>DESeq2</i> which accepts only integer count data
  for differential gene expression analysis
- ComBat-Seq takes input as a raw un-normalized data as input and addresses the batch effects using
  a negative binomial regression model
- Briefly, ComBat-Seq adjust the count data by comparing the quantiles of the  empirical distributions of data to the
  expected distribution without batch effects in the data
- <a href="https://github.com/zhangyuqing/ComBat-seq" target="_blank">ComBat-Seq</a> is available in R

**<span style="color:#060606">GeTMM method</span>**
- Smid et al., 2018 proposed a GeTMM (Gene length corrected TMM)  which works better for both between-samples and 
  within-sample gene expression analysis
- GeTMM is based on the TMM normalization but allows the gene length correction which lacks in TMM and <i>DESeq</i> or 
  <i>DESeq2</i>
- In GeTMM, calculate RPK for each gene from raw read count data which is then corrected by TMM normalization factor and
  scaled to per million reads (See Smid et al., 2018 for detailed calculation) 
  
GeTMM normalization using edgeR,

```r
# I am using R version 3.6.3 (2020-02-29) 
# load library
library(edgeR)
# load expression dataset (Published in Bedre et al., 2019)
x <- read.csv("https://reneshbedre.github.io/assets/posts/gexp/df_sc.csv",row.names="gene")
# calculate reads per Kb of gene length (corrected for gene length)
rpk <- (x[,1:6]/x[,7])
# delete last column (gene length column)
x <- x[,-7]
# comparing groups
group <- factor(c('c','c', 'c', 't', 't', 't'))
y <- DGEList(counts=rpk, group=group)
# normalize for library size by cacluating scaling factor using TMM (default method)
y <- calcNormFactors(y)
# normalization factors for each library
y$samples
    group  lib.size norm.factors
ctr1     c 1709.9624    1.0768814
ctr2     c 1674.1908    0.9843628
ctr3     c 1715.2323    1.0496274
trt1     t 1638.5170    0.9841850
trt2     t 1467.5495    0.9432723
trt3     t  935.1252    0.9681173

# count per million read (normalized count)
norm_counts <- cpm(y)
head(norm_counts)
                      ctr1      ctr2      ctr3      trt1      trt2      trt3
Sobic.001G000200 92.610154 99.193046 68.940330 91.046159 73.623746 93.628467
Sobic.001G000400  5.579744  2.671972  6.172917  2.080487  2.423611  2.547814
Sobic.001G000700 19.324115 27.128476 15.203816 26.026728 34.273857 25.196008
Sobic.001G000800 74.410626 83.143686 71.656564 79.999334 72.089337 75.391042
Sobic.001G001000  9.283029  2.593128  3.164935  2.650064 10.290420 11.014460
Sobic.001G001132  7.464703  4.170392  3.817499  6.392939  9.929724  3.795852
```          

**<span style="color:#060606">References</span>**

- Mortazavi A, Williams BA, McCue K, Schaeffer L, Wold B. Mapping and quantifying mammalian transcriptomes by RNA-Seq. Nature methods. 2008 Jul 1;5(7):621-8.
- Wagner GP, Kin K, Lynch VJ. Measurement of mRNA abundance using RNA-seq data: RPKM measure is inconsistent among samples. Theory in biosciences. 2012 Dec 1;131(4):281-5.
- Bullard JH, Purdom E, Hansen KD, Dudoit S. Evaluation of statistical methods for normalization and differential expression in mRNA-Seq experiments. BMC bioinformatics. 2010 Dec;11(1):94.
- Robinson MD, Oshlack A. A scaling normalization method for differential expression analysis of RNA-seq data. Genome biology. 2010 Mar;11(3):R25.
- Anders S, Huber W. Differential expression analysis for sequence count data. Nature Precedings. 2010 Apr 30:1-.
- Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome biology. 2014 Dec 1;15(12):550.
- Bacher R, Chu LF, Leng N, Gasch AP, Thomson JA, Stewart RM, Newton M, Kendziorski C. SCnorm: robust normalization of single-cell RNA-seq data. Nature methods. 2017 Jun;14(6):584.
- Zhang, Y., Parmigiani, G., & Johnson, W. E. (2020). ComBat-Seq: batch effect adjustment for RNA-Seq count data. bioRxiv, 904730.
- Smid M, van den Braak RR, van de Werken HJ, van Riet J, van Galen A, de Weerd V, van der Vlugt-Daane M, Bril SI, Lalmahomed ZS, Kloosterman WP, Wilting SM. Gene length corrected trimmed mean of M-values (GeTMM) processing of RNA-seq data performs similarly in intersample analyses while improving intrasample comparisons. BMC bioinformatics. 2018 Dec;19(1):1-3.
- Bedre R, Irigoyen S, Schaker PD, Monteiro-Vitorello CB, Da Silva JA, Mandadi KK. Genome-wide alternative splicing landscapes modulated by biotrophic sugarcane smut pathogen. Scientific reports. 2019 Jun 20;9(1):1-2.
- Robinson MD, McCarthy DJ, Smyth GK. edgeR: a Bioconductor package for differential expression analysis of digital gene expression data. Bioinformatics. 2010 Jan 1;26(1):139-40.

**<span style="color:#33a8ff">How to cite?</span>**

Bedre, R.  (2017, May 05) .Gene expression units explained: RPM, RPKM, FPKM, TPM, <i>DESeq</i>, TMM, SCnorm, and ComBat-Seq. 
https://reneshbedre.github.io/blog/expression_units.html


<span style="color:#9e9696">If you have any questions, comments or recommendations, please email me at 
<b>reneshbe@gmail.com</b></span>

<span style="color:#9e9696"><i> Last updated: July 20, 2020</i> </span>

<p>
{% include  subscribe.html %}
</p>

<p>
{% include  license.html %}
</p>