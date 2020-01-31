---
layout: post
title: "One-way ANOVA using Python"
date:   2020-01-31 06:18:08
author: Renesh Bedre
description: "Advanced Bioinformatics"
permalink: blog/oanova.html
comments: true
hidden: true
---




Read basic about, [one-way ANOVA analysis]({{"/blog/anova.html" | absolute_url }})

We will use bioinfokit for performing one-way ANOVA analysis

**<span style="color:#33a8ff">Install bioinfokit</span>**
```python
# We will use `bioinfokit` 
# download and install bioinfokit (Tested on Linux, Mac, Windows) 
# read instructions https://github.com/reneshbedre/bioinfokit
git clone https://github.com/reneshbedre/bioinfokit.git
cd bioinfokit
python setup.py install
```   

Example data for one-way ANOVA analysis, [dataset]({{"/myfiles/anova/onewayanova.txt" | absolute_url }})

```python
# load packages
import pandas as pd
d = pd.read_csv("https://reneshbedre.github.io/myfiles/anova/onewayanova.txt", sep="\t")
>>> d.head()
    A   B   C   D
0  25  45  30  54
1  30  55  29  60
2  28  29  33  51
3  36  56  37  62
4  29  40  27  73

# convert it as a stacked table 
d_melt = pd.melt(d.reset_index(), id_vars=['index'], value_vars=['A', 'B', 'C', 'D'])
>>> d_melt.head()
   index variable  value
0      0        A     25
1      1        A     30
2      2        A     28
3      3        A     36
4      4        A     29
```

Now, perform one-way ANOVA using bioinfokit

```python
# load packages
# check documentation at 
from bioinfokit import analys
analys.stat.oanova(d=d_melt, res="value", xfac="variable", ph=True)

One-way ANOVA Summary

              sum_sq    df         F    PR(>F)
C(variable)  3010.95   3.0  17.49281  0.000026
Residual      918.00  16.0       NaN       NaN



Post-hoc Tukey HSD test

 Multiple Comparison of Means - Tukey HSD, FWER=0.05
=====================================================
group1 group2 meandiff p-adj   lower    upper  reject
-----------------------------------------------------
     A      B     15.4 0.0251   1.6929 29.1071   True
     A      C      1.6    0.9 -12.1071 15.3071  False
     A      D     30.4  0.001  16.6929 44.1071   True
     B      C    -13.8 0.0482 -27.5071 -0.0929   True
     B      D     15.0 0.0296   1.2929 28.7071   True
     C      D     28.8  0.001  15.0929 42.5071   True
-----------------------------------------------------

ANOVA Assumption tests

Shapiro-Wilk (P-value): 0.7229810953140259

Bartlett (P-value): 0.1278253399753447


```

<b>Interpretation</b>: The P-value obtained from ANOVA analysis is 
significant (P=0.000026), and therefore, we conclude that there are 
significant differences among treatments.

Above results from Tukey HSD suggests that except A-C, all other 
pairwise comparisons for treatments rejects null hypothesis and 
indicates statistical significant differences.

The <b>Shapiro-Wilk test</b> can be used to check the <b> normal 
distribution of residuals </b>. <i>Null hypothesis</i>: 
data is drawn from normal distribution.
As the P-value is non significant (P=0.72), we fail to reject null 
hypothesis and conclude that data is drawn from normal distribution.

Use Bartlett’s test to check the <b>Homogeneity of variances</b>.
 <i>Null hypothesis</i>: samples from populations 
have equal variances.
As the P-value (P=0.12) is non significant, we fail to reject null 
hypothesis and conclude that treatments have equal variances.

Note: <b>Levene test</b> can be used to check the Homogeneity of variances
when the data is not drawn from normal distribution.

**<span style="color:#33a8ff">How to cite?</span>**

<!--
Bedre, R. “ANOVA using Python” Renesh Bedre (blog), October 22, 2018, 
https://reneshbedre.github.io/blog/anova.html.
-->
Bedre, R. Bioinformatics data analysis and visualization toolkit. 
GitHub repository, <a href="https://github.com/reneshbedre/bioinfokit">https://github.com/reneshbedre/bioinfokit</a>


<span style="color:#9e9696">If you have any questions, comments or recommendations, please email me at 
<b>reneshbe@gmail.com</b></span>
    
<span style="color:#9e9696"><i> Last updated: January 31, 2020</i> </span>


<div>

<a href="https://twitter.com/renbedre?ref_src=twsrc%5Etfw" class="twitter-follow-button" data-show-count="false">Follow @renbedre</a><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<a href="https://twitter.com/share?url=https://reneshbedre.github.io/blog/anova.html&amp;text=ANOVA%20using%20Python&amp;hashtags=" target="_blank">
        <img src="https://simplesharebuttons.com/images/somacro/twitter.png" alt="Twitter" style="width: 35px;"/>
    </a>
<a href="http://www.facebook.com/sharer.php?u=https://reneshbedre.github.io/blog/anova.html" target="_blank">
        <img src="https://simplesharebuttons.com/images/somacro/facebook.png" alt="Facebook" style="width: 35px;"/>
    </a>    
    
</div>