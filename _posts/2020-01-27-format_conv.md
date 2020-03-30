---
title: "File format conversions"
date:   2020-01-27 02:15:18
permalink: blog/format.html
author_profile: false
toc: true
toc_label: "Page Content"
---
<p>
{% include  share.html %}
</p>

## <span style="color:#33a8ff">File format conversions</span>
- bioinfokit will help you to convert the file formats for variety of tasks

## <span style="color:#33a8ff">Install bioinfokit</span>
- Check [bioinfokit documentation]({{"/blog/howtoinstall.html" | absolute_url }}) for installation and documentation
 

## <span style="color:#33a8ff">FASTQ to FASTA</span>

Download test [dataset]({{"/assets/posts/format/test_1.fastq" | absolute_url }})

```python
>>> from bioinfokit import analys
>>> analys.format.fqtofa(file="test_1.fastq")
# output will ve saved in same directory (output.fasta)
```  

## <span style="color:#33a8ff">HMM to CSV</span>

Convert table output obtained from hmmscan (HMMER tool) to CSV format
Download test [dataset]({{"/assets/posts/format/test_hmm.txt" | absolute_url }})

```python
>>> from bioinfokit import analys
>>> analys.format.hmmtocsv(file="test_hmm.txt")
# output will ve saved in same directory (output_hmm.csv)
```  

## <span style="color:#33a8ff">TAB to CSV</span>

Download test [dataset]({{"/assets/posts/format/test_tab.txt" | absolute_url }})

```python
>>> from bioinfokit import analys
>>> analys.format.tabtocsv(file="test_tab.txt")
# output will ve saved in same directory (output.csv)
```  

## <span style="color:#33a8ff">CSV to TAB</span>

Download test [dataset]({{"/assets/posts/format/test_csv.csv" | absolute_url }})

```python
>>> from bioinfokit import analys
>>> analys.format.csvtotab(file="test_csv.csv")
# output will ve saved in same directory (output.txt)
```  

<p>
{% include  cite.html %}
</p>

<p>
{% include  share.html %}
</p>
    
<span style="color:#9e9696"><i> Last updated: January 27, 2020</i> </span>    
