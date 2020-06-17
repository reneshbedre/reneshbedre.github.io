---
title: "Bioinformatics file readers and processing (FASTA, FASTQ, and VCF)"
date:   2020-06-17 06:18:08
author_profile: true
permalink: blog/filereaders.html
classes: wide
tags:
  - High-throughput sequence analysis
  - Biological data handling
---

- We will discuss how to read and process common bioinformatics files
- We will use `bioinfokit v0.8.4` or later
- Check [bioinfokit documentation]({{"/blog/howtoinstall.html" | absolute_url }}) for installation and documentation

## <span style="color:#33a8ff">FASTA reader</span>
```python
# you can use interactive python interpreter, jupyter notebook, google colab, spyder or python code
# I am using interactive python interpreter (Python 3.8.2)
from bioinfokit.analys import fasta
fasta_iter = fasta.fasta_reader(file='fasta_file')
# read fasta file
for record in fasta_iter:
    header, sequence = record
    print(header, sequence)
    # get sequence length
    sequence_len = len(sequence)
    # count A bases
    a_base = sequence.count('A')
    print(a_base)
    # reverse complementary
    rev_com_seq = fasta.rev_com(seq=sequence)
    print(rev_com_seq)
```

## <span style="color:#33a8ff">FASTQ reader</span>
```python
# you can use interactive python interpreter, jupyter notebook, google colab, spyder or python code
# I am using interactive python interpreter (Python 3.8.2)
from bioinfokit.analys import fastq
fastq_iter = fastq.fastq_reader(file='fastq_file')
# read fastq file
for record in fastq_iter:
    # get sequence headers, sequence, and quality values
    header_1, sequence, header_2, qual = record
    # get sequence length
    sequence_len = len(sequence)
    # count A bases
    a_base = sequence.count('A')
    print(sequence, qual, a_base, sequence_len)
```

## <span style="color:#33a8ff">VCF (variant call format) reader</span>
```python
# you can use interactive python interpreter, jupyter notebook, google colab, spyder or python code
# I am using interactive python interpreter (Python 3.8.2)
from bioinfokit.analys import marker
# id is for column name with chromosome identifier in VCF file
vcf_iter = marker.vcfreader(file='vcf_file', id='#CHROM')
# read vcf file
for record in vcf_iter:
    # get info lines, header, and variant records
    headers, info_lines, marker_lines = record
    # get variant records with specified chromosome name
    # change chr name as per need
    if marker_lines[0] == 'chr':
        print(marker_lines)
    # retrieve markers within specified positions
    # change chr, pos1, and pos2 variable as per need
    if marker_lines[0]=='SL4.0ch00' and int(marker_lines[1]>=764654) and int(marker_lines[1])<=1038399:
        print(marker_lines)
```

<p>
{% include  cite.html %}
</p>

<span style="color:#9e9696"><i> Last updated: June 17, 2020</i> </span>

<p>
{% include  license.html %}
</p>