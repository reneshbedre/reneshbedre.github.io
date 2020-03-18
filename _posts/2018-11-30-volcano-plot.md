---
layout: posts
title: "Volcano plot in Python"
date:   2018-11-30 02:15:18
author_profile: true
permalink: blog/volcano.html
classes: wide
categories: blog
tags: bioinformatics
---
<p>
{% include  share.html %}
</p>

**<span style="color:#33a8ff">What is Volcano plot?</span>**
 - 2-dimensional (2D) scatter plot having a shape like volcano
 - Used to visualize and identify statistically significant gene expression changes from two different conditions (eg. normal vs. 
   treated) in terms of log fold change (X-axis) and P-value (Y-axis)
   
**<span style="color:#33a8ff">Applications</span>**   
 - Easy to visualize expression of thousands of genes obtained from omics research (eg. Transcriptomics, genomics, 
   proteomics etc.) and pinpoint genes with significant changes 
   
**<span style="color:#33a8ff">How to create Volcano plot in Python?</span>**

For generating volcano plot, I have used gene expression data published in Bedre et al. 2016 to identify statistically significantly
induced or downregulated genes in response to salt stress in <i>Spartina alterniflora</i> 
(<a href="https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-016-3017-3">Read paper</a>). 
Here's you can download gene expression dataset used for generating volcano plot: 
[dataset]({{"/assets/posts/volcano/testvolcano.csv" | absolute_url }})

```python
# We will use `bioinfokit` for volcano plot.
# download and install bioinfokit (Tested on Linux, Mac, Windows) 
git clone https://github.com/reneshbedre/bioinfokit.git
cd bioinfokit
python setup.py install
```

After installing `bioinfokit`, it can be used for volcano plot,

```python
# you can use interactive python console, jupyter or python code
# I am using interactive python console (Python 3.6)
>>> from bioinfokit import visuz
# here you can change default parameters. 
# Read documentation at https://github.com/reneshbedre/bioinfokit
>>> visuz.volcano(table="testvolcano.csv", lfc="log2FC", pv="p-value")
```

Generated volcano plot by above code (green: upregulated and red: downregulated genes),

<p align="center">
<img src="/assets/posts/volcano/vol1.png" width="600">
</p>


Change color and transparency of volcano plot
```python
# change colormap
>>>visuz.volcano(table="testvolcano.csv", lfc="log2FC", pv="p-value", color=("#00239CFF","#E10600FF"))
```

<p align="center">
<img src="/assets/posts/volcano/vol2.png" width="600">
</p>

```python
# change transparency
>>>visuz.volcano(table="testvolcano.csv", lfc="log2FC", pv="p-value", color=("#00239CFF","#E10600FF"), valpha=0.5)
```

<p align="center">
<img src="/assets/posts/volcano/vol3.png" width="600">
</p>

Add gene labels to the points,

```python
# add gene customized labels
# note: here you need to provide column name of gene Ids (geneid parameter)
>>>visuz.volcano(table="testvolcano.csv", lfc="log2FC", pv="p-value", geneid="GeneNames",genenames=("LOC_Os09g01000.1", "LOC_Os01g50030.1", "LOC_Os06g40940.3", "LOC_Os03g03720.1") )
```
<p align="center">
<img src="/assets/posts/volcano/vol4.png" width="600">
</p>

change gene label size,

```python
# add gene customized labels
# note: here you need to provide column name of gene Ids (geneid parameter)
>>>visuz.volcano(table="testvolcano.csv", lfc="log2FC", pv="p-value", geneid="GeneNames",genenames=("LOC_Os09g01000.1", "LOC_Os01g50030.1", "LOC_Os06g40940.3", "LOC_Os03g03720.1"), gfont=12 )
```

<p align="center">
<img src="/assets/posts/volcano/vol5.png" width="600">
</p>

Add gene names instead of gene Ids, 
```python
# add gene customized labels
# note: here you need to provide column name of gene Ids (geneid parameter)
# as the dataset only have geneids, you need to provide tuple of gene Id and corresponding gene names
>>>visuz.volcano(table="testvolcano.csv", lfc="log2FC", pv="p-value", geneid="GeneNames", genenames=({"LOC_Os09g01000.1":"EP", "LOC_Os01g50030.1":"CPuORF25", "LOC_Os06g40940.3":"GDH", "LOC_Os03g03720.1":"G3PD"}))
```

<p align="center">
<img src="/assets/posts/volcano/vol6.png" width="600">
</p>


To create a inverted volcano plot,

```python
# you can use interactive python console, jupyter or python code
# I am using interactive python console
>>> from bioinfokit import visuz
# here you can change default parameters. 
# Read documentation at https://github.com/reneshbedre/bioinfokit
>>> visuz.involcano(table="testvolcano.csv", lfc="log2FC", pv="p-value")
```

Generated inverted volcano plot by adding above code,

<!-- ![screenshot]({{ "/assets/posts/bioinfokit_data/involcano.png" | absolute_url }}) -->

<p align="center">
<img src="/assets/posts/volcano/involcano1.png" width="600">
</p>


Change color inverted volcano plot
```python
# change colormap
>>>visuz.involcano(table="testvolcano.csv", lfc="log2FC", pv="p-value", color=("#00239CFF","#E10600FF"))
```
<p align="center">
<img src="/assets/posts/volcano/involcano2.png" width="600">
</p>


Add gene names instead of gene Ids, 
```python
# add gene customized labels
# note: here you need to provide column name of gene Ids (geneid parameter)
# as the dataset only have geneids, you need to provide tuple of gene Id and corresponding gene names
>>>visuz.involcano(table="testvolcano.csv", lfc="log2FC", pv="p-value", geneid="GeneNames", genenames=({"LOC_Os09g01000.1":"EP", "LOC_Os01g50030.1":"CPuORF25", "LOC_Os06g40940.3":"GDH", "LOC_Os03g03720.1":"G3PD"}))
```

<p align="center">
<img src="/assets/posts/volcano/involcano3.png" width="600">
</p>


<p>
{% include  cite.html %}
</p>

<p>
{% include  share.html %}
</p>


<span style="color:#9e9696"><i> Last updated: December 21, 2019</i> </span>


