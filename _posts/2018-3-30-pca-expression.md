---
layout: post
title: "Principal component analysis (PCA) analysis and visualization of gene expression data using R"
date:   2018-03-30 06:18:08
author: Renesh Bedre
description: "Advanced Bioinformatics"
permalink: blog/pca_3d.html
comments: true
---

Transcriptomics  experiments such as RNA-seq allows researchers to study large numbers of genes across multiple treatment
conditions simultaneously. There have been various analytical methods has been used to deduce underlying biological
information from gene expression data such as heatmaps, clustering, PCA, Venn diagrams, gene networks and so on.

PCA is a classical multivariate statistical method that used to interpret the variation in dataset usually when the dataset
contains a large number of variables to study. PCA does this by reducing the dimensionality of the dataset by transforming old variable into a new
set of variables called principal component (PC), which are easy to visualize and summarise the features of datasets.
In gene expression datasets, variables can be treatment comparisons, gene expression counts across various conditions,
individual samples etc.

For example, when datasets contain 10 variables (10D), it is arduous to visualize them at the same time
(you may have to do 45 pairwise comparisons to interpret dataset effectively). PCA transforms them into a new set of
variables (PCs) with top PCs having highest variation. Most of time first 3 PCs contribute most of the variance. These
top 2 or 3 PCs can be plotted easily and summarize and/or clusters the features of all 10 variables.

In this tutorial, I will discuss in detail how to perform PCA analysis, visualization and interpretation using sample gene
expression data. For PCA analysis and visualization, I will use [prcomp](https://www.rdocumentation.org/packages/stats/versions/3.4.3/topics/prcomp)
and [scatter3d](https://www.rdocumentation.org/packages/car/versions/2.1-6/topics/scatter3d) R packages.

Sample dataset used in this tutorial [dataset]({{"/myfiles/pca_example_dataset.csv" | absolute_url }}). This sample
gene expression dataset contains 13324 genes and 18 variables (A to R). These variables represent the log2 expression
fold changes between different treatments.

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