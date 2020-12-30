---
title: "Principal component analysis (PCA) and visualization using Python"
date:   2018-03-30 06:18:08
author_profile: true
permalink: blog/pca_3d.html
classes: wide
redirect_to:
  - https://www.reneshbedre.com/blog/pca_3d.html
tags:
  - Unsupervised machine learning
  - Clustering
  - Exploratory data analysis
  - Dimensionality reduction
---

<!--
This page has been moved <a href='https://www.reneshbedre.com/blog/pca_3d.html' target='_blank'>here</a>
-->

<p>
{% include  share.html %}
</p>


## <span style="color:#33a8ff">What is PCA?</span>

- PCA is a classical multivariate (unsupervised machine learning) statistical method that used to interpret the 
  variation in high-dimensional interrelated dataset (dataset with a large number of variables)
- PCA reduces the high-dimensional interrelated data to low-dimension by linearly transforming the old variable into a 
  new set of uncorrelated variables called principal component (PC) while retaining the most possible variation. 
- The PCs are easy to visualize and summarise the feature of original high-dimensional datasets in low-dimensional space.  
- PCA preserves the global data structure by forming well-separated clusters but can fail to preserve the 
  similarities within the clusters. 
- For example, when datasets contain 10 variables (10D), it is arduous to visualize them at the same time
  (you may have to do 45 pairwise comparisons to interpret dataset effectively). PCA transforms them into a new set of
  variables (PCs) with top PCs having the highest variation. PCs are ordered which means that the first few PCs
  (generally first 3 PCs but can be more) contribute most of the variance present in the the original high-dimensional 
  dataset. These top first 2 or 3 PCs can be plotted easily and summarize and the features of all original 10 variables.


## <span style="color:#33a8ff">Perform PCA in Python</span>
- we will use sklearn, seaborn, and bioinfokit (v0.9.8 or later) packages for PCA and visualization
- Check [bioinfokit documentation]({{"/blog/howtoinstall.html" | absolute_url }}) for installation and documentation
- Download [dataset]({{"/assets/posts/pca/cot_pca.csv" | absolute_url }}) for PCA (a subset of gene expression data associated with
  different conditions of fungal stress in cotton which is published in <a href="https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0138025">Bedre et al., 2015</a>)

### load dataset
```python
# I am using interactive python console (Python 3.7.4)
>>> from sklearn.decomposition import PCA
>>> from sklearn.preprocessing import StandardScaler
>>> from bioinfokit.analys import get_data
>>> import numpy as np
>>> import pandas as pd
# load dataset as pandas dataframe
>>> df = get_data('gexp').data
>>> df.head()
         A         B         C          D          E         F
0  4.50570  3.260360 -1.249400   8.898070   8.059550 -0.842803
1  3.50856  1.660790 -1.856680  -2.573360  -1.373700  1.196000
2  4.44701  3.411940 -1.040870  10.271195  10.517256  0.272272
3  2.16003  3.146520  0.982809   9.024300   6.058320 -2.967420
4  2.35701  0.452589 -1.910680  12.984239  10.019605 -2.939020
# variables A to F denotes multiple conditions associated with fungal stress
# Read full paper https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0138025
```

### Standardization
- Standardization dataset with (mean=0, variance=1) scale is necessary as it removes the biases in the original 
  variables. For example, when the data for each variable is collected on different units.
- The standardized variables will be unitless and have a similar variance.
- Standardization is an advisable method for data transformation when the variables in the original dataset have been 
  measured on a significantly different scale.
- At some cases, the dataset needs not to be standardized as the original variation in the dataset is important (Gewers et al., 2018)

```python
# this is an optional step
>>> df_st =  StandardScaler().fit_transform(df)  
# see few rows of standardized dataset
>>> pd.DataFrame(df_st, columns=df.columns).head()
          A         B         C         D         E         F
0  0.619654  0.448280 -0.240867  2.457058  2.304732 -0.331489
1  0.342286 -0.041499 -0.428652 -1.214732 -0.877151  0.474930
2  0.603329  0.494693 -0.176385  2.896569  3.133729  0.109563
3 -0.032825  0.413423  0.449383  2.497462  1.629707 -1.171850
4  0.021968 -0.411444 -0.445350  3.764964  2.965869 -1.160617
```

### Perform PCA

```python
>>> pca_out = PCA().fit(df_st)

# get the component variance
# Proportion of Variance (from PC1 to PC6)
>>> pca_out.explained_variance_ratio_
array([0.2978742 , 0.27481252, 0.23181442, 0.19291638, 0.00144353,
       0.00113895])

# Cumulative proportion of variance (from PC1 to PC6)   
>>> np.cumsum(pca_out.explained_variance_ratio_)
array([0.2978742 , 0.57268672, 0.80450114, 0.99741752, 0.99886105,
       1.        ])
       
# get component loadings (correlation coefficient between original variables and the component) 
# the squared loadings within the PCs always sums to 1
>>> loadings = pca_out.components_
>>> num_pc = pca_out.n_features_
>>> pc_list = ["PC"+str(i) for i in list(range(1, num_pc+1))]
>>> loadings_df = pd.DataFrame.from_dict(dict(zip(pc_list, loadings)))
>>> loadings_df['variable'] = df.columns.values
>>> loadings_df = loadings_df.set_index('variable')
>>> loadings_df
               PC1       PC2       PC3       PC4       PC5       PC6
variable
A        -0.510898  0.452234  0.227356 -0.323464  0.614881  0.008372
B        -0.085908  0.401197  0.708556  0.132788 -0.558448 -0.010616
C         0.477477 -0.100994  0.462437  0.487951  0.556605  0.007893
D         0.370318  0.611485 -0.308295  0.054973 -0.007642  0.625159
E         0.568491  0.300118 -0.011775 -0.484115  0.009382 -0.593425
F         0.208090 -0.400426  0.370440 -0.634234 -0.010111  0.506732

# positive and negative values in component loadings reflects the positive and negative correlation of the variables
# with then PCs. 

# get correlation matrix plot for loadings
>>> import seaborn as sns
>>> import matplotlib.pyplot as plt
>>> ax = sns.heatmap(loadings_df, annot=True, cmap='Spectral')
>>> plt.show()

```
Generated correlation matrix plot for loadings,
<p align="center">
<img src="/assets/posts/pca/loading_corr.svg" width="600">
</p>

### Principal component (PC) retention
- As the number of PCs is equal to the number of original variables, We should keep only the PCs which explain the most variance 
  (70-95%) to make the interpretation easier. More the PCs you include that explains most variation in the original 
  data, better will be the PCA model. This is highly subjective and based on the user interpretation 
  (Cangelosi et al., 2007). 
- The eigenvalues for PCs can help to retain the number of PCs. Generally, PCs with eigenvalues > 1 contributes greater 
  variance and should be retained for further analysis.
- Scree plot (for elbow test) is another graphical technique useful in PCs retention. We should keep the PCs where 
  there is a sharp change in the slope of the line connecting adjacent PCs. 
  
        
```python
# get eigenvalues (from PC1 to PC6)  
>>> pca_out.explained_variance_
array([1.78994905, 1.65136965, 1.39299071, 1.15924943, 0.0086743 ,
       0.00684401])
# get scree plot (for scree or elbow test)
>>> from bioinfokit.visuz import cluster
>>> cluster.screeplot(obj=[pc_list, pca_out.explained_variance_ratio_])
# Scree plot will be saved in the same directory with name screeplot.png
```

Generated Scree plot,
<p align="center">
<img src="/assets/posts/pca/screeplot.svg" width="600">
</p>

### PCA loadings plots

```python
# get PCA loadings plots (2D and 3D)
# 2D
>>> cluster.pcaplot(x=loadings[0], y=loadings[1], labels=df.columns.values, 
    var1=round(pca_out.explained_variance_ratio_[0]*100, 2),
    var2=round(pca_out.explained_variance_ratio_[1]*100, 2))

# 3D
>>> cluster.pcaplot(x=loadings[0], y=loadings[1], z=loadings[2],  labels=df.columns.values, 
    var1=round(pca_out.explained_variance_ratio_[0]*100, 2), var2=round(pca_out.explained_variance_ratio_[1]*100, 2), 
    var3=round(pca_out.explained_variance_ratio_[2]*100, 2))
```

Generated 2D PCA loadings plot (2 PCs) plot,
<p align="center">
<img src="/assets/posts/pca/pcaplot_2d.svg" width="600">
</p>

Generated 3D PCA loadings plot (3 PCs) plot,
<p align="center">
<img src="/assets/posts/pca/pcaplot_3d.svg" width="600">
</p>

### PCA biplot
- In biplot, the PC loadings and scores are plotted in a single figure
- biplots are useful to visualize the relationships between variables and observations

```python
# get PC scores
>>> pca_scores = PCA().fit_transform(df_st)

# get 2D biplot
>>> cluster.biplot(cscore=pca_scores, loadings=loadings, labels=df.columns.values, var1=round(pca_out.explained_variance_ratio_[0]*100, 2),
    var2=round(pca_out.explained_variance_ratio_[1]*100, 2))
    
# get 3D biplot
>>> cluster.biplot(cscore=pca_scores, loadings=loadings, labels=df.columns.values, 
    var1=round(pca_out.explained_variance_ratio_[0]*100, 2), var2=round(pca_out.explained_variance_ratio_[1]*100, 2), 
    var3=round(pca_out.explained_variance_ratio_[2]*100, 2))
```

Generated 2D biplot,
<p align="center">
<img src="/assets/posts/pca/biplot_2d.svg" width="600">
</p>

Generated 3D biplot,
<p align="center">
<img src="/assets/posts/pca/biplot_3d.svg" width="600">
</p>

In addition to these features, we can also control the label fontsize,
figure size, resolution, figure format, and other many parameters for scree plot, loadings plot and biplot.

Check detailed <a href='https://reneshbedre.github.io/blog/howtoinstall.html' target='_blank'>usage</a>

## <span style="color:#33a8ff">PCA interpretation</span>
- The first three PCs (3D) contribute  ~81% of the total variation in the dataset  and have eigenvalues > 1, and thus 
  provides a good approximation of the variation present in the original 6D dataset (see the cumulative proportion of 
  variance and scree plot). The cut-off of cumulative 70% variation is common to retain the PCs for analysis 
  (Jolliffe et al., 2016). Even though the first four PCs contribute  ~99% and have eigenvalues > 1, it will be 
  difficult to visualize them at once and needs to perform pairwise visualization.
- From the biplot and loadings plot, we can see the variables D and E are highly associated and forms cluster (gene 
  expression response in D and E conditions are highly similar). Similarly, A and B are highly associated and forms 
  another cluster (gene expression response in A and B conditions are highly similar but different from other clusters). 
  If the variables are highly associated, the angle between the variable vectors should be as small as possible in the 
  biplot.
- The length of PCs in biplot refers to the amount of variance contributed by the PCs. The longer the length of PC, 
  the higher the variance contributed and well represented in space.


## <span style="color:#33a8ff">References</span>
- Pedregosa F, Varoquaux G, Gramfort A, Michel V, Thirion B, Grisel O, Blondel M, Prettenhofer P, Weiss R, Dubourg V, Vanderplas J.
  Scikit-learn: Machine learning in Python. the Journal of machine Learning research. 2011 Nov 1;12:2825-30.
- Abdi H, Williams LJ. Principal component analysis. Wiley interdisciplinary reviews: computational statistics. 2010 Jul;2(4):433-59.
- Jolliffe IT, Cadima J. Principal component analysis: a review and recent developments. Philosophical Transactions of the Royal Society A:
  Mathematical, Physical and Engineering Sciences. 2016 Apr 13;374(2065):20150202.
- Gewers FL, Ferreira GR, de Arruda HF, Silva FN, Comin CH, Amancio DR, Costa LD. Principal component analysis: A natural approach to data
  exploration. arXiv preprint arXiv:1804.02502. 2018 Apr 7.
- Bedre R, Rajasekaran K, Mangu VR, Timm LE, Bhatnagar D, Baisakh N. Genome-wide transcriptome analysis of cotton (Gossypium hirsutum L.)
  identifies candidate gene signatures in response to aflatoxin producing fungus Aspergillus flavus. PLoS One. 2015;10(9).
- Kirkwood RN, Brandon SC, de Souza Moreira B, Deluzio KJ. Searching for stability as we age: the PCA-Biplot approach. International
  Journal of Statistics in Medical Research. 2013 Oct 1;2(4):255.
- Cangelosi R, Goriely A. Component retention in principal component analysis with application to cDNA microarray data. 
  Biology direct. 2007 Dec 1;2(1):2.  
- Vallejos CA. Exploring a world of a thousand dimensions. Nature Biotechnology. 2019 Dec;37(12):1423-4.  


<!--
Transcriptomics  experiments such as RNA-seq allows researchers to study large numbers of genes across multiple treatment
conditions simultaneously. There have been various analytical methods has been used to deduce underlying biological
information from gene expression data such as heatmaps, clustering, PCA, Venn diagrams, gene networks and so on.

PCA is a classical multivariate (unconstrained ordination) statistical method that used to interpret the variation in dataset usually when the dataset
contains a large number of variables to study. PCA does this by reducing the dimensionality of the dataset by transforming old variable into a new
set of variables called principal component (PC), which are easy to visualize and summarise the features of datasets.
In gene expression datasets, variables can be treatment comparisons, gene expression counts across various conditions,
individual samples etc.

For example, when datasets contain 10 variables (10D), it is arduous to visualize them at the same time
(you may have to do 45 pairwise comparisons to interpret dataset effectively). PCA transforms them into a new set of
variables (PCs) with top PCs having highest variation. Most of time first 3 PCs contribute most of the variance. These
top 2 or 3 PCs can be plotted easily and summarize and/or clusters the features of all 10 variables.

For simplicity and understanding, here I am doing PCA analysis and visualization on small dataset (4 variables). But, 
once you understood, you can follow similar steps to run PCA analysis and visualization on large gene expression 
<a href="/assets/posts/pca/pca_example_dataset.csv">dataset</a>. (This dataset contains 13324 genes and 18 variables (A to R). 
These variables represent the log2 expression fold changes between different treatments) 

```python
# We will use `bioinfokit` for PCA analysis
# download and install bioinfokit (Tested on Linux, Mac, Windows) 
# Read documentation at https://github.com/reneshbedre/bioinfokit
git clone https://github.com/reneshbedre/bioinfokit.git
cd bioinfokit
python3 setup.py install
```

```python
# you can use interactive python console, jupyter or python code
# I am using interactive python console (Python 3.6)
# Read documentation at https://github.com/reneshbedre/bioinfokit
>>>from bioinfokit import analys
>>>import pandas as pd
# load data file
>>>d = pd.read_csv("https://reneshbedre.github.io/myfiles/bioinfokit_data/pca_small_dataset.csv")
#view first few rows
>>>d.head()
 	rowid 	A 	B 	C 	D
0 	row1 	25 	45 	30 	54
1 	row2 	30 	55 	29 	60
2 	row3 	28 	29 	33 	51
3 	row4 	36 	56 	37 	62
4 	row5 	29 	40 	27 	73
# make sure the dataframe should not have first identifier columns. 
# if table has identifier columns, drop it using d=d.drop('column_name', 1) 
>>>d = d.drop("rowid", 1)
# now, perform pca analysis and visualization
>>>analys.pca(table=d)

Component summary

                             PC1       PC2       PC3       PC4
Proportion of Variance  0.619541  0.291498  0.085472  0.003489
Cumulative proportion   0.619541  0.911039  0.996511  1.000000

Loadings

      PC1       PC2       PC3       PC4
A -0.222185 -0.020970  0.671764  0.706348
B -0.905969 -0.336097 -0.251237 -0.056019
C -0.042114 -0.285112  0.682941 -0.671215
D -0.357882  0.897391  0.138581 -0.217728
# plot will be saved in same directory (screeplot.png, pcaplot_2d.png, and pcaplot_3d.png)
```

Generated Scree plot,

![screenshot]({{ "/assets/posts/pca/screeplot.png" | absolute_url }})


Generated PCA plot (2 PCs) plot,

![screenshot]({{ "/assets/posts/pca/pcaplot_2d.png" | absolute_url }})


Generated PCA plot (3 PCs) plot,

![screenshot]({{ "/assets/posts/pca/pcaplot_3d.png" | absolute_url }})

<!--

In this tutorial, I will discuss in detail how to perform PCA analysis, visualization and interpretation using sample gene
expression data. For PCA analysis and visualization, I will use [prcomp](https://www.rdocumentation.org/packages/stats/versions/3.4.3/topics/prcomp)
and [scatter3d](https://www.rdocumentation.org/packages/car/versions/2.1-6/topics/scatter3d) R packages.

<p>Sample dataset used in this tutorial <a href="/myfiles/pca_example_dataset.csv">dataset</a>. This sample
gene expression dataset contains 13324 genes and 18 variables (A to R). These variables represent the log2 expression
fold changes between different treatments.</p>

First, let's load the R libraries needed for analysis. You can install using `install.packages("package")` or using
`bioclite()` [How to install R packages](https://www.r-bloggers.com/installing-r-packages/)

```r
# load libraries
>library(gplots)
>library(plot3D)
>library(factoextra)

# import data
>data_table = read.csv("pca_example_dataset.csv", header=TRUE, row.names=1)

# convert table to matrix
>data_table_mat = data.matrix(data_table)

# remove all rows where all columns have 0 values; this is optional
>data_table_mat = data_table_mat[!apply(data_table_mat, 1,
                  function(x) all(x == 0)), ]

# check the dimension of data; this is optional
>dim(data_table_mat)

# perform PCA analysis
# As the gene expression data represents log2 fold change for each variable,
# I did not use data scaling. But if your data is not uniform set scale. to TRUE
>pca = prcomp(data_table_mat, center = TRUE, scale. = FALSE)
>summary(pca)

Importance of components:
                          PC1    PC2    PC3    PC4    PC5    PC6     PC7     PC8     PC9    PC10    PC11    PC12    PC13    PC14    PC15    PC16    PC17    PC18
Standard deviation     1.9455 1.4881 1.0065 0.8670 0.8059 0.5280 0.44982 0.29553 0.20272 0.16985 0.16136 0.15739 0.15648 0.14507 0.14080 0.12359 0.10881 0.07024
Proportion of Variance 0.4114 0.2407 0.1101 0.0817 0.0706 0.0303 0.02199 0.00949 0.00447 0.00314 0.00283 0.00269 0.00266 0.00229 0.00215 0.00166 0.00129 0.00054
Cumulative Proportion  0.4114 0.6521 0.7622 0.8439 0.9145 0.9448 0.96679 0.97629 0.98075 0.98389 0.98672 0.98941 0.99207 0.99436 0.99652 0.99818 0.99946 1.00000

> print(pca)
Standard deviations:
 [1] 1.9454740 1.4880623 1.0064682 0.8669863 0.8059281 0.5279759 0.4498158 0.2955292 0.2027190 0.1698526 0.1613629 0.1573882 0.1564750 0.1450662 0.1407983 0.1235900
[17] 0.1088081 0.0702448

Rotation:
            PC1           PC2           PC3           PC4           PC5           PC6           PC7           PC8          PC9        PC10         PC11          PC12
A -1.343097e-02  6.323911e-02 -0.0691930665 -0.0251485010  2.648116e-01 -0.0068880765  0.7378477434 -2.797293e-02 -0.013313448  0.45133597 -0.047667148  3.798500e-01
B  9.944610e-04  2.052127e-03 -0.0010546396 -0.0014434767 -3.881107e-04  0.0119064932  0.0043613575  6.916949e-02  0.581932245  0.03253789 -0.028603243  4.767295e-02
C -8.140684e-02  1.399650e-01 -0.1785363909 -0.1751961080  9.017922e-01  0.0181988685 -0.1205413085 -6.525849e-05  0.003943627 -0.21588497  0.016013714 -1.738292e-01
D  1.077459e-03  5.397563e-03 -0.0017227414 -0.0039767523  1.267614e-03  0.0026945290  0.0033614634  1.299098e-01  0.698601169  0.02934107 -0.005190133  8.856802e-03
E -2.649007e-02  1.596509e-02 -0.0188695088 -0.0418816412  1.298501e-01  0.0055685428 -0.6620367699  3.339199e-03 -0.014262923  0.54107576 -0.051299478  4.548146e-01
F  1.037250e-05 -2.405279e-04 -0.0006820423 -0.0006352852 -2.638620e-04  0.0002915963 -0.0031961689  9.196993e-03  0.033868485 -0.00936920 -0.011872259 -3.157451e-02
G -2.386615e-01  6.197818e-01  0.1332794399  0.5103241553  1.868681e-03 -0.0003165033 -0.0283642481 -3.434216e-03  0.006738578  0.10670154 -0.005890525 -5.706278e-03
H  1.391363e-04  7.681416e-05  0.0037942991  0.0009830673 -1.355079e-05  0.0180411335  0.0011590960 -8.100548e-02 -0.244308428  0.11649427  0.371923927  1.430474e-01
I -3.025029e-01  6.429415e-01  0.1339030619 -0.4223749927 -1.832009e-01  0.0015723094  0.0152779069  2.410008e-03 -0.011718735 -0.11804666  0.003760751  1.711777e-04
J  2.560176e-04 -2.473587e-03  0.0009164680  0.0065768346  3.258796e-03 -0.0004740609  0.0007920751 -1.553285e-02 -0.240716220 -0.04818989  0.160745710 -1.576680e-02
K -4.744437e-02  2.908348e-03 -0.0041924433 -0.7225472267 -1.425278e-01  0.0057912403  0.0134547203 -4.342178e-03  0.004161067  0.14724539 -0.005147585  4.745356e-06
L  4.715754e-05  4.907378e-04  0.0003933394 -0.0005349674  1.659020e-03  0.0052901093 -0.0010713225  1.314869e-02  0.014867245 -0.06628550  0.085932013 -5.681064e-02
M -1.638196e-01  6.155082e-02 -0.7333155095  0.0443475349 -1.597227e-01 -0.0132622111 -0.0110459916 -6.070002e-03  0.004825864  0.38847026  0.095382102 -4.852087e-01
N -1.033679e-03 -3.308257e-03 -0.0014869916  0.0011950672 -6.403041e-03 -0.0047481981 -0.0090857791 -3.165633e-01 -0.109896382  0.06077214 -0.858276753 -1.179570e-01
O -7.435837e-01 -2.737301e-01 -0.2931561888  0.0624532403 -7.351970e-02  0.0165813972  0.0107652654  8.034087e-03 -0.003647926 -0.32700124 -0.073697374  3.983436e-01
P -1.887610e-02 -8.281844e-03  0.0120953610 -0.0055112850  9.161067e-03 -0.7081026816 -0.0215197071 -6.568586e-01  0.147450699 -0.03377019  0.186836647  2.342964e-02
Q -5.112824e-01 -3.158753e-01  0.5507435665  0.0047827881  1.207506e-01  0.0237725778  0.0255365539 -3.784083e-03  0.009165146  0.35232174  0.079283799 -4.270666e-01
R -1.580017e-02 -5.545668e-03  0.0095338169 -0.0054161090  1.634094e-02 -0.7047010118  0.0077147106  6.623119e-01 -0.140628767  0.02876676 -0.172816683 -1.918284e-02
         PC13        PC14        PC15         PC16          PC17          PC18
A -0.13144587 -0.08098070 -0.02454076  0.005024232  0.0068548628 -0.0087163304
B  0.22489236 -0.06594585  0.01655057 -0.723954644 -0.1793772902 -0.2019050547
C  0.06350182  0.04249943  0.01290563 -0.002550700 -0.0029193380  0.0028313002
D  0.13625476 -0.18425319  0.21476143  0.595999291 -0.0422636595  0.1959741840
E -0.15526624 -0.11547560 -0.04177224  0.002490795  0.0073294659 -0.0059979130
F -0.06090704 -0.01877042  0.02292072  0.274529691  0.0080100137 -0.9578691426
G  0.03395061  0.42286852  0.29340827  0.001678223 -0.0481454837 -0.0044208407
H  0.81614494 -0.17197121  0.15349452  0.035460715  0.1943202854 -0.0529466757
I -0.03138874 -0.40906158 -0.28385963 -0.003310920  0.0445140567  0.0033739474
J -0.12581248 -0.37763772  0.45723299 -0.017328250 -0.7385870070  0.0055287452
K  0.04011712  0.53449469  0.37638339  0.001563203 -0.0662633365 -0.0049186038
L -0.32965389 -0.30625160  0.60501298 -0.206034590  0.6111525072 -0.0103873038
M -0.01077729 -0.06532134 -0.06058002 -0.015428480  0.0009911860  0.0077789306
N  0.26634321 -0.17244991  0.18012991  0.010423377  0.0082944518  0.0008488798
O  0.01182740  0.05142697  0.04918092  0.012848866  0.0001328309 -0.0057525674
P -0.07705292  0.02894234 -0.02557832  0.010912012 -0.0091597356  0.0027707527
Q -0.01330507 -0.05801475 -0.05504574 -0.012494298  0.0008232983  0.0065952811
R  0.10040449 -0.03144543  0.03677366 -0.021420805  0.0155227342 -0.0072646567
```

From the analysis, we can see that PC1 contributes to highest variation (41%) followed by PC2 and so on. Overall, first
three PC compoenent add to ~77% of total variance and can summarize all 18 variables.

```r
> fviz_eig(pca)
```
![screenshot]({{ "/myfiles/pca_blog_screeplot.png" | absolute_url }})

*<span style="color:#696969">Fig 1: Screeplot represents eigenvalues of PCs and percentages of variation contributed by
each PC. Top principal component contribute maximum (~77%) of the total variance (only 10 PCs shown)</span>*

Now, plot the variables along the top three PCs.

```r
pca_rot = data.frame(pca$rotation)
# plot 3D
scatter3D(pca_rot$PC1, pca_rot$PC2, pca_rot$PC3, bty = "g", phi=0, xlab = "PC1", ylab ="PC2", zlab = "PC3",
            pch = 20, cex = 2, ticktype = "detailed", colvar=pca_rot$PC1, col = ramp.col(c("gold", "black", "blue")),
            colkey = list(side = 4, length = 0.5))
# add label to variables
text3D(pca_rot$PC1, pca_rot$PC2, pca_rot$PC3,  labels = rownames(pca_rot), add = TRUE, colkey = FALSE, cex = 1)
```
![screenshot]({{ "/myfiles/pca_3d.png" | absolute_url }})

*<span style="color:#696969">Fig 2: Scatterplot of variables (A to R) with respect to top three PCs, which explains
maximum (~77%) of the total variance. Color scale represents the rotation of PC1. </span>*

Interpretation: From above PCA analysis, it can be concluded that all variables except M, O, and Q has similar gene expression
profiles and form one cluster. Variables M, O and Q has high gene expression changes with respect to other variables
and form an individual cluster. From the 18 variables, it will be important to focus on M, O and Q variables.

**R and package version information used in the analysis**
```r
> sessionInfo()
R version 3.3.1 (2016-06-21)
Platform: x86_64-w64-mingw32/x64 (64-bit)
Running under: Windows 7 x64 (build 7601) Service Pack 1

locale:
[1] LC_COLLATE=English_United States.1252  LC_CTYPE=English_United States.1252    LC_MONETARY=English_United States.1252
[4] LC_NUMERIC=C                           LC_TIME=English_United States.1252

attached base packages:
[1] grid      parallel  stats     graphics  grDevices utils     datasets  methods   base

other attached packages:
 [1] plot3D_1.1.1        factoextra_1.0.5    ggplot2_2.2.1       knitr_1.17          kableExtra_0.7.0
 [6] gplots_3.0.1        Vennerable_3.0      xtable_1.8-2        gtools_3.5.0        reshape_0.8.6
[11] RColorBrewer_1.1-2  RBGL_1.50.0         graph_1.52.0        SeqGSEA_1.14.0      DESeq_1.26.0
[16] lattice_0.20-34     locfit_1.5-9.1      doParallel_1.0.10   iterators_1.0.8     foreach_1.4.3
[21] Biobase_2.34.0      BiocGenerics_0.20.0

loaded via a namespace (and not attached):
 [1] ggrepel_0.7.0        Rcpp_0.12.7          assertthat_0.1       rprojroot_1.3-2      digest_0.6.12
 [6] R6_2.2.0             plyr_1.8.4           backports_1.1.2      stats4_3.3.1         RSQLite_1.0.0
[11] evaluate_0.10.1      httr_1.2.1           highr_0.6            pillar_1.1.0         rlang_0.1.6
[16] misc3d_0.8-4         lazyeval_0.2.0       annotate_1.52.0      gdata_2.17.0         S4Vectors_0.12.0
[21] Matrix_1.2-7.1       rmarkdown_1.9        labeling_0.3         splines_3.3.1        readr_1.1.1
[26] geneplotter_1.52.0   stringr_1.3.0        RCurl_1.95-4.8       biomaRt_2.30.0       munsell_0.4.3
[31] pkgconfig_2.0.1      htmltools_0.3.6      tibble_1.4.2         IRanges_2.8.1        codetools_0.2-15
[36] XML_3.98-1.4         viridisLite_0.2.0    dplyr_0.7.4          ggpubr_0.1.6         bitops_1.0-6
[41] gtable_0.2.0         DBI_0.8              magrittr_1.5         scales_0.4.1         KernSmooth_2.23-15
[46] stringi_1.1.7        genefilter_1.56.0    bindrcpp_0.2         xml2_1.2.0           tools_3.3.1
[51] glue_1.2.0           hms_0.4.2            survival_2.39-5      AnnotationDbi_1.36.0 colorspace_1.3-2
[56] caTools_1.17.1       rvest_0.3.2          bindr_0.1
```


-->


<p>
{% include  cite.html %}
</p>

<p>
{% include  share.html %}
</p>


<span style="color:#9e9696"><i> Last updated: September 26, 2020</i> </span>


<p>
{% include  license.html %}
</p>