---
layout: post
title: "File format conversions"
date:   2020-01-27 02:15:18
author: Renesh Bedre
description: "Advanced Bioinformatics"
permalink: blog/format.html
comments: true
---

**<span style="color:#33a8ff">File format conversions</span>**
- bioinfokit will help you to convert the file formats for variety of tasks

**<span style="color:#33a8ff">Install bioinfokit</span>**
```python
# We will use `bioinfokit` 
# download and install bioinfokit (Tested on Linux, Mac, Windows) 
# read instructions https://github.com/reneshbedre/bioinfokit
git clone https://github.com/reneshbedre/bioinfokit.git
cd bioinfokit
python setup.py install
```   

**<span style="color:#33a8ff">FASTQ to FASTA</span>**

Download test [dataset]({{"/myfiles/format/test_1.fastq" target="_blank""| absolute_url }})

```python
>>> from bioinfokit import analys
>>> analys.format.fqtofa(file="test_1.fastq")
# output will ve saved in same directory (output.fasta)
```  

**<span style="color:#33a8ff">HMM to CSV</span>**

Convert table output obtained from hmmscan (HMMER tool) to CSV format
Download test [dataset]({{"/myfiles/format/test_hmm.txt" target="_blank""| absolute_url }})

```python
>>> from bioinfokit import analys
>>> analys.format.hmmtocsv(file="test_hmm.txt")
# output will ve saved in same directory (output_hmm.csv)
```  

**<span style="color:#33a8ff">TAB to CSV</span>**

Download test [dataset]({{"/myfiles/format/test_tab.txt" target="_blank""| absolute_url }})

```python
>>> from bioinfokit import analys
>>> analys.format.tabtocsv(file="test_tab.txt")
# output will ve saved in same directory (output.csv)
```  

**<span style="color:#33a8ff">CSV to TAB</span>**

Download test [dataset]({{"/myfiles/format/test_csv.csv" target="_blank""| absolute_url }})

```python
>>> from bioinfokit import analys
>>> analys.format.csvtotab(file="test_csv.csv")
# output will ve saved in same directory (output.txt)
```  

**<span style="color:#33a8ff">How to cite?</span>**

Bedre, R. Bioinformatics data analysis and visualization toolkit. GitHub repository, <a href="https://github.com/reneshbedre/bioinfokit">https://github.com/reneshbedre/bioinfokit</a>

<span style="color:#9e9696">If you have any questions, comments or recommendations, please email me at 
<b>reneshbe@gmail.com</b></span>
    
<span style="color:#9e9696"><i> Last updated: January 27, 2020</i> </span>    


<div>

<a href="https://twitter.com/renbedre?ref_src=twsrc%5Etfw" class="twitter-follow-button" data-show-count="false">Follow @renbedre</a><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<a href="https://twitter.com/share?url=https://reneshbedre.github.io/blog/venn.html&amp;text=Venn%20Diagrams%20Python&amp;hashtags=" target="_blank">
        <img src="https://simplesharebuttons.com/images/somacro/twitter.png" alt="Twitter" style="width: 35px;"/>
    </a>
<a href="http://www.facebook.com/sharer.php?u=https://reneshbedre.github.io/blog/venn.html" target="_blank">
        <img src="https://simplesharebuttons.com/images/somacro/facebook.png" alt="Facebook" style="width: 35px;"/>
    </a>    
    
</div>