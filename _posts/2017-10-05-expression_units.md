---
title: "Gene expression units explained: RPM, RPKM, FPKM, TPM, <i>DESeq</i>, TMM, SCnorm, and ComBat-Seq"
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


**<span style="color:#060606"><i>DESeq</i> or <i>DESeq2</i> normalization (median-of-ratios method)</span>**
- The <i>DESeq</i> (and also <i>DESeq2</i>) normalization method is proposed by Anders and Huber, 2010 and is similar to TMM
- <i>DESeq</i> normalization method  also assumes that most of the genes are not differentially expressed
- The <i>DESeq</i> calculates size factors for each sample to compare the counts obtained from different samples with
  different sequencing depth
- <i>DESeq</i> normalization uses the median of the ratios of observed counts to calculate size factors. Briefly,
  the size factor is calculated by first dividing the observed counts for each sample by its geometric mean. The size factor
  is then calculated as the median of this ratio for each sample. This size factor then used for normalizing raw
  count data for each sample.


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

<!--
**<span style="color:#060606">Relationship between RPKM and TPM,</span>**

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;RPKM&space;=&space;TPM&space;\times&space;\frac{10^3&space;\times&space;total&space;\&space;number&space;\&space;of&space;\&space;transcripts&space;\&space;sampled}{read&space;\&space;length&space;\times&space;total&space;\&space;number&space;\&space;of&space;\&space;mapped&space;\&space;reads}" />
-->

**<span style="color:#060606">References</span>**

 - Mortazavi A, Williams BA, McCue K, Schaeffer L, Wold B. Mapping and quantifying mammalian transcriptomes by RNA-Seq. Nature methods. 2008 Jul 1;5(7):621-8.
 - Wagner GP, Kin K, Lynch VJ. Measurement of mRNA abundance using RNA-seq data: RPKM measure is inconsistent among samples. Theory in biosciences. 2012 Dec 1;131(4):281-5.
 - Bullard JH, Purdom E, Hansen KD, Dudoit S. Evaluation of statistical methods for normalization and differential expression in mRNA-Seq experiments. BMC bioinformatics. 2010 Dec;11(1):94.
 - Robinson MD, Oshlack A. A scaling normalization method for differential expression analysis of RNA-seq data. Genome biology. 2010 Mar;11(3):R25.
 - Anders S, Huber W. Differential expression analysis for sequence count data. Nature Precedings. 2010 Apr 30:1-.
 - Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome biology. 2014 Dec 1;15(12):550.
 - Bacher R, Chu LF, Leng N, Gasch AP, Thomson JA, Stewart RM, Newton M, Kendziorski C. SCnorm: robust normalization of single-cell RNA-seq data. Nature methods. 2017 Jun;14(6):584.
 - Zhang, Y., Parmigiani, G., & Johnson, W. E. (2020). ComBat-Seq: batch effect adjustment for RNA-Seq count data. bioRxiv, 904730.

**<span style="color:#33a8ff">How to cite?</span>**

Bedre, R.  (2017, May 05) .Gene expression units explained: RPM, RPKM, FPKM, TPM, <i>DESeq</i>, TMM, SCnorm, and ComBat-Seq. 
https://reneshbedre.github.io//blog/expression_units.html


<span style="color:#9e9696">If you have any questions, comments or recommendations, please email me at 
<b>reneshbe@gmail.com</b></span>

<span style="color:#9e9696"><i> Last updated: June 13, 2020</i> </span>

<p>
{% include  subscribe.html %}
</p>

<p>
{% include  license.html %}
</p>