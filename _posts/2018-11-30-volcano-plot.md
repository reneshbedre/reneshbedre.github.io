---
title: "Volcano plot in Python"
date:   2018-11-30 02:15:18
author_profile: true
permalink: blog/volcano.html
classes: wide
redirect_to:
  - https://www.reneshbedre.com/blog/volcano.html
tags:
  - Genomic visualization
  - Gene expression
---
<p>
{% include  share.html %}
</p>


## <span style="color:#33a8ff">What is Volcano plot?</span>
 - 2-dimensional (2D) scatter plot having a shape like a volcano
 - Used to visualize and identify statistically significant gene expression changes from two different conditions 
   (eg. normal vs. treated) in terms of log fold change (X-axis) and P-value (Y-axis)
   
## <span style="color:#33a8ff">Applications</span>   
 - Easy to visualize the expression of thousands of genes obtained from omics research (eg. Transcriptomics, genomics, 
   proteomics, etc.) and pinpoint genes with significant changes 
   
## <span style="color:#33a8ff">How to create Volcano plot in Python?</span>
- We will use `bioinfokit v0.8.8` or later
- Check [bioinfokit documentation]({{"/blog/howtoinstall.html" | absolute_url }}) for installation and documentation
- For generating a volcano plot, I have used gene expression data published in Bedre et al. 2016 to identify 
  statistically significantly induced or downregulated genes in response to salt stress in <i>Spartina alterniflora</i> 
  (<a href="https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-016-3017-3">Read paper</a>). Here's you can 
  download gene expression dataset used for generating volcano plot: 
  [dataset]({{"/assets/posts/volcano/testvolcano.csv" | absolute_url }})


```python
# you can use interactive python interpreter, jupyter notebook, spyder or python code
# I am using interactive python interpreter (Python 3.7)
>>> from bioinfokit import analys, visuz
# load dataset as pandas dataframe
>>> df = analys.get_data('volcano').data
>>> df.head()
          GeneNames  value1  value2    log2FC       p-value
0  LOC_Os09g01000.1    8862   32767 -1.886539  1.250000e-55
1  LOC_Os12g42876.1    1099     117  3.231611  1.050000e-55
2  LOC_Os12g42884.2     797      88  3.179004  2.590000e-54
3  LOC_Os03g16920.1     274       7  5.290677  4.690000e-54
4  LOC_Os05g47540.4     308      18  4.096862  2.190000e-54

>>> visuz.gene_exp.volcano(df=df, lfc='log2FC', pv='p-value')
# plot will be saved in same directory (volcano.png)
# set parameter show=True, if you want view the image instead of saving
```

Generated volcano plot by above code (green: upregulated and red: downregulated genes),

<p align="center">
<img src="/assets/posts/volcano/vol1.svg" width="600">
</p>

Add legend to the plot and adjust the legend position,
```python
>>> visuz.gene_exp.volcano(df=df, lfc='log2FC', pv='p-value', plotlegend=True, legendpos='upper right', 
    legendanchor=(1.46,1))
```
<p align="center">
<img src="/assets/posts/volcano/vol1_2.svg" width="600">
</p>


Change color of volcano plot
```python
# change colormap
>>> visuz.gene_exp.volcano(df=df, lfc='log2FC', pv='p-value', color=("#00239CFF", "grey", "#E10600FF"))
```

<p align="center">
<img src="/assets/posts/volcano/vol2.svg" width="600">
</p>


Change log fold change and P-value threshold,
```python
>>> visuz.gene_exp.volcano(df=df, lfc='log2FC', pv='p-value', lfc_thr=2, pv_thr=0.01, 
    color=("#00239CFF", "grey", "#E10600FF"), plotlegend=True, legendpos='upper right', 
    legendanchor=(1.46,1))
```
<p align="center">
<img src="/assets/posts/volcano/vol2_1.svg" width="600">
</p>


Change transparency of volcano plot
```python
>>> visuz.gene_exp.volcano(df=df, lfc='log2FC', pv='p-value', color=("#00239CFF", "grey", "#E10600FF"), 
    valpha=0.5)
```

<p align="center">
<img src="/assets/posts/volcano/vol3.svg" width="600">
</p>

Change the shape of the points
```python
# add star shape
# check more shapes at https://matplotlib.org/3.1.1/api/markers_api.html
>>> visuz.gene_exp.volcano(df=df, lfc='log2FC', pv='p-value', color=("#00239CFF", "grey", "#E10600FF"), 
    markerdot='*')
```
<p align="center">
<img src="/assets/posts/volcano/vol3_1.svg" width="600">
</p>

Change the shape and size of the points
```python
>>> visuz.gene_exp.volcano(df=df, lfc='log2FC', pv='p-value', color=("#00239CFF", "grey", "#E10600FF"), 
    markerdot='*', dotsize=30)
```
<p align="center">
<img src="/assets/posts/volcano/vol3_2.svg" width="600">
</p>

Add gene labels (text style) to the points,

```python
# add gene customized labels
# note: here you need to provide column name of gene Ids (geneid parameter)
# default simple text will be added
>>> visuz.gene_exp.volcano(df=df, lfc="log2FC", pv="p-value", geneid="GeneNames",
    genenames=("LOC_Os09g01000.1", "LOC_Os01g50030.1", "LOC_Os06g40940.3", "LOC_Os03g03720.1") )
# if you want to label all DEGs defined lfc_thr and pv_thr, set genenames='deg' 
```
<p align="center">
<img src="/assets/posts/volcano/vol4.svg" width="600">
</p>

Add gene labels (box style) to the points,

```python
>>> visuz.gene_exp.volcano(df=df, lfc="log2FC", pv="p-value", geneid="GeneNames",
    genenames=("LOC_Os09g01000.1", "LOC_Os01g50030.1", "LOC_Os06g40940.3", "LOC_Os03g03720.1"), gstyle=2 )
```
<p align="center">
<img src="/assets/posts/volcano/vol5.svg" width="600">
</p>

Add gene names instead of gene Ids, 
```python
# add gene customized labels
# note: here you need to provide column name of gene Ids (geneid parameter)
# as the dataset only have geneids, you need to provide tuple of gene Id and corresponding gene names
>>> visuz.gene_exp.volcano(df=df, lfc="log2FC", pv="p-value", geneid="GeneNames", 
    genenames=({"LOC_Os09g01000.1":"EP", "LOC_Os01g50030.1":"CPuORF25", "LOC_Os06g40940.3":"GDH", "LOC_Os03g03720.1":"G3PD"}),
    gstyle=2)
```

<p align="center">
<img src="/assets/posts/volcano/vol6.svg" width="600">
</p>

Add threshold lines,
```python
>>> visuz.gene_exp.volcano(df=df, lfc="log2FC", pv="p-value", geneid="GeneNames", 
    genenames=({"LOC_Os09g01000.1":"EP", "LOC_Os01g50030.1":"CPuORF25", "LOC_Os06g40940.3":"GDH", "LOC_Os03g03720.1":"G3PD"}),
    gstyle=2, sign_line=True)
``` 
<p align="center">
<img src="/assets/posts/volcano/vol6_1.svg" width="600">
</p>


Change X and Y range ticks, font size and name for tick labels
```python
>>> visuz.gene_exp.volcano(df=df, lfc="log2FC", pv="p-value", geneid="GeneNames", 
    genenames=({"LOC_Os09g01000.1":"EP", "LOC_Os01g50030.1":"CPuORF25", "LOC_Os06g40940.3":"GDH", "LOC_Os03g03720.1":"G3PD"}),
    gstyle=2, sign_line=True, xlm=(-6,6,1), ylm=(0,61,5), figtype='svg', axtickfontsize=10,
    axtickfontname='Verdana')
```
<p align="center">
<img src="/assets/posts/volcano/vol7.svg" width="600">
</p>

In addition to these parameters, the parameters for figure type (`figtype`), X and Y axis ticks range (`xlm`, `ylm`), axis labels (`axxlabel`, `axylabel`),  
axis labels font size  (`axlabelfontsize`), and axis tick labels font size and name (`axtickfontsize`, `axtickfontname`)
can be provided.



To create a inverted volcano plot,

```python
# you can use interactive python console, jupyter or python code
# I am using interactive python console
>>> from bioinfokit import visuz
# here you can change default parameters. 
# Read documentation at https://github.com/reneshbedre/bioinfokit
>>> visuz.gene_exp.involcano(df=df, lfc="log2FC", pv="p-value")
```

Generated inverted volcano plot by adding above code,

<p align="center">
<img src="/assets/posts/volcano/involcano1.svg" width="600">
</p>


Change color inverted volcano plot
```python
# change colormap
>>> visuz.gene_exp.involcano(df=df, lfc="log2FC", pv="p-value", color=("#00239CFF", "grey", "#E10600FF"))
```
<p align="center">
<img src="/assets/posts/volcano/involcano2.svg" width="600">
</p>


Add gene names instead of gene Ids, 
```python
# add gene customized labels
# note: here you need to provide column name of gene Ids (geneid parameter)
# as the dataset only have geneids, you need to provide tuple of gene Id and corresponding gene names
>>> visuz.gene_exp.involcano(df=df, lfc="log2FC", pv="p-value", geneid="GeneNames", 
    genenames=({"LOC_Os09g01000.1":"EP", "LOC_Os01g50030.1":"CPuORF25", "LOC_Os06g40940.3":"GDH", "LOC_Os03g03720.1":"G3PD"}), 
    gstyle=2)
```

<p align="center">
<img src="/assets/posts/volcano/involcano3.svg" width="600">
</p>

In addition to these parameters, the parameters for figure type (`figtype`), axis labels (`axxlabel`, `axylabel`), axis labels
font size (`axlabelfontsize`), and axis tick labels font size and name (`axtickfontsize`,  `axtickfontname`)
can be provided.



<p>
{% include  cite.html %}
</p>

<p>
{% include  share.html %}
</p>


<span style="color:#9e9696"><i> Last updated: July 02, 2020</i> </span>


<p>
{% include  license.html %}
</p>