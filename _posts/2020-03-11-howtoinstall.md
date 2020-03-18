---
title: "How to install bioinfokit"
date:   2020-03-11 02:15:18
permalink: blog/howtoinstall.html
author_profile: true
classes: wide
---

<p>
{% include  share.html %}
</p>

bioinfokit can be installed using pip and git. Before installing, please check the
requirement for `bioinfokit` below,

**<span style="color:#33a8ff">How to install:</span>**

`bioinfokit` requires
- Python 3
- NumPy
- scikit-learn
- seaborn
- pandas
- matplotlib
- SciPy

Install using <a href="https://pip.pypa.io/en/stable/installing/" target="_blank">pip</a> for Python 3 (easiest way)

```python
# install
pip install bioinfokit

# uninstall 
pip uninstall bioinfokit
```

Install using <a href="https://setuptools.readthedocs.io/en/latest/easy_install.html" target="_blank">easy_install</a> for Python 3 (easiest way)
```python
# install latest version
easy_install bioinfokit

# specific version
easy_install bioinfokit==0.3

# uninstall 
pip uninstall bioinfokit
```


Install using <a href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git" target="_blank">git</a>

```python
# download and install bioinfokit (Tested on Linux, Mac, Windows) 
git clone https://github.com/reneshbedre/bioinfokit.git
cd bioinfokit
python setup.py install
```

<p>
{% include  share.html %}
</p>


<span style="color:#9e9696"><i> Last updated: March 11, 2020</i> </span>