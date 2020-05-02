---
title: "New good features in Python 3.9"
date:   2020-04-29 06:18:08
author_profile: true
permalink: blog/newfeaturespython39.html
classes: wide
tags:
  - Python
---
<p>
{% include  share.html %}
</p>

- Python 3.9 is still in development and expected to release final version on October 2020
- Currently, <a href='https://www.python.org/downloads/release/python-390a6/' target='_blank'>3.9.0a6</a> version is available for download as of April 29, 2020
- Note: Here, I have covered some of the major features of Python 3.9 and a complete list of of all features can found
  <a href='https://docs.python.org/3.9/whatsnew/3.9.html' target='_blank'>here</a>

## <span style="color:#33a8ff">Merge two dictionaries (dicts) using union operators</span>

```python
# I am using interactive python interpreter (Python 3.9.0a6)
# create two dicts
>>> d1 = {1: 'one', 2: 'two', 3: 'three'}
>>> d2 = {4: 'four', 5: 'five', 6: 'six'}

# merge two dicts in python 3.9
>>> d1 | d2
{1: 'one', 2: 'two', 3: 'three', 4: 'four', 5: 'five', 6: 'six'}

# merge two dicts in python (3.5 to 3.8)
>>> {**d1, **d2}
{1: 'one', 2: 'two', 3: 'three', 4: 'four', 5: 'five', 6: 'six'}

```

## <span style="color:#33a8ff">Update dictionaries (dicts) union operators</span>
```python
# update current dict with key-values from other dict (like appending the dict)
>>> d1 = {1: 'one', 2: 'two', 3: 'three'}
>>> d2 = {4: 'four', 5: 'five', 6: 'six'}

# update d1 in python 3.9
>>> d1 |= d2
>>> d1
{1: 'one', 2: 'two', 3: 'three', 4: 'four', 5: 'five', 6: 'six'}

# in python 3.8
>>> d1 = {1: 'one', 2: 'two', 3: 'three'}
>>> d2 = {4: 'four', 5: 'five', 6: 'six'}

>>> d1.update(d2)
>>> d1
{1: 'one', 2: 'two', 3: 'three', 4: 'four', 5: 'five', 6: 'six'}

```

## <span style="color:#33a8ff">String methods (`removeprefix()` and `removesuffix() `)</span>
```python
# in python 3.9
>>> s = 'python_390a6'

# apply removeprefix()
>>> s.removeprefix('python_')
'390a6'

# apply removesuffix()
>>> s = 'python.exe'
>>> s.removesuffix('.exe')
'python'

# in python 3.8 or before
>>> s = 'python_390a6'
>>> s.lstrip('python_')
'390a6'

>>> s = 'python.exe'
>>> s.rstrip('.exe')
'python'
```
Note: `removeprefix()` and `removesuffix() ` string methods added in Python 3.9 due to
issues associated with `lstrip` and `rstrip` interpretation of parameters passed to them.
Read  <a href='https://www.python.org/dev/peps/pep-0616' target='_blank'>PEP 616</a> for more details


## <span style="color:#33a8ff">`math` functions</span>
```python
# in python 3.9
math.gcd() function can take more than two arguments
math.lcm(*integers), math.ulp(x), and math.nextafter(x, y) functions added

```
Check detailed <a href="https://docs.python.org/3.9/library/math.html" target='_blank'>usage</a>

## <span style="color:#33a8ff">`readlink()` function added to `Path` module</span>
```python
# in python 3.9
>>> from pathlib import Path
>>> p = Path('/some/path/for/symlink_file')
>>> p.symlink_to('/some/path/for/existing_file')
# get the path for existing_file to which symlink_file points to
>>> p.readlink()
# should get PosixPath('/some/path/for/existing_file')

```

## <span style="color:#33a8ff">References</span>
- https://docs.python.org/3.9/whatsnew/3.9.html
- https://www.python.org/dev/peps/pep-0596/
- https://www.python.org/dev/peps/pep-0584
- https://www.python.org/dev/peps/pep-0616
- https://docs.python.org/3.9/library/math.html



<p>
{% include  share.html %}
</p>

<span style="color:#9e9696"><i> Last updated: May 1, 2020</i> </span>