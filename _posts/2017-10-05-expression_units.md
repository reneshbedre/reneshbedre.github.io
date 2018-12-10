---
layout: post
title: "Gene expression units explained: RPM, RPKM, FPKM and TPM"
date:   2017-10-05
author: Renesh Bedre
description: "Basic Bioinformatics"
permalink: blog/expression_units.html
comments: true
---

In RNA-seq gene expression data analysis, we come across various expression units such as RPM, RPKM, FPKM and raw reads counts. Most of the times it's difficult to understand basic underlying methodology to calculate these units from mapped sequence data.

I have seen a lot of post of such normalization questions and their confusion among readers. Hence, I attempted here to explain these units in the much simpler way (avoided complex mathematical expressions).
Note: To use biopython, you need to install it.

## <span style="color:#33a8ff">Why different normalized expression units</span> ##

The expression units provide a digital measure of the abundance of transcripts. Normalized expression units are necessary to remove technical biases in sequenced data such as depth of sequencing (more sequencing depth produces more read count for gene expressed at same level) and gene length (differences in gene length generate unequal reads count for genes expressed at the same level; longer the gene more the read count).


## <span style="color:#33a8ff"> Gene expression units and calculation </span> ##

 **<span style="color:#060606">RPM (Reads per million mapped reads) </span>**

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;RPM&space;=&space;\frac{Number&space;\&space;of&space;\&space;reads&space;\&space;mapped&space;\&space;to&space;\&space;gene&space;\times&space;10^6}{Total&space;\&space;number&space;\&space;of&space;\&space;mapped&space;\&space;reads}" />

For example, You have sequenced one library with 5 million(M) reads. Among them, total 4 M matched to the genome sequence and 5000 reads matched to a given gene.

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;\large&space;RPM&space;=&space;\frac{5000&space;\times&space;10^6}{4&space;\times&space;10^6}&space;=&space;1250" />

Notes:

 - RPM does not consider the transcript length normalization.
 - RPM Suitable for sequencing protocols where reads are generated irrespective of gene length


**<span style="color:#060606">RPKM (Reads per kilo base per million mapped reads)</span>**

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;RPKM&space;=&space;\frac{Number&space;\&space;of&space;\&space;reads&space;\&space;mapped&space;\&space;to&space;\&space;gene&space;\times&space;10^3&space;\times&space;10^6}{Total&space;\&space;number&space;\&space;of&space;\&space;mapped&space;\&space;reads&space;\times&space;gene&space;\&space;length&space;\&space;in&space;\&space;bp}" />

 Here, 10^3 normalizes for gene length and 10^6 for sequencing depth factor.

FPKM (Fragments per kilo base per million mapped reads) is analogous to RPKM and used especially in paired-end RNA-seq experiments. In paired-end RNA-seq experiments, two (left and right) reads are sequenced from same DNA fragment. When we map paired-end data, both reads or only one read with high quality from a fragment can map to reference sequence. To avoid confusion or multiple counting, the fragments to which both or single read mapped is counted and represented for FPKM calculation.

For example, You have sequenced one library with 5 M reads. Among them, total 4 M matched to the genome sequence and 5000 reads matched to a given gene with a length of 2000 bp.

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;RPKM&space;=&space;\frac{5000&space;\times&space;10^3&space;\times&space;10^6}{4&space;\times&space;10^6&space;\times&space;2000}&space;=&space;625" />

Notes:

 - RPKM considers the gene length for normalization
 - RPKM is suitable for sequencing protocols where reads sequencing depends on gene length
 - Used in single-end RNA-seq experiments (FPKM for paired-end RNA-seq data)


**<span style="color:#060606">TPM (Transcript per million)</span>**

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;TPM&space;=&space;\frac{Number&space;\&space;of&space;\&space;reads&space;\&space;mapped&space;\&space;to&space;\&space;gene&space;\times&space;read&space;\&space;length&space;\times&space;10^6}{Total&space;\&space;number&space;\&space;of&space;\&space;transcripts&space;\&space;sampled&space;\times&space;gene&space;\&space;length&space;\&space;in&space;\&space;bp}" />

Here, read length refers to the average number of nucleotides mapped to a gene.

For example, You have sequenced one library with 5M 100 bp reads. Among them, total 4M matched to the genome sequence and 5000 reads matched to a given gene with a length of 2000 bp. There were 10K transcripts were sampled from a genome sequence i.e. reads mapped to 10K genes. Suppose all 100 bp mapped from 5000 reads.

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;TPM&space;=&space;\frac{5000&space;\times&space;100&space;\times&space;10^6}{10000&space;\times&space;2000}&space;=&space;25000" />

Notes:

 - TPM considers the gene length for normalization
 - TPM proposed as an alternative to RPKM due to inaccuracy in RPKM measurement (Wagner et al., 2012)
 - TPM is suitable for sequencing protocols where reads sequencing depends on gene length

**<span style="color:#060606">Relationship between RPKM and TPM,</span>**

 <img src="https://latex.codecogs.com/gif.latex?\bg_green&space;RPKM&space;=&space;TPM&space;\times&space;\frac{10^3&space;\times&space;total&space;\&space;number&space;\&space;of&space;\&space;transcripts&space;\&space;sampled}{read&space;\&space;length&space;\times&space;total&space;\&space;number&space;\&space;of&space;\&space;mapped&space;\&space;reads}" />


References:

 - Mortazavi A, Williams BA, McCue K, Schaeffer L, Wold B. Mapping and quantifying mammalian transcriptomes by RNA-Seq. Nature methods. 2008 Jul 1;5(7):621-8.
 - Wagner GP, Kin K, Lynch VJ. Measurement of mRNA abundance using RNA-seq data: RPKM measure is inconsistent among samples. Theory in biosciences. 2012 Dec 1;131(4):281-5.


**<span style="color:#33a8ff">How to cite?</span>**

Bedre, R. “Gene expression units explained: RPM, RPKM, FPKM and TPM” Renesh Bedre (blog), October 5, 2017, 
https://reneshbedre.github.io/blog/expression_units.html.

<span style="color:#9e9696">If you have any questions, comments or recommendations, please email me at 
<b>reneshbe@gmail.com</b></span>