---
source: hackernews
title: Show HN: High-Performance Wavelet Matrix for Python, Implemented in Rust
url: https://pypi.org/project/wavelet-matrix/
date: 2025-12-18
---

[Skip to main content](#content)
Switch to mobile version

Warning
Some features may not work without JavaScript. Please try enabling it if you encounter problems.

[![PyPI](/static/images/logo-small.8998e9d1.svg)](/)

Search PyPI

Search

* [Help](/help/)
* [Docs](https://docs.pypi.org/)
* [Sponsors](/sponsors/)
* [Log in](/account/login/?next=https%3A%2F%2Fpypi.org%2Fproject%2Fwavelet-matrix%2F)
* [Register](/account/register/)

Menu

* [Help](/help/)
* [Docs](https://docs.pypi.org/)
* [Sponsors](/sponsors/)
* [Log in](/account/login/)
* [Register](/account/register/)

Search PyPI

Search

# wavelet-matrix 2.0.3

pip install wavelet-matrix

Copy PIP instructions

[Latest version](/project/wavelet-matrix/)

Released:
Dec 16, 2025

High-performance indexed sequence data structure powered by Rust, supporting fast rank/select and range queries.

### Navigation

* [Project description](#description)
* [Release history](#history)
* [Download files](#files)

### Verified details

*These details have been [verified by PyPI](https://docs.pypi.org/project_metadata/#verified-details)*

###### Maintainers

[![Avatar for math-hiyoko from gravatar.com](https://pypi-camo.freetls.fastly.net/439d3d5c89e82d0411eaef9b3aef03340a527cc4/68747470733a2f2f7365637572652e67726176617461722e636f6d2f6176617461722f36346134656638653537396539343937363838653431616530366239323432373f73697a653d3530 "Avatar for math-hiyoko from gravatar.com")
math-hiyoko](/user/math-hiyoko/)

### Unverified details

*These details have **not** been verified by PyPI*

###### Project links

* [documentation](https://math-hiyoko.github.io/wavelet-matrix)
* [homepage](https://github.com/math-hiyoko/wavelet-matrix)
* [repository](https://github.com/math-hiyoko/wavelet-matrix)

###### Meta

* **License:** MIT License
* **Author:** Koki Watanabe
* Tags

  wavelet matrix
  ,

  data structure
  ,

  algorithm
  ,

  sequence index
  ,

  rank
  ,

  select
  ,

  quantile
  ,

  top-k
  ,

  range query
  ,

  dynamic update
  ,

  indexing
  ,

  succinct
* **Requires:** Python >=3.9

###### Classifiers

* **Intended Audience**
  + [Developers](/search/?c=Intended+Audience+%3A%3A+Developers)
  + [Information Technology](/search/?c=Intended+Audience+%3A%3A+Information+Technology)
  + [Science/Research](/search/?c=Intended+Audience+%3A%3A+Science%2FResearch)
* **License**
  + [OSI Approved :: MIT License](/search/?c=License+%3A%3A+OSI+Approved+%3A%3A+MIT+License)
* **Programming Language**
  + [Python :: 3](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3)
  + [Python :: 3.9](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.9)
  + [Python :: 3.10](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.10)
  + [Python :: 3.11](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.11)
  + [Python :: 3.12](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.12)
  + [Python :: 3.13](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.13)
  + [Python :: 3.14](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.14)
  + [Python :: Implementation :: CPython](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+Implementation+%3A%3A+CPython)
  + [Python :: Implementation :: PyPy](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+Implementation+%3A%3A+PyPy)
* **Topic**
  + [Scientific/Engineering :: Information Analysis](/search/?c=Topic+%3A%3A+Scientific%2FEngineering+%3A%3A+Information+Analysis)
  + [Scientific/Engineering :: Mathematics](/search/?c=Topic+%3A%3A+Scientific%2FEngineering+%3A%3A+Mathematics)
  + [Software Development :: Libraries :: Python Modules](/search/?c=Topic+%3A%3A+Software+Development+%3A%3A+Libraries+%3A%3A+Python+Modules)
* **Typing**
  + [Typed](/search/?c=Typing+%3A%3A+Typed)

[Report project as malware](https://pypi.org/project/wavelet-matrix/submit-malware-report/)

* [Project description](#description)
* [Project details](#data)
* [Release history](#history)
* [Download files](#files)

## Project description

# Wavelet Matrix

[![CI](https://pypi-camo.freetls.fastly.net/e045d1e0810360f20189c0f8a606505ac197f352/68747470733a2f2f6769746875622e636f6d2f6d6174682d6869796f6b6f2f776176656c65742d6d61747269782f616374696f6e732f776f726b666c6f77732f43492e796d6c2f62616467652e7376673f6272616e63683d6d61696e)](https://github.com/math-hiyoko/wavelet-matrix/actions/workflows/CI.yml)
[![codecov](https://pypi-camo.freetls.fastly.net/1affcb1124708306b5ba705d3e8ba560b827e0ce/68747470733a2f2f636f6465636f762e696f2f67682f6d6174682d6869796f6b6f2f776176656c65742d6d61747269782f67726170682f62616467652e7376673f746f6b656e3d54584252374d46324350)](https://codecov.io/gh/math-hiyoko/wavelet-matrix)
![PyPI - Version](https://pypi-camo.freetls.fastly.net/e0913b5a440cd4a854debddda46bf530458177f6/68747470733a2f2f696d672e736869656c64732e696f2f707970692f762f776176656c65742d6d6174726978)
![PyPI - License](https://pypi-camo.freetls.fastly.net/f44a05a3dae4e2a39726d2de5849220b95826d52/68747470733a2f2f696d672e736869656c64732e696f2f707970692f6c2f776176656c65742d6d6174726978)
![PyPI - PythonVersion](https://pypi-camo.freetls.fastly.net/835ba3e2c4afcc7db5ae53f1c5d0983378e811b1/68747470733a2f2f696d672e736869656c64732e696f2f707970692f707976657273696f6e732f776176656c65742d6d6174726978)
![PyPI - Implementation](https://pypi-camo.freetls.fastly.net/525c1e1bc7c4846dc2346e514af06b6e1b4010fa/68747470733a2f2f696d672e736869656c64732e696f2f707970692f696d706c656d656e746174696f6e2f776176656c65742d6d6174726978)
![PyPI - Types](https://pypi-camo.freetls.fastly.net/f09edf2f9434f3e810a9e65836469f3f17ce8985/68747470733a2f2f696d672e736869656c64732e696f2f707970692f74797065732f776176656c65742d6d6174726978)
[![PyPI - Downloads](https://pypi-camo.freetls.fastly.net/5836e8f3db80156b3612f284181d1c0eaeafab12/68747470733a2f2f696d672e736869656c64732e696f2f707970692f646d2f776176656c65742d6d61747269783f6c6162656c3d50795049253230646f776e6c6f616473)](https://pypistats.org/packages/wavelet-matrix)
![PyPI - Format](https://pypi-camo.freetls.fastly.net/e8780910335f10d6c40e418fe910e6c711c71eba/68747470733a2f2f696d672e736869656c64732e696f2f707970692f666f726d61742f776176656c65742d6d6174726978)
![Rust](https://pypi-camo.freetls.fastly.net/436779fd226959885f9e49a067ed5aac503bd943/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706f776572656425323062792d527573742d6f72616e6765)

High-performance indexed sequence structure powered by Rust, supporting fast rank/select and range queries with optional dynamic updates.

* PyPI: <https://pypi.org/project/wavelet-matrix>
* Document: <https://math-hiyoko.github.io/wavelet-matrix>
* Repository: <https://github.com/math-hiyoko/wavelet-matrix>

## Quick Start

```
$ pip install wavelet-matrix
```

### WaveletMatrix

```
>>> from wavelet_matrix import WaveletMatrix
>>>
>>> # Create a WaveletMatrix
>>> data = [5, 4, 5, 5, 2, 1, 5, 6, 1, 3, 5, 0]
>>> wm = WaveletMatrix(data)
>>> wm
WaveletMatrix([5, 4, 5, 5, 2, 1, 5, 6, 1, 3, 5, 0])
```

#### Count occurrences (rank)

```
>>> # Count of 5 in the range [0, 9)
>>> wm.rank(value=5, end=9)
4
```

#### Find position (select)

```
>>> # Find the index of 4th occurrence of value 5
>>> wm.select(value=5, kth=4)
6
```

#### Find k-th smallest (quantile)

```
>>> # Find 8th smallest value in the range [2, 12)
>>> wm.quantile(start=2, end=12, kth=8)
5
```

#### List top-k highest frequent values (topk)

```
>>> # List values in [1, 10) with the top-2 highest frequencies.
>>> wm.topk(start=1, end=10, k=2)
[{'value': 5, 'count': 3}, {'value': 1, 'count': 2}]
```

#### Sum values in a range (range\_sum)

```
>>> # Sum of elements in the range [2, 8).
>>> wm.range_sum(start=2, end=8)
24
```

#### List intersection of two ranges (range\_intersection)

```
>>> # List the intersection of two ranges [0, 6) and [6, 11).
>>> wm.range_intersection(start1=0, end1=6, start2=6, end2=11)
[{'value': 1, 'count1': 1, 'count2': 1}, {'value': 5, 'count1': 3, 'count2': 2}]
```

#### Count values in a range (range\_freq)

```
>>> # Count values c in the range [1, 9) such that 4 <= c < 6.
>>> wm.range_freq(start=1, end=9, lower=4, upper=6)
4
```

#### List values in a range (range\_list)

```
>>> # List values c in the range [1, 9) such that 4 <= c < 6.
>>> wm.range_list(start=1, end=9, lower=4, upper=6)
[{'value': 4, 'count': 1}, {'value': 5, 'count': 3}]
```

#### List top-k maximum values (range\_maxk)

```
>>> # List values in [1, 9) with the top-2 maximum values.
>>> wm.range_maxk(start=1, end=9, k=2)
[{'value': 6, 'count': 1}, {'value': 5, 'count': 3}]
```

#### List top-k minimum values (range\_mink)

```
>>> # List values in [1, 9) with the top-2 minimum values.
>>> wm.range_mink(start=1, end=9, k=2)
[{'value': 1, 'count': 2}, {'value': 2, 'count': 1}]
```

#### Get the maximun value (prev\_value)

```
>>> # Get the maximum value c in the range [1, 9) such that c < 7.
>>> wm.prev_value(start=1, end=9, upper=7)
6
```

#### Get the minimun value (next\_value)

```
>>> # Get the minimum value c in the range [1, 9) such that 4 <= c.
>>> wm.next_value(start=1, end=9, lower=4)
4
```

### Dynamic Wavelet Matrix

```
>>> from wavelet_matrix import DynamicWaveletMatrix
>>>
>>> # Create a DynamicWaveletMatrix
>>> # max_bit sets the maximum bit-width of stored values (auto-inferred if omitted).
>>> data = [5, 4, 5, 5, 2, 1, 5, 6, 1, 3, 5, 0]
>>> dwm = DynamicWaveletMatrix(data, max_bit=4)
>>> dwm
DynamicWaveletMatrix([5, 4, 5, 5, 2, 1, 5, 6, 1, 3, 5, 0], max_bit=4)
```

#### Insert value (insert)

```
>>> dwm
DynamicWaveletMatrix([5, 4, 5, 5, 2, 1, 5, 6, 1, 3, 5, 0], max_bit=4)
>>> # Inserts 8 at index 4.
>>> # The bit width of the new value must not exceed max_bit.
>>> dwm.insert(index=4, value=8)
>>> dwm
DynamicWaveletMatrix([5, 4, 5, 5, 8, 2, 1, 5, 6, 1, 3, 5, 0], max_bit=4)
```

#### Remove value (remove)

```
>>> dwm
DynamicWaveletMatrix([5, 4, 5, 5, 8, 2, 1, 5, 6, 1, 3, 5, 0], max_bit=4)
>>> # Remove the value at index 4.
>>> dwm.remove(index=4)
8
>>> dwm
DynamicWaveletMatrix([5, 4, 5, 5, 2, 1, 5, 6, 1, 3, 5, 0], max_bit=4)
```

#### Update value (update)

```
>>> dwm
DynamicWaveletMatrix([5, 4, 5, 5, 2, 1, 5, 6, 1, 3, 5, 0], max_bit=4)
>>> # Update the value at index 4 to 5
>>> # The bit width of the new value must not exceed max_bit.
>>> dwm.update(index=4, value=5)
2
>>> dwm
DynamicWaveletMatrix([5, 4, 5, 5, 5, 1, 5, 6, 1, 3, 5, 0], max_bit=4)
```

## Development

### Running Tests

```
$ pip install -e ".[dev]"

# Cargo test
$ cargo test --all --release

# Run tests
$ pytest --benchmark-skip

# Run benchmarks
$ pytest --benchmark-only
```

### Formating Code

```
# Format Rust code
$ cargo fmt

# Format Python code
$ ruff format
```

### Generating Docs

```
$ pdoc wavelet_matrix \
      --output-directory docs \
      --no-search \
      --no-show-source \
      --docformat markdown \
      --footer-text "© 2025 Koki Watanabe"
```

## References

* Francisco Claude, Gonzalo Navarro, Alberto Ordóñez,
  The wavelet matrix: An efficient wavelet tree for large alphabets,
  Information Systems,
  Volume 47,
  2015,
  Pages 15-32,
  ISSN 0306-4379,
  <https://doi.org/10.1016/j.is.2014.06.002>.

## Project details

### Verified details

*These details have been [verified by PyPI](https://docs.pypi.org/project_metadata/#verified-details)*

###### Maintainers

[![Avatar for math-hiyoko from gravatar.com](https://pypi-camo.freetls.fastly.net/439d3d5c89e82d0411eaef9b3aef03340a527cc4/68747470733a2f2f7365637572652e67726176617461722e636f6d2f6176617461722f36346134656638653537396539343937363838653431616530366239323432373f73697a653d3530 "Avatar for math-hiyoko from gravatar.com")
math-hiyoko](/user/math-hiyoko/)

### Unverified details

*These details have **not** been verified by PyPI*

###### Project links

* [documentation](https://math-hiyoko.github.io/wavelet-matrix)
* [homepage](https://github.com/math-hiyoko/wavelet-matrix)
* [repository](https://github.com/math-hiyoko/wavelet-matrix)

###### Meta

* **License:** MIT License
* **Author:** Koki Watanabe
* Tags

  wavelet matrix
  ,

  data structure
  ,

  algorithm
  ,

  sequence index
  ,

  rank
  ,

  select
  ,

  quantile
  ,

  top-k
  ,

  range query
  ,

  dynamic update
  ,

  indexing
  ,

  succinct
* **Requires:** Python >=3.9

###### Classifiers

* **Intended Audience**
  + [Developers](/search/?c=Intended+Audience+%3A%3A+Developers)
  + [Information Technology](/search/?c=Intended+Audience+%3A%3A+Information+Technology)
  + [Science/Research](/search/?c=Intended+Audience+%3A%3A+Science%2FResearch)
* **License**
  + [OSI Approved :: MIT License](/search/?c=License+%3A%3A+OSI+Approved+%3A%3A+MIT+License)
* **Programming Language**
  + [Python :: 3](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3)
  + [Python :: 3.9](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.9)
  + [Python :: 3.10](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.10)
  + [Python :: 3.11](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.11)
  + [Python :: 3.12](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.12)
  + [Python :: 3.13](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.13)
  + [Python :: 3.14](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+3.14)
  + [Python :: Implementation :: CPython](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+Implementation+%3A%3A+CPython)
  + [Python :: Implementation :: PyPy](/search/?c=Programming+Language+%3A%3A+Python+%3A%3A+Implementation+%3A%3A+PyPy)
* **Topic**
  + [Scientific/Engineering :: Information Analysis](/search/?c=Topic+%3A%3A+Scientific%2FEngineering+%3A%3A+Information+Analysis)
  + [Scientific/Engineering :: Mathematics](/search/?c=Topic+%3A%3A+Scientific%2FEngineering+%3A%3A+Mathematics)
  + [Software Development :: Libraries :: Python Modules](/search/?c=Topic+%3A%3A+Software+Development+%3A%3A+Libraries+%3A%3A+Python+Modules)
* **Typing**
  + [Typed](/search/?c=Typing+%3A%3A+Typed)

## Release history [Release notifications](/help/#project-release-notifications) | [RSS feed](/rss/project/wavelet-matrix/releases.xml)

This version

![](https://pypi.org/static/images/blue-cube.572a5bfb.svg)

[2.0.3

Dec 16, 2025](/project/wavelet-matrix/2.0.3/)

![](https://pypi.org/static/images/white-cube.2351a86c.svg)

[2.0.2

Dec 16, 2025](/project/wavelet-matrix/2.0.2/)

![](https://pypi.org/static/images/white-cube.2351a86c.svg)

[2.0.1

Dec 14, 2025](/project/wavelet-matrix/2.0.1/)

![](https://pypi.org/static/images/white-cube.2351a86c.svg)

[2.0.0

Dec 14, 2025](/project/wavelet-matrix/2.0.0/)

![](https://pypi.org/static/images/white-cube.2351a86c.svg)

[1.0.0

Dec 12, 2025](/project/wavelet-matrix/1.0.0/)

![](https://pypi.org/static/images/white-cube.2351a86c.svg)

[0.1.0

Dec 6, 2025](/project/wavelet-matrix/0.1.0/)

## Download files

Download the file for your platform. If you're not sure which to choose, learn more about [installing packages](https://packaging.python.org/tutorials/installing-packages/ "External link").

### Source Distribution

[wavelet\_matrix-2.0.3.tar.gz](https://files.pythonhosted.org/packages/10/08/90441d83c7b372cd4692cf65de07c7ff7c4e85a24b8df66cdd1420ca13f7/wavelet_matrix-2.0.3.tar.gz)
(52.9 kB
[view details](#wavelet_matrix-2.0.3.tar.gz))

Uploaded
Dec 16, 2025
`Source`

### Built Distributions

Filter files by name, interpreter, ABI, and platform.

If you're not sure about the file name format, learn more about [wheel file names](https://packaging.python.org/en/latest/specifications/binary-distribution-format/ "External link").

The dropdown lists show the available interpreters, ABIs, and platforms.

Enable javascript to be able to filter the list of wheel files.

Copy a direct link to the current filters

Copy

File name

Interpreter

Interpreter
cp310
cp311
cp312
cp313
cp314
cp39
pp311

ABI

ABI
cp310
cp311
cp312
cp313
cp313t
cp314
cp314t
cp39
pypy311\_pp73

Platform

Platform
macosx\_10\_12\_x86\_64
macosx\_11\_0\_arm64
manylinux1\_i686
manylinux2014\_aarch64
manylinux2014\_armv7l
manylinux2014\_ppc64le
manylinux2014\_s390x
manylinux2014\_x86\_64
manylinux\_2\_17\_aarch64
manylinux\_2\_17\_armv7l
manylinux\_2\_17\_ppc64le
manylinux\_2\_17\_s390x
manylinux\_2\_17\_x86\_64
manylinux\_2\_5\_i686
musllinux\_1\_2\_aarch64
musllinux\_1\_2\_armv7l
musllinux\_1\_2\_i686
musllinux\_1\_2\_x86\_64
win32
win\_amd64

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/ba/8e/7b85fe4811238a4d1820775a08fe0c45e82d93397208b7f57914943a46ae/wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_x86_64.whl)
(758.7 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`PyPy``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/7b/17/28edda48f8075ce61c8599bbdbb18f6d4a86a87db711c8c90e7b57075daf/wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_i686.whl)
(824.2 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`PyPy``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/05/f6/58140c6f70d885e27fc86b1deb7eb9b83a9a63c3d2dfde30062385e5b719/wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_armv7l.whl)
(851.6 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`PyPy``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/71/82/aad6a107ce3e1302c6ad115ea1e4a6ecdb6374e8096d1a4db30d6d19250d/wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_aarch64.whl)
(700.8 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`PyPy``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/14/e7/8aaa07f5b4eb0965d39f567c0152ac4dc3b4d53c7edf9b724ee020077447/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
(551.4 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl))

Uploaded
Dec 16, 2025
`PyPy``manylinux: glibc 2.17+ x86-64`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/e1/09/a04c92ed00f26ac4f8fed53922373eb90e857f724874358b42d2381439e1/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(583.6 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`PyPy``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/80/d1/5728cb834356fcb96fc48af7450c7c5529ffda7d99268621a4fcc363667d/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(684.2 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`PyPy``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/a0/33/573afd4aa46b038c6c36c671392ae87d22b632f0bc2c43c84f7bd6a1d2b8/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(580.8 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`PyPy``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/a2/ad/bce51ad87fd2f3ec75b96a4253aeeaf0c50580420d467979108183c00328/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(517.8 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`PyPy``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/c8/93/2d6f3673fb79a02a15968eca3f743f6abdd038ab3111459c82c3fcc6b8ea/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_5_i686.manylinux1_i686.whl)
(633.8 kB
[view details](#wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_5_i686.manylinux1_i686.whl))

Uploaded
Dec 16, 2025
`PyPy``manylinux: glibc 2.5+ i686`

[wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/2b/1f/4c6621b2278dd19e9e945b4a14a9d36bb448450b336b718be023a9ebd258/wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_x86_64.whl)
(766.5 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14t``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/d9/de/94e4fcb344af3976bebca721f8fd98247300847dbf83b48535014656a2e0/wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_i686.whl)
(832.6 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.14t``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/28/b9/0a8acdac759f28e99556fbec400b84cc0606ce1dd226dfc112da6ef120cd/wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_armv7l.whl)
(862.0 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.14t``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/b1/bc/352801fb0f473080d367487b5a2fffe43cda2d4719a0d40a56f24d4f892b/wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_aarch64.whl)
(702.9 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14t``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/8a/a4/78c76a8d80fd9c4b6306114b0dff2a81ac616f0d76a22c19b44f6f483166/wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(591.7 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`CPython 3.14t``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/b1/ba/688d94089e3f3c341316175d63f5ecbe113c35ddfbd8c68eb3e1d88ad3f6/wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(687.4 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`CPython 3.14t``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/03/be/35b6d81b63c67095572dd7dd05cfcf478cf2bfa913013ac2c5ba1decc08c/wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(592.6 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.14t``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/12/40/7062a18e2ab741b72dc08211d242425c012889502d0df039dbea0afcccd0/wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(521.6 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14t``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-cp314-cp314-win\_amd64.whl](https://files.pythonhosted.org/packages/bc/3a/7de2ec59bd2dc133e4e6f8c8a7ef7f680cb3178bd15c88ccac8f01fc2a94/wavelet_matrix-2.0.3-cp314-cp314-win_amd64.whl)
(388.8 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-win_amd64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``Windows x86-64`

[wavelet\_matrix-2.0.3-cp314-cp314-win32.whl](https://files.pythonhosted.org/packages/7a/22/f73ef977a7fd0a775d49e1310937ca9c7609c376db0021a56f6a21593cfe/wavelet_matrix-2.0.3-cp314-cp314-win32.whl)
(391.4 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-win32.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``Windows x86`

[wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/db/61/3b94fb423aba36c8736fccc118b32a7490e3f6563f5a3daaec6ec9fbb578/wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_x86_64.whl)
(758.9 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/dd/8a/214431e27e2e6fcd9a906874805fdf2131152b49e1b31a8c18b55cf4f40f/wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_i686.whl)
(824.5 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/2a/47/78ced222a07d9a866cb6d8ff3646d4d26b95e47afdc53d1829ed205c0597/wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_armv7l.whl)
(852.3 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/6d/07/21d9e8ca91554a344b16954f38ce719b48c6f013091bed1e6b96cdcb7752/wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_aarch64.whl)
(700.3 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/4b/d6/03f3a7f85f5e1a2a22c4403a64bd7e0335df086b4603c1d16a55dab0c8da/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
(552.0 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_x86_64.manylinux2014_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``manylinux: glibc 2.17+ x86-64`

[wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/61/25/32435e1cae2c00b30c5c653631c822598f29dd03036c5ddd910fc79b18ce/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(581.9 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/40/b3/fc5bcf4b0508806cb98ccf8d60cea8456055fcdc64bc17387c926cf06f3e/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(681.6 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/8f/fa/4bb6661768ded1fd460054dc73d7efa34d727721e0d9a3d50a0d1f57eefd/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(582.0 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/e5/a7/2bb103e783357e89b469b3dbb658c67d5d594e2537397da8ac8237d9af43/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(516.8 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/b7/7e/bd66a4a498037cf924b7158f7a073d1fbfc80083f22b202713224fc14b3d/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_5_i686.manylinux1_i686.whl)
(635.7 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_5_i686.manylinux1_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``manylinux: glibc 2.5+ i686`

[wavelet\_matrix-2.0.3-cp314-cp314-macosx\_11\_0\_arm64.whl](https://files.pythonhosted.org/packages/9c/11/0b8f64a4171d98c9272e61ba9ca810c43b2de424488c50a0ea45a5d746cd/wavelet_matrix-2.0.3-cp314-cp314-macosx_11_0_arm64.whl)
(500.2 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-macosx_11_0_arm64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``macOS 11.0+ ARM64`

[wavelet\_matrix-2.0.3-cp314-cp314-macosx\_10\_12\_x86\_64.whl](https://files.pythonhosted.org/packages/ef/74/9505f7f6beabfc23cb9e5fadbad1733e12451cf990fe684cd6ab78a6d646/wavelet_matrix-2.0.3-cp314-cp314-macosx_10_12_x86_64.whl)
(532.0 kB
[view details](#wavelet_matrix-2.0.3-cp314-cp314-macosx_10_12_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.14``macOS 10.12+ x86-64`

[wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/f4/cc/ebf708a4cae2a249d11459d2428c2f0a5d4e6d8e35a3beb0dfb1f46bf5d5/wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_x86_64.whl)
(765.9 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13t``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/69/cd/27ee7dcfd15e5e1f7c8f3bdd9cbfdad0725047595ce38ba27946a02e98c2/wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_i686.whl)
(831.3 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.13t``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/ab/51/b4f8cf311740d28d3958d0cb50f753758eeaab4918057fa20a430a33e3db/wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_armv7l.whl)
(860.3 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.13t``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/9c/c6/408d5a75607dc38484ecba87a553a02ed1d408d0329d886c8ec35a3cdc14/wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_aarch64.whl)
(702.8 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13t``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/57/97/55126d32f66f93e5b6504fa8d1386565e4f1f00923a895df19c98bc1b085/wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(591.6 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`CPython 3.13t``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/00/b0/72420d8f3b353818ce12b159d8ab13f0d7d00257e5c05a79b1888346b6f3/wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(686.2 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`CPython 3.13t``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/10/fb/0a3c600bc1b5e660ef49f29f5480a838df76ac867cd809bde8fbec2f4656/wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(590.6 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.13t``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/37/eb/77a57cb353e8be0d167e16df761d56aa8e2f865fc57b571672e662dfc5b8/wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(520.8 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13t``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-cp313-cp313-win\_amd64.whl](https://files.pythonhosted.org/packages/11/e1/9f31a42cec45c4ae9261d90fc5196cf909def43b93fb6711c1597f87443b/wavelet_matrix-2.0.3-cp313-cp313-win_amd64.whl)
(392.1 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-win_amd64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``Windows x86-64`

[wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/50/7a/9825563a17f3dfac9f58681b780c3a7c6547d6ed6db030136d42052cc2fb/wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_x86_64.whl)
(761.1 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/bc/6a/30b454940d0c5f36c2701de708c0659189963277b855032040294f6c1bd6/wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_i686.whl)
(826.3 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/88/c6/30ed20747029ca2b7b182dad9dd293023e7d53b066e1930cd64e2deb2050/wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_armv7l.whl)
(854.9 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/81/c6/90747aad15c36b3570cfbb5b8dc416469ae0fef6931d3fd9c12bebef817f/wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_aarch64.whl)
(700.6 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/a5/4e/efdaa33b8f88d4112cfce6d66386b47355337866dc9dfe28342c5f1f26db/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
(555.3 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``manylinux: glibc 2.17+ x86-64`

[wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/67/c2/3ef6a3a12fe53f637ea095a3c3e83fc2b9c88d9307930d7b69e88fc2d040/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(584.6 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/cb/0b/3cdad4bed21338341dd64e5b41bce970df5018b01d3d315171460a135b11/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(679.9 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/eb/d2/ebe1b4408c83a45c42c4911acdc3f2042ff2475bff017ea6060c68ecfdef/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(585.5 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/72/c1/37b1ed6ebf5de27494e7ef88fe93d7b1d44163f00ee4178ca0394e6a1561/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(517.4 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/78/ae/7fab495e2c69f4da672e3b321e7091247d6d8f8bb6c922a2c7f34cda9ef4/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_5_i686.manylinux1_i686.whl)
(638.0 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_5_i686.manylinux1_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``manylinux: glibc 2.5+ i686`

[wavelet\_matrix-2.0.3-cp313-cp313-macosx\_11\_0\_arm64.whl](https://files.pythonhosted.org/packages/37/c8/242f0c79ed377f61e43c4bb763eff223599db28451cefb6769b3477fca91/wavelet_matrix-2.0.3-cp313-cp313-macosx_11_0_arm64.whl)
(501.6 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-macosx_11_0_arm64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``macOS 11.0+ ARM64`

[wavelet\_matrix-2.0.3-cp313-cp313-macosx\_10\_12\_x86\_64.whl](https://files.pythonhosted.org/packages/42/48/abf02fc482c86aabfedc235f120b7f6327045ee5cd8d3b3ec89ddfb664c8/wavelet_matrix-2.0.3-cp313-cp313-macosx_10_12_x86_64.whl)
(535.4 kB
[view details](#wavelet_matrix-2.0.3-cp313-cp313-macosx_10_12_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.13``macOS 10.12+ x86-64`

[wavelet\_matrix-2.0.3-cp312-cp312-win\_amd64.whl](https://files.pythonhosted.org/packages/e5/dd/90c8a3612d93aa6f3f72997f7018c72890d86831293c25774e30a4ad681e/wavelet_matrix-2.0.3-cp312-cp312-win_amd64.whl)
(391.9 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-win_amd64.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``Windows x86-64`

[wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/fc/69/79851c4e1cbaa83b6b86d9e18997bc36bdefdb5f06a344dcaa695a3e85e1/wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_x86_64.whl)
(760.7 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/1f/30/e058019c3fdf83935d5f2008cd46e9bf13056ca35beddc3c92dde3cb3268/wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_i686.whl)
(826.0 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/53/d3/60e6dd088b245282e82a8c47c0fd6d4a86247cbecfa930a561a9f2832ad6/wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_armv7l.whl)
(855.6 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/ec/98/087773434b7652b0f583309cac739f91a4d12d33d8e9b23dc179f8f1efc8/wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_aarch64.whl)
(700.3 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/7c/c2/80bb3c9670998ed73e562843f6ee5fa54ccf9816f4d19c8518bf476159d8/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
(555.2 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``manylinux: glibc 2.17+ x86-64`

[wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/36/28/643c80cc66ca7ca2ed261c01dcbb10c332797b35d97111cf650227a6b8fc/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(584.4 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/0a/2e/9d36211b515508cdd91a74c2138904c4e4bd4d18a37482e93f885bd18089/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(678.1 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/5a/47/b617620e73276818ffdada8fff3d4133a580a9b51601e5fdb9bd53d81ec4/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(586.7 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/a6/ed/848eeb3521def3fa01e71130305c85a992d0fca430f15105396b1e408cc8/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(517.2 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/6d/08/cdcfe51d87cc53e88cf6744074b3a646e0f31aab01dda11c4bb103e72e46/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.whl)
(638.1 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``manylinux: glibc 2.5+ i686`

[wavelet\_matrix-2.0.3-cp312-cp312-macosx\_11\_0\_arm64.whl](https://files.pythonhosted.org/packages/32/37/a8fe1064a5d0601118afe3df5385e7331d85fa466f4595505e405da52958/wavelet_matrix-2.0.3-cp312-cp312-macosx_11_0_arm64.whl)
(501.5 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-macosx_11_0_arm64.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``macOS 11.0+ ARM64`

[wavelet\_matrix-2.0.3-cp312-cp312-macosx\_10\_12\_x86\_64.whl](https://files.pythonhosted.org/packages/82/31/d61e391b567237c7337a1311b3997797281c01dbfdc18e35f9942e46eb9b/wavelet_matrix-2.0.3-cp312-cp312-macosx_10_12_x86_64.whl)
(535.1 kB
[view details](#wavelet_matrix-2.0.3-cp312-cp312-macosx_10_12_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.12``macOS 10.12+ x86-64`

[wavelet\_matrix-2.0.3-cp311-cp311-win\_amd64.whl](https://files.pythonhosted.org/packages/e4/bf/938887971a928a61524d0b9e6d7b8621de98f4e79a4079b1479e22a902b6/wavelet_matrix-2.0.3-cp311-cp311-win_amd64.whl)
(389.9 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-win_amd64.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``Windows x86-64`

[wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/dc/a6/24d4d9e194d9c615cb1a9be6ca502795e918c75b1aef2e880c5bffc873e1/wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_x86_64.whl)
(759.5 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/38/67/3779e52a65e029ea46cbd8735c7604b26e8838a85dc6d3fb3181b5ab085f/wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_i686.whl)
(825.5 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/fa/6d/8d43f7b59eff75f9262122ee405a2fc128fe443edeb023eb5dfa5acb0110/wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_armv7l.whl)
(852.1 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/13/18/032f03134a27a43294719c24a5439aa1b2261d1dd43d745dcdd10851da19/wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_aarch64.whl)
(701.0 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/9a/00/365c574a53e3ebcca7a952f8bd52c8470e2a2743442d48b519bd947eab4d/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
(553.2 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``manylinux: glibc 2.17+ x86-64`

[wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/f2/11/190086b3ea16618e9240b5372482d0a9db901d5f39b7251b25e939d4446d/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(583.1 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/4b/7b/25671096db37e0af815746d28fd3ae95fa95aed9c7b085bb6c97a328b92d/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(686.4 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/7f/be/0ee051c75b8b225316918320e5cbb5f19b77009b694dddcd9f0b6ad13393/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(581.4 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/a9/d2/f5b1094d1d1e3fa70b773e7b334693dbb77cc88e81dc6bd0810bb6a34c84/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(517.9 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/42/cf/d515fc393ab4f570e0aa15f52216efb6d41097c93ea96de266794e7eadf4/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.whl)
(635.4 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``manylinux: glibc 2.5+ i686`

[wavelet\_matrix-2.0.3-cp311-cp311-macosx\_11\_0\_arm64.whl](https://files.pythonhosted.org/packages/86/8d/1381cd46cc240775f58c717ed9481d5b2845f942824e4e7f71b317dc3be0/wavelet_matrix-2.0.3-cp311-cp311-macosx_11_0_arm64.whl)
(498.4 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-macosx_11_0_arm64.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``macOS 11.0+ ARM64`

[wavelet\_matrix-2.0.3-cp311-cp311-macosx\_10\_12\_x86\_64.whl](https://files.pythonhosted.org/packages/01/b6/4dab9a5060dc936c34c39e4ea3a8915b627f211ef9fa70fd589c917810bb/wavelet_matrix-2.0.3-cp311-cp311-macosx_10_12_x86_64.whl)
(534.6 kB
[view details](#wavelet_matrix-2.0.3-cp311-cp311-macosx_10_12_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.11``macOS 10.12+ x86-64`

[wavelet\_matrix-2.0.3-cp310-cp310-win\_amd64.whl](https://files.pythonhosted.org/packages/53/31/c4b9a1ae11046af45e2cffc68ce642a3333bb55dc0618ca1a037ba44d140/wavelet_matrix-2.0.3-cp310-cp310-win_amd64.whl)
(389.8 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-win_amd64.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``Windows x86-64`

[wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/1d/aa/d409cb71fa4ccb3f63264657b9dc2a5d56aa0286403de0e9e845c8893913/wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_x86_64.whl)
(759.5 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/79/7b/c5a0330466263be3dfd2b985b973b9ac85d8e567d55c5074eb573b83408d/wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_i686.whl)
(825.6 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/78/29/5738b1a0665c72078aaaa1b837163671a41588edb7441be12691f47b9040/wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_armv7l.whl)
(851.5 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/53/15/42ab03f3309ffbf4740b6970deab952f960c0aece9b54c57d4c1475fa590/wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_aarch64.whl)
(700.7 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/1d/1e/29d8e283ea79a3d881a68367f4e0f25603589bad3afd7cfdc9b782439ca2/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
(553.3 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``manylinux: glibc 2.17+ x86-64`

[wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/85/8d/1d976332785e9632ea123a4499b1d291976fd54e2a1e73601fc52f451630/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(583.6 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/3d/65/e144fe146bf22f6bf16ee2483a8496361d173088cc000a7035c4bb66c7d2/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(686.2 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/2e/74/85aeb0384983970ff7ca3acb6307db830369d5aac0b5389ca8e2909d83d4/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(580.8 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/78/84/9f435f181f1fdb00279840273a1900c38c02ff8190a34bf34fa6868fdd99/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(517.1 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/18/a7/221ba43f28619a43666979390290250aff96fd01059f6eef96d9a0f8208d/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_5_i686.manylinux1_i686.whl)
(635.6 kB
[view details](#wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_5_i686.manylinux1_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.10``manylinux: glibc 2.5+ i686`

[wavelet\_matrix-2.0.3-cp39-cp39-win\_amd64.whl](https://files.pythonhosted.org/packages/fb/61/92a366b847ca026dce75601dff887a1915a8b4ad6ae50d97151c33c321a3/wavelet_matrix-2.0.3-cp39-cp39-win_amd64.whl)
(391.8 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-win_amd64.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``Windows x86-64`

[wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/5b/45/68682d96f62da999b980339da512898edde1e1837aa28ffce076b1184b98/wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_x86_64.whl)
(761.3 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``musllinux: musl 1.2+ x86-64`

[wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/30/4a/26bd11d3757fed94e6e57d31dd308ca47c39abbc3a02a3badb056d4f1c40/wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_i686.whl)
(826.6 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``musllinux: musl 1.2+ i686`

[wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/fa/14/409c5941493cb500e00fb577f74208002486479d1a52549a27b8e087ee1e/wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_armv7l.whl)
(852.9 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``musllinux: musl 1.2+ ARMv7l`

[wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/a9/6d/2202a699f3731b314374051e23314da9e7b7794d98c7c21bb5adc49dfa97/wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_aarch64.whl)
(702.0 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``musllinux: musl 1.2+ ARM64`

[wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/09/a2/7f6c05c1ac95c6f56011bec510232c63379ed397f5108a93106b89157e31/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
(556.0 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``manylinux: glibc 2.17+ x86-64`

[wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/33/da/442c38dffc93cb77696559eaee13ffc39cebfdcb25cdc5575b5b0543dcfe/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_s390x.manylinux2014_s390x.whl)
(586.6 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_s390x.manylinux2014_s390x.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``manylinux: glibc 2.17+ s390x`

[wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/ed/90/0b38b11809f4c6c543a1d079c354019b8b7ea96063e2afb591749ca599b5/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
(688.2 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``manylinux: glibc 2.17+ ppc64le`

[wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/6d/8a/5c519186a00c66f12dca2bdc0b5ff3a565c49fcc581ec695d648eb1e74d7/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
(582.3 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_armv7l.manylinux2014_armv7l.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``manylinux: glibc 2.17+ ARMv7l`

[wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/e5/97/f8c35656810d83d5e457c62714bb717fd4e325fef9c253c848f02fd2f7ed/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
(519.8 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``manylinux: glibc 2.17+ ARM64`

[wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/a3/72/513f95a7ea7593f5374c03b92577cd3b2096907163d5aad01a68ffc27f1a/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_5_i686.manylinux1_i686.whl)
(636.9 kB
[view details](#wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_5_i686.manylinux1_i686.whl))

Uploaded
Dec 16, 2025
`CPython 3.9``manylinux: glibc 2.5+ i686`

## File details

Details for the file `wavelet_matrix-2.0.3.tar.gz`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3.tar.gz](https://files.pythonhosted.org/packages/10/08/90441d83c7b372cd4692cf65de07c7ff7c4e85a24b8df66cdd1420ca13f7/wavelet_matrix-2.0.3.tar.gz)
* Upload date:
  Dec 16, 2025
* Size: 52.9 kB
* Tags: Source
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3.tar.gz

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `646dc2d9ce484f73f43b342c7b5481a7c110fea976ef13d353e7ae7c6586bb69` | Copy |
| MD5 | `2babe91262c3bdc8965a8f38d42cc451` | Copy |
| BLAKE2b-256 | `100890441d83c7b372cd4692cf65de07c7ff7c4e85a24b8df66cdd1420ca13f7` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/ba/8e/7b85fe4811238a4d1820775a08fe0c45e82d93397208b7f57914943a46ae/wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 758.7 kB
* Tags: PyPy, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `60c0ef41361434449386d877fa458cc17d2b6b6a28cd51607b6def4a7d326a90` | Copy |
| MD5 | `638ec0ad28159f1e6435bdd7d2dfb7a0` | Copy |
| BLAKE2b-256 | `ba8e7b85fe4811238a4d1820775a08fe0c45e82d93397208b7f57914943a46ae` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/7b/17/28edda48f8075ce61c8599bbdbb18f6d4a86a87db711c8c90e7b57075daf/wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 824.2 kB
* Tags: PyPy, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `5cc3004faf9209a2147019cf1dcec3cf2d11e6a75df6998872fcaf6eba55fb51` | Copy |
| MD5 | `7883bdd6471bb811bfa72ae36ffc7860` | Copy |
| BLAKE2b-256 | `7b1728edda48f8075ce61c8599bbdbb18f6d4a86a87db711c8c90e7b57075daf` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/05/f6/58140c6f70d885e27fc86b1deb7eb9b83a9a63c3d2dfde30062385e5b719/wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 851.6 kB
* Tags: PyPy, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `876f6d572338ac824df08422c276ad79fc610495216cc75ce1f57f46a4d78746` | Copy |
| MD5 | `9a882745cbfd11afdd56b3dc33137006` | Copy |
| BLAKE2b-256 | `05f658140c6f70d885e27fc86b1deb7eb9b83a9a63c3d2dfde30062385e5b719` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/71/82/aad6a107ce3e1302c6ad115ea1e4a6ecdb6374e8096d1a4db30d6d19250d/wavelet_matrix-2.0.3-pp311-pypy311_pp73-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 700.8 kB
* Tags: PyPy, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `10ba075507424c411616701559ae65c314d676698379d341a522e1ddfd2bdfeb` | Copy |
| MD5 | `6385446e47c97c9112d2c125cbdd573e` | Copy |
| BLAKE2b-256 | `7182aad6a107ce3e1302c6ad115ea1e4a6ecdb6374e8096d1a4db30d6d19250d` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/14/e7/8aaa07f5b4eb0965d39f567c0152ac4dc3b4d53c7edf9b724ee020077447/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 551.4 kB
* Tags: PyPy, manylinux: glibc 2.17+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `421560b1efe20f921edc13e2db9350a4465e58b95da5039aacee153ad1785f34` | Copy |
| MD5 | `ec1e2558bd01256a9704a7aad536bacc` | Copy |
| BLAKE2b-256 | `14e78aaa07f5b4eb0965d39f567c0152ac4dc3b4d53c7edf9b724ee020077447` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/e1/09/a04c92ed00f26ac4f8fed53922373eb90e857f724874358b42d2381439e1/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 583.6 kB
* Tags: PyPy, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f3d4cf7e6f0b0439a5126a5c3f0b5326fa72596b2602f81086f6d57c4bb74e21` | Copy |
| MD5 | `4865612793e8e88944255eddbf94971a` | Copy |
| BLAKE2b-256 | `e109a04c92ed00f26ac4f8fed53922373eb90e857f724874358b42d2381439e1` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/80/d1/5728cb834356fcb96fc48af7450c7c5529ffda7d99268621a4fcc363667d/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 684.2 kB
* Tags: PyPy, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `0867cb6173a42bb329b96dbe3c9b563eb0fa730ae28d617bae649191b893a656` | Copy |
| MD5 | `b55a302c0d6a205e71cfe96145bf2b37` | Copy |
| BLAKE2b-256 | `80d15728cb834356fcb96fc48af7450c7c5529ffda7d99268621a4fcc363667d` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/a0/33/573afd4aa46b038c6c36c671392ae87d22b632f0bc2c43c84f7bd6a1d2b8/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 580.8 kB
* Tags: PyPy, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `94a305ba410760921094a1bf2bf03b6cb68c544a93c30d8469b926b5150e20d6` | Copy |
| MD5 | `e49a89df90175569e83020dd9aaf1c2e` | Copy |
| BLAKE2b-256 | `a033573afd4aa46b038c6c36c671392ae87d22b632f0bc2c43c84f7bd6a1d2b8` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/a2/ad/bce51ad87fd2f3ec75b96a4253aeeaf0c50580420d467979108183c00328/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 517.8 kB
* Tags: PyPy, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `2b366d1e4c4909da0f0ad1a54d7e9749034477c78808af8a05314197680e881e` | Copy |
| MD5 | `8a8475696ce821faaafd4903b7c6ce1f` | Copy |
| BLAKE2b-256 | `a2adbce51ad87fd2f3ec75b96a4253aeeaf0c50580420d467979108183c00328` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_5_i686.manylinux1_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/c8/93/2d6f3673fb79a02a15968eca3f743f6abdd038ab3111459c82c3fcc6b8ea/wavelet_matrix-2.0.3-pp311-pypy311_pp73-manylinux_2_5_i686.manylinux1_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 633.8 kB
* Tags: PyPy, manylinux: glibc 2.5+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-pp311-pypy311\_pp73-manylinux\_2\_5\_i686.manylinux1\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `144178bd46b2fadaaaafca603b5c155dabca66a747d4b487ee15fb501ae15fd4` | Copy |
| MD5 | `69e41070d2e9cfeb80e5371da6661dc7` | Copy |
| BLAKE2b-256 | `c8932d6f3673fb79a02a15968eca3f743f6abdd038ab3111459c82c3fcc6b8ea` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/2b/1f/4c6621b2278dd19e9e945b4a14a9d36bb448450b336b718be023a9ebd258/wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 766.5 kB
* Tags: CPython 3.14t, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `9a3539418155aea6505132935749f7af757c2044d7980a23e36d84d5c5161415` | Copy |
| MD5 | `c62c5e359bdef2317fadab4eac54d38e` | Copy |
| BLAKE2b-256 | `2b1f4c6621b2278dd19e9e945b4a14a9d36bb448450b336b718be023a9ebd258` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/d9/de/94e4fcb344af3976bebca721f8fd98247300847dbf83b48535014656a2e0/wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 832.6 kB
* Tags: CPython 3.14t, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `fdf0871ca733b2b9117ea2b1566f72017e6817ea0a3d7d701aee32bb0545d4d4` | Copy |
| MD5 | `63fbc0e286d2b97e6b7d9b85e80d9894` | Copy |
| BLAKE2b-256 | `d9de94e4fcb344af3976bebca721f8fd98247300847dbf83b48535014656a2e0` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/28/b9/0a8acdac759f28e99556fbec400b84cc0606ce1dd226dfc112da6ef120cd/wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 862.0 kB
* Tags: CPython 3.14t, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `b16d88c04e1beaba9b3bea705a7cf293d3fd327eb538f13c95446b7a41ee5232` | Copy |
| MD5 | `e48795237afeb207c399ff425526ce9e` | Copy |
| BLAKE2b-256 | `28b90a8acdac759f28e99556fbec400b84cc0606ce1dd226dfc112da6ef120cd` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/b1/bc/352801fb0f473080d367487b5a2fffe43cda2d4719a0d40a56f24d4f892b/wavelet_matrix-2.0.3-cp314-cp314t-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 702.9 kB
* Tags: CPython 3.14t, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314t-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `28b06bed585dd7d0a243c8e113c0990c16171f06af10cea6a7d7b4b10a8a1749` | Copy |
| MD5 | `5a09b91e1973d82c5c799a8deea03d8c` | Copy |
| BLAKE2b-256 | `b1bc352801fb0f473080d367487b5a2fffe43cda2d4719a0d40a56f24d4f892b` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/8a/a4/78c76a8d80fd9c4b6306114b0dff2a81ac616f0d76a22c19b44f6f483166/wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 591.7 kB
* Tags: CPython 3.14t, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f947a887122e49d873a33a97c67fdfad01bb091cc1dd0586a569eeb0a8d13372` | Copy |
| MD5 | `4dea57927590996446594d79c0397cfc` | Copy |
| BLAKE2b-256 | `8aa478c76a8d80fd9c4b6306114b0dff2a81ac616f0d76a22c19b44f6f483166` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/b1/ba/688d94089e3f3c341316175d63f5ecbe113c35ddfbd8c68eb3e1d88ad3f6/wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 687.4 kB
* Tags: CPython 3.14t, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `d1ff955ffa4bc955d3b8978898706cd0a253ed9a5ece712d52ffc5394486ac51` | Copy |
| MD5 | `f6cc7b2c8042b26bcc600e5989809418` | Copy |
| BLAKE2b-256 | `b1ba688d94089e3f3c341316175d63f5ecbe113c35ddfbd8c68eb3e1d88ad3f6` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/03/be/35b6d81b63c67095572dd7dd05cfcf478cf2bfa913013ac2c5ba1decc08c/wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 592.6 kB
* Tags: CPython 3.14t, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `3c6070788c934b6919f56861b69ba89485907259411a9be27be56ffd3a4152ad` | Copy |
| MD5 | `dd6d16412c2cf0e38ea593fc389cd114` | Copy |
| BLAKE2b-256 | `03be35b6d81b63c67095572dd7dd05cfcf478cf2bfa913013ac2c5ba1decc08c` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/12/40/7062a18e2ab741b72dc08211d242425c012889502d0df039dbea0afcccd0/wavelet_matrix-2.0.3-cp314-cp314t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 521.6 kB
* Tags: CPython 3.14t, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314t-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `31bb011015258666bdba94eb5648b24516d6a1560e4b784844434154a4b33c0a` | Copy |
| MD5 | `068228a7c966cfd10f66bda934ee8738` | Copy |
| BLAKE2b-256 | `12407062a18e2ab741b72dc08211d242425c012889502d0df039dbea0afcccd0` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-win_amd64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-win\_amd64.whl](https://files.pythonhosted.org/packages/bc/3a/7de2ec59bd2dc133e4e6f8c8a7ef7f680cb3178bd15c88ccac8f01fc2a94/wavelet_matrix-2.0.3-cp314-cp314-win_amd64.whl)
* Upload date:
  Dec 16, 2025
* Size: 388.8 kB
* Tags: CPython 3.14, Windows x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-win\_amd64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `dcbf52a61aae11abf566a31177c75777c84b1834554030940c51be2d31c921de` | Copy |
| MD5 | `cefafe0614638e698cd8568d64553874` | Copy |
| BLAKE2b-256 | `bc3a7de2ec59bd2dc133e4e6f8c8a7ef7f680cb3178bd15c88ccac8f01fc2a94` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-win32.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-win32.whl](https://files.pythonhosted.org/packages/7a/22/f73ef977a7fd0a775d49e1310937ca9c7609c376db0021a56f6a21593cfe/wavelet_matrix-2.0.3-cp314-cp314-win32.whl)
* Upload date:
  Dec 16, 2025
* Size: 391.4 kB
* Tags: CPython 3.14, Windows x86
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-win32.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `9c1617eb64b0dbc59926a08bab77d0b3324ad43cd77b163ff3fc5aa759748f37` | Copy |
| MD5 | `78fca03319f6e168e4eecfee757a05e4` | Copy |
| BLAKE2b-256 | `7a22f73ef977a7fd0a775d49e1310937ca9c7609c376db0021a56f6a21593cfe` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/db/61/3b94fb423aba36c8736fccc118b32a7490e3f6563f5a3daaec6ec9fbb578/wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 758.9 kB
* Tags: CPython 3.14, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `a371b9c8b9ad9afbc0851e04d3c5299622cf1e82b2275ad6ddbb37b92360809d` | Copy |
| MD5 | `c2a8fe5dec6fa5229a30f67d41e8b278` | Copy |
| BLAKE2b-256 | `db613b94fb423aba36c8736fccc118b32a7490e3f6563f5a3daaec6ec9fbb578` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/dd/8a/214431e27e2e6fcd9a906874805fdf2131152b49e1b31a8c18b55cf4f40f/wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 824.5 kB
* Tags: CPython 3.14, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `fa9fd971aac0995d8f0ddafbf50903e224516d86a72baeae56d146a3807fd3ac` | Copy |
| MD5 | `42eb14c472df457d5d927d3f0fa59b2b` | Copy |
| BLAKE2b-256 | `dd8a214431e27e2e6fcd9a906874805fdf2131152b49e1b31a8c18b55cf4f40f` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/2a/47/78ced222a07d9a866cb6d8ff3646d4d26b95e47afdc53d1829ed205c0597/wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 852.3 kB
* Tags: CPython 3.14, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `a568c5901cc151f4d43c5c577b2e75ae141ef5f4e544608e7a50a7fc40f558aa` | Copy |
| MD5 | `206b89b3a3ec4182b8f8e82d9fc04f0b` | Copy |
| BLAKE2b-256 | `2a4778ced222a07d9a866cb6d8ff3646d4d26b95e47afdc53d1829ed205c0597` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/6d/07/21d9e8ca91554a344b16954f38ce719b48c6f013091bed1e6b96cdcb7752/wavelet_matrix-2.0.3-cp314-cp314-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 700.3 kB
* Tags: CPython 3.14, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `ffc6d0b5ec31b4ec117c36654b3bda6685e799d55a2d31bea99f928a3fc02f5e` | Copy |
| MD5 | `eaf8472d61241502842385aa34f92ca5` | Copy |
| BLAKE2b-256 | `6d0721d9e8ca91554a344b16954f38ce719b48c6f013091bed1e6b96cdcb7752` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/4b/d6/03f3a7f85f5e1a2a22c4403a64bd7e0335df086b4603c1d16a55dab0c8da/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 552.0 kB
* Tags: CPython 3.14, manylinux: glibc 2.17+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `106324469d121b2856bff819a812133f038f1bf692001f0c23657d538c2a4743` | Copy |
| MD5 | `38a785bbee6dc42d696886afd73a20e0` | Copy |
| BLAKE2b-256 | `4bd603f3a7f85f5e1a2a22c4403a64bd7e0335df086b4603c1d16a55dab0c8da` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/61/25/32435e1cae2c00b30c5c653631c822598f29dd03036c5ddd910fc79b18ce/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 581.9 kB
* Tags: CPython 3.14, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `e8ee81d37a1e5f9dbd04b3458bcb149cb5da32f8765e44f1dd2034b6c90a9a5f` | Copy |
| MD5 | `6b67e07f06588b5492472488dd0f035b` | Copy |
| BLAKE2b-256 | `612532435e1cae2c00b30c5c653631c822598f29dd03036c5ddd910fc79b18ce` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/40/b3/fc5bcf4b0508806cb98ccf8d60cea8456055fcdc64bc17387c926cf06f3e/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 681.6 kB
* Tags: CPython 3.14, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `ab9cdf66f3d4ee0541780068707da247e77dbe724453d926457257fe3f26b5cf` | Copy |
| MD5 | `cda18337bbdf063a0ecd17dd79542bff` | Copy |
| BLAKE2b-256 | `40b3fc5bcf4b0508806cb98ccf8d60cea8456055fcdc64bc17387c926cf06f3e` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/8f/fa/4bb6661768ded1fd460054dc73d7efa34d727721e0d9a3d50a0d1f57eefd/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 582.0 kB
* Tags: CPython 3.14, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `b19561536390bb2b8d7d91fb347b7ab6ae99745c685565da498c2dd3cce45c9f` | Copy |
| MD5 | `d161f19e37a1981bd4445392688c056d` | Copy |
| BLAKE2b-256 | `8ffa4bb6661768ded1fd460054dc73d7efa34d727721e0d9a3d50a0d1f57eefd` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/e5/a7/2bb103e783357e89b469b3dbb658c67d5d594e2537397da8ac8237d9af43/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 516.8 kB
* Tags: CPython 3.14, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `57528ac0091fed82cc00e150e9a0094db72ff6d2f99874dbcf094fddf80cf1b4` | Copy |
| MD5 | `d51451bc6fc09078fd34f5e4dd7e0a0a` | Copy |
| BLAKE2b-256 | `e5a72bb103e783357e89b469b3dbb658c67d5d594e2537397da8ac8237d9af43` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_5_i686.manylinux1_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/b7/7e/bd66a4a498037cf924b7158f7a073d1fbfc80083f22b202713224fc14b3d/wavelet_matrix-2.0.3-cp314-cp314-manylinux_2_5_i686.manylinux1_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 635.7 kB
* Tags: CPython 3.14, manylinux: glibc 2.5+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-manylinux\_2\_5\_i686.manylinux1\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f3851c4d5b14373dc4d808a39c6da46c7037b6c80e65c7ea4ff93cf7f517ed33` | Copy |
| MD5 | `7bb291b9bda72784ac6a382d64cc8d23` | Copy |
| BLAKE2b-256 | `b77ebd66a4a498037cf924b7158f7a073d1fbfc80083f22b202713224fc14b3d` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-macosx_11_0_arm64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-macosx\_11\_0\_arm64.whl](https://files.pythonhosted.org/packages/9c/11/0b8f64a4171d98c9272e61ba9ca810c43b2de424488c50a0ea45a5d746cd/wavelet_matrix-2.0.3-cp314-cp314-macosx_11_0_arm64.whl)
* Upload date:
  Dec 16, 2025
* Size: 500.2 kB
* Tags: CPython 3.14, macOS 11.0+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-macosx\_11\_0\_arm64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f362148702a6cf88e5add1ed1b60dda2f30f925dfff467410a01488fb16ec40d` | Copy |
| MD5 | `46f04c43915b90c9adac4b85463e5e68` | Copy |
| BLAKE2b-256 | `9c110b8f64a4171d98c9272e61ba9ca810c43b2de424488c50a0ea45a5d746cd` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp314-cp314-macosx_10_12_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp314-cp314-macosx\_10\_12\_x86\_64.whl](https://files.pythonhosted.org/packages/ef/74/9505f7f6beabfc23cb9e5fadbad1733e12451cf990fe684cd6ab78a6d646/wavelet_matrix-2.0.3-cp314-cp314-macosx_10_12_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 532.0 kB
* Tags: CPython 3.14, macOS 10.12+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp314-cp314-macosx\_10\_12\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `89f42f88f81fb0efcc0029ed096c032b2c2b8f2aada2d058ae64dcc56b746ee5` | Copy |
| MD5 | `5dd0603a7cfeda4a047fb78d5f217612` | Copy |
| BLAKE2b-256 | `ef749505f7f6beabfc23cb9e5fadbad1733e12451cf990fe684cd6ab78a6d646` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/f4/cc/ebf708a4cae2a249d11459d2428c2f0a5d4e6d8e35a3beb0dfb1f46bf5d5/wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 765.9 kB
* Tags: CPython 3.13t, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `ecb1160cb3a29115342e8b2fb39a5ea6f7add23eb7b7f0a344a813c0a7fd88fd` | Copy |
| MD5 | `451537b17e79ad89a87fbe8d0c73494c` | Copy |
| BLAKE2b-256 | `f4ccebf708a4cae2a249d11459d2428c2f0a5d4e6d8e35a3beb0dfb1f46bf5d5` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/69/cd/27ee7dcfd15e5e1f7c8f3bdd9cbfdad0725047595ce38ba27946a02e98c2/wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 831.3 kB
* Tags: CPython 3.13t, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `7f95e172d23f79421619ff14cb8c0e636c9366b5712cb1518128db86a7dc195e` | Copy |
| MD5 | `65108136cbff90cd3eacff8e3c8369e4` | Copy |
| BLAKE2b-256 | `69cd27ee7dcfd15e5e1f7c8f3bdd9cbfdad0725047595ce38ba27946a02e98c2` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/ab/51/b4f8cf311740d28d3958d0cb50f753758eeaab4918057fa20a430a33e3db/wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 860.3 kB
* Tags: CPython 3.13t, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `80c0405d65bdfe77f2586f88eb3fcbafeb7eef77ada519d9039aac02671e5d9f` | Copy |
| MD5 | `f05365b5c53b02c5c87e9cf3e1bde0f3` | Copy |
| BLAKE2b-256 | `ab51b4f8cf311740d28d3958d0cb50f753758eeaab4918057fa20a430a33e3db` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/9c/c6/408d5a75607dc38484ecba87a553a02ed1d408d0329d886c8ec35a3cdc14/wavelet_matrix-2.0.3-cp313-cp313t-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 702.8 kB
* Tags: CPython 3.13t, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313t-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `aab1c4f1555d10a76f9443572c9c00a3a4ffe1841fe48b19bf70c23baaca87ac` | Copy |
| MD5 | `185271512695602d03aff4455051b4a8` | Copy |
| BLAKE2b-256 | `9cc6408d5a75607dc38484ecba87a553a02ed1d408d0329d886c8ec35a3cdc14` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/57/97/55126d32f66f93e5b6504fa8d1386565e4f1f00923a895df19c98bc1b085/wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 591.6 kB
* Tags: CPython 3.13t, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `338591e7f81584e26a6360fe939df8bec2769df6c458f0e86b7ea6188c601e06` | Copy |
| MD5 | `bccbeb56cf2f2893c6e3a825dbebd9de` | Copy |
| BLAKE2b-256 | `579755126d32f66f93e5b6504fa8d1386565e4f1f00923a895df19c98bc1b085` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/00/b0/72420d8f3b353818ce12b159d8ab13f0d7d00257e5c05a79b1888346b6f3/wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 686.2 kB
* Tags: CPython 3.13t, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `8ae153e2c7cf00efc85279d99952ef97214648433ed5e29898ed358990c9c22c` | Copy |
| MD5 | `088608cb264ba695f4ae522ebd225988` | Copy |
| BLAKE2b-256 | `00b072420d8f3b353818ce12b159d8ab13f0d7d00257e5c05a79b1888346b6f3` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/10/fb/0a3c600bc1b5e660ef49f29f5480a838df76ac867cd809bde8fbec2f4656/wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 590.6 kB
* Tags: CPython 3.13t, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `a81b3926812ef472527dc567040423f3bbc31f541f5719ab489e62d772b6e62b` | Copy |
| MD5 | `c447acbc30bb14134bbe1d3b907de96e` | Copy |
| BLAKE2b-256 | `10fb0a3c600bc1b5e660ef49f29f5480a838df76ac867cd809bde8fbec2f4656` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/37/eb/77a57cb353e8be0d167e16df761d56aa8e2f865fc57b571672e662dfc5b8/wavelet_matrix-2.0.3-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 520.8 kB
* Tags: CPython 3.13t, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313t-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `b127498045f337527f6e0f1f8bfddd237d90ee890782401b63352c880f1c1c34` | Copy |
| MD5 | `c0b0fb8a32ac5951886a1d4dd595b1a2` | Copy |
| BLAKE2b-256 | `37eb77a57cb353e8be0d167e16df761d56aa8e2f865fc57b571672e662dfc5b8` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-win_amd64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-win\_amd64.whl](https://files.pythonhosted.org/packages/11/e1/9f31a42cec45c4ae9261d90fc5196cf909def43b93fb6711c1597f87443b/wavelet_matrix-2.0.3-cp313-cp313-win_amd64.whl)
* Upload date:
  Dec 16, 2025
* Size: 392.1 kB
* Tags: CPython 3.13, Windows x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-win\_amd64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `8cc971a55cb712d3073b131a82a75c40dff71ef09545074dbfd2d2cbadbbbe00` | Copy |
| MD5 | `d4bd986ff2052dd87c2a9933185994c0` | Copy |
| BLAKE2b-256 | `11e19f31a42cec45c4ae9261d90fc5196cf909def43b93fb6711c1597f87443b` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/50/7a/9825563a17f3dfac9f58681b780c3a7c6547d6ed6db030136d42052cc2fb/wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 761.1 kB
* Tags: CPython 3.13, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `cc97330c2940a7567556933d013bf5c724be28cd2b06702babc43735cdb54540` | Copy |
| MD5 | `f27f2ec8665826f536bd4d6c229ce6c1` | Copy |
| BLAKE2b-256 | `507a9825563a17f3dfac9f58681b780c3a7c6547d6ed6db030136d42052cc2fb` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/bc/6a/30b454940d0c5f36c2701de708c0659189963277b855032040294f6c1bd6/wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 826.3 kB
* Tags: CPython 3.13, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `cf0cd8996aa61458bdf8811efb30e05ab257bf109b35a9019a1fd4f8aefd39b2` | Copy |
| MD5 | `f01c20ef1bf0598aec81a3b2f120c8f5` | Copy |
| BLAKE2b-256 | `bc6a30b454940d0c5f36c2701de708c0659189963277b855032040294f6c1bd6` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/88/c6/30ed20747029ca2b7b182dad9dd293023e7d53b066e1930cd64e2deb2050/wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 854.9 kB
* Tags: CPython 3.13, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `0036b04e3c2d2a94423d47f3678422c08249f92b02871d7ed6d71d80e292ae59` | Copy |
| MD5 | `78c6decf8e55cc98134764c21329f2dc` | Copy |
| BLAKE2b-256 | `88c630ed20747029ca2b7b182dad9dd293023e7d53b066e1930cd64e2deb2050` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/81/c6/90747aad15c36b3570cfbb5b8dc416469ae0fef6931d3fd9c12bebef817f/wavelet_matrix-2.0.3-cp313-cp313-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 700.6 kB
* Tags: CPython 3.13, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `7824086d79b79cfeadc9c314252b927844a40c08385f0d63b1473f041f460ade` | Copy |
| MD5 | `154f13f7021b5ab0b5dc64342c64cf7c` | Copy |
| BLAKE2b-256 | `81c690747aad15c36b3570cfbb5b8dc416469ae0fef6931d3fd9c12bebef817f` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/a5/4e/efdaa33b8f88d4112cfce6d66386b47355337866dc9dfe28342c5f1f26db/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 555.3 kB
* Tags: CPython 3.13, manylinux: glibc 2.17+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `25947d6f5e3b8ec887a667ce56578e832495943b50af3214c4388eb64d567b2a` | Copy |
| MD5 | `cab91f318016edf016251e4893edf978` | Copy |
| BLAKE2b-256 | `a54eefdaa33b8f88d4112cfce6d66386b47355337866dc9dfe28342c5f1f26db` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/67/c2/3ef6a3a12fe53f637ea095a3c3e83fc2b9c88d9307930d7b69e88fc2d040/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 584.6 kB
* Tags: CPython 3.13, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `b10b5c657ba29e279c332fb9b8281a41391a2ca704df3ff6eb2bd875e1b2312e` | Copy |
| MD5 | `9458ccadbbeb0b15a18afbb1895013cc` | Copy |
| BLAKE2b-256 | `67c23ef6a3a12fe53f637ea095a3c3e83fc2b9c88d9307930d7b69e88fc2d040` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/cb/0b/3cdad4bed21338341dd64e5b41bce970df5018b01d3d315171460a135b11/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 679.9 kB
* Tags: CPython 3.13, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `c73e13ff4cc5064e5d36f833b641927d799c5795c10a4f3bf51962524f700ca3` | Copy |
| MD5 | `bacf50a3f4543f4a4b3747cb2b6dfb53` | Copy |
| BLAKE2b-256 | `cb0b3cdad4bed21338341dd64e5b41bce970df5018b01d3d315171460a135b11` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/eb/d2/ebe1b4408c83a45c42c4911acdc3f2042ff2475bff017ea6060c68ecfdef/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 585.5 kB
* Tags: CPython 3.13, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f5604160a3504401d2d4a1b6228e1093e31b684b4ce4997c165dbc1a8eb60cde` | Copy |
| MD5 | `e7a6ac3719e1216fba66dc52c2495db9` | Copy |
| BLAKE2b-256 | `ebd2ebe1b4408c83a45c42c4911acdc3f2042ff2475bff017ea6060c68ecfdef` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/72/c1/37b1ed6ebf5de27494e7ef88fe93d7b1d44163f00ee4178ca0394e6a1561/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 517.4 kB
* Tags: CPython 3.13, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `3d8bc8015967f5aeb299c2d053fe8869861bb8eba9547cd49c2bec93d36d096e` | Copy |
| MD5 | `682dd7093f84c2907d72d7d891009852` | Copy |
| BLAKE2b-256 | `72c137b1ed6ebf5de27494e7ef88fe93d7b1d44163f00ee4178ca0394e6a1561` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_5_i686.manylinux1_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/78/ae/7fab495e2c69f4da672e3b321e7091247d6d8f8bb6c922a2c7f34cda9ef4/wavelet_matrix-2.0.3-cp313-cp313-manylinux_2_5_i686.manylinux1_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 638.0 kB
* Tags: CPython 3.13, manylinux: glibc 2.5+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-manylinux\_2\_5\_i686.manylinux1\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `59c04e4fae98d75b36fd379cb4c195506ecd1679607d9510b81bf77228290217` | Copy |
| MD5 | `ac8c5146a340317d95ee48a01ea2d52e` | Copy |
| BLAKE2b-256 | `78ae7fab495e2c69f4da672e3b321e7091247d6d8f8bb6c922a2c7f34cda9ef4` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-macosx_11_0_arm64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-macosx\_11\_0\_arm64.whl](https://files.pythonhosted.org/packages/37/c8/242f0c79ed377f61e43c4bb763eff223599db28451cefb6769b3477fca91/wavelet_matrix-2.0.3-cp313-cp313-macosx_11_0_arm64.whl)
* Upload date:
  Dec 16, 2025
* Size: 501.6 kB
* Tags: CPython 3.13, macOS 11.0+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-macosx\_11\_0\_arm64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `32d08f3531f9899bf70bf065c0a44e2dbe30d5b980edb604dada7da270573eec` | Copy |
| MD5 | `721548a953f9226f9f45eebce3134689` | Copy |
| BLAKE2b-256 | `37c8242f0c79ed377f61e43c4bb763eff223599db28451cefb6769b3477fca91` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp313-cp313-macosx_10_12_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp313-cp313-macosx\_10\_12\_x86\_64.whl](https://files.pythonhosted.org/packages/42/48/abf02fc482c86aabfedc235f120b7f6327045ee5cd8d3b3ec89ddfb664c8/wavelet_matrix-2.0.3-cp313-cp313-macosx_10_12_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 535.4 kB
* Tags: CPython 3.13, macOS 10.12+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp313-cp313-macosx\_10\_12\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `1b4cd1290d44f9f4cb3d5f047bfd2ae285f4f0fde69a26081bd97bec388d1e7e` | Copy |
| MD5 | `c828a618906e196bcc6c7cc4f24a2c30` | Copy |
| BLAKE2b-256 | `4248abf02fc482c86aabfedc235f120b7f6327045ee5cd8d3b3ec89ddfb664c8` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-win_amd64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-win\_amd64.whl](https://files.pythonhosted.org/packages/e5/dd/90c8a3612d93aa6f3f72997f7018c72890d86831293c25774e30a4ad681e/wavelet_matrix-2.0.3-cp312-cp312-win_amd64.whl)
* Upload date:
  Dec 16, 2025
* Size: 391.9 kB
* Tags: CPython 3.12, Windows x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-win\_amd64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `ee0e33b438d2275bb579439ea48b9445055b5a4484d5495cf115a03a4833e2b6` | Copy |
| MD5 | `014806448e305d5958b2da5a36eb642a` | Copy |
| BLAKE2b-256 | `e5dd90c8a3612d93aa6f3f72997f7018c72890d86831293c25774e30a4ad681e` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/fc/69/79851c4e1cbaa83b6b86d9e18997bc36bdefdb5f06a344dcaa695a3e85e1/wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 760.7 kB
* Tags: CPython 3.12, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `c202a51e4ec83d20efb2df1ca77eee200ec38d3031685145b3dee2954e18bed0` | Copy |
| MD5 | `d3f6ebf3bdfc140c6b18a0bc43ca4b51` | Copy |
| BLAKE2b-256 | `fc6979851c4e1cbaa83b6b86d9e18997bc36bdefdb5f06a344dcaa695a3e85e1` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/1f/30/e058019c3fdf83935d5f2008cd46e9bf13056ca35beddc3c92dde3cb3268/wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 826.0 kB
* Tags: CPython 3.12, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `d068ff0d82eb4322f0cfeed94830c5dcb24cb26be623838e299d07284c1dcd4a` | Copy |
| MD5 | `b04e91870ea0366e02ca52741927d20c` | Copy |
| BLAKE2b-256 | `1f30e058019c3fdf83935d5f2008cd46e9bf13056ca35beddc3c92dde3cb3268` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/53/d3/60e6dd088b245282e82a8c47c0fd6d4a86247cbecfa930a561a9f2832ad6/wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 855.6 kB
* Tags: CPython 3.12, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f8d5c5147ffdf078613424140141bc7d5c532a8a0ce1129cfd37d9bfee9ebe01` | Copy |
| MD5 | `f5cc45b2ed732963ce109d7f0bf940de` | Copy |
| BLAKE2b-256 | `53d360e6dd088b245282e82a8c47c0fd6d4a86247cbecfa930a561a9f2832ad6` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/ec/98/087773434b7652b0f583309cac739f91a4d12d33d8e9b23dc179f8f1efc8/wavelet_matrix-2.0.3-cp312-cp312-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 700.3 kB
* Tags: CPython 3.12, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `16b7236511ffa68201edf22140ca5e6ea50ccb5d2ef79ecfc5d1133b22973809` | Copy |
| MD5 | `49adf500912afaf0a00f8e388a43ca64` | Copy |
| BLAKE2b-256 | `ec98087773434b7652b0f583309cac739f91a4d12d33d8e9b23dc179f8f1efc8` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/7c/c2/80bb3c9670998ed73e562843f6ee5fa54ccf9816f4d19c8518bf476159d8/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 555.2 kB
* Tags: CPython 3.12, manylinux: glibc 2.17+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `0dac21e3a6fe63e61dc502570373dcdc241e29448db4f4e378ebef5e69e9f2f5` | Copy |
| MD5 | `9f68186ac0bd3c67ca40fb5bcf91e594` | Copy |
| BLAKE2b-256 | `7cc280bb3c9670998ed73e562843f6ee5fa54ccf9816f4d19c8518bf476159d8` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/36/28/643c80cc66ca7ca2ed261c01dcbb10c332797b35d97111cf650227a6b8fc/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 584.4 kB
* Tags: CPython 3.12, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `599d642373ebfae1a2ad4e60030f87446050d25a8b283145f812398d13d9d436` | Copy |
| MD5 | `3447c531e83db7116b9f88dff9931458` | Copy |
| BLAKE2b-256 | `3628643c80cc66ca7ca2ed261c01dcbb10c332797b35d97111cf650227a6b8fc` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/0a/2e/9d36211b515508cdd91a74c2138904c4e4bd4d18a37482e93f885bd18089/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 678.1 kB
* Tags: CPython 3.12, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `1664d57e9fdab46a9ed9aac517947a07ebedb1a3d27bffb2f0bacefc0b7c9a4c` | Copy |
| MD5 | `deea413fb74044adbad0339d90b93d3d` | Copy |
| BLAKE2b-256 | `0a2e9d36211b515508cdd91a74c2138904c4e4bd4d18a37482e93f885bd18089` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/5a/47/b617620e73276818ffdada8fff3d4133a580a9b51601e5fdb9bd53d81ec4/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 586.7 kB
* Tags: CPython 3.12, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f66cac31f1589e2cd9b0a40c832e37d8f5b2a754b69226099e6f92f42cbfc3b6` | Copy |
| MD5 | `b0914c27966e5e8cb166903cedc56e91` | Copy |
| BLAKE2b-256 | `5a47b617620e73276818ffdada8fff3d4133a580a9b51601e5fdb9bd53d81ec4` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/a6/ed/848eeb3521def3fa01e71130305c85a992d0fca430f15105396b1e408cc8/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 517.2 kB
* Tags: CPython 3.12, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `4eb40dc09f59855da80e47ac855b4f4d0b9b983e23e2985df061a85f18d9a128` | Copy |
| MD5 | `4404b5467d5104f365f8a9051a37126e` | Copy |
| BLAKE2b-256 | `a6ed848eeb3521def3fa01e71130305c85a992d0fca430f15105396b1e408cc8` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/6d/08/cdcfe51d87cc53e88cf6744074b3a646e0f31aab01dda11c4bb103e72e46/wavelet_matrix-2.0.3-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 638.1 kB
* Tags: CPython 3.12, manylinux: glibc 2.5+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-manylinux\_2\_5\_i686.manylinux1\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `7a7aa652aa8bd436f5041a407003014727a369f29bd03a148758d0f733f79c8b` | Copy |
| MD5 | `eb19fac1d9dbec4adf8576fb476a0177` | Copy |
| BLAKE2b-256 | `6d08cdcfe51d87cc53e88cf6744074b3a646e0f31aab01dda11c4bb103e72e46` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-macosx_11_0_arm64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-macosx\_11\_0\_arm64.whl](https://files.pythonhosted.org/packages/32/37/a8fe1064a5d0601118afe3df5385e7331d85fa466f4595505e405da52958/wavelet_matrix-2.0.3-cp312-cp312-macosx_11_0_arm64.whl)
* Upload date:
  Dec 16, 2025
* Size: 501.5 kB
* Tags: CPython 3.12, macOS 11.0+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-macosx\_11\_0\_arm64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `5ef845f64947d6aa59ee2c34217b2be4709bfeb9963960758c4aaad63dad44b8` | Copy |
| MD5 | `7bddbf502ba1c1d62577a4f1d68adc3d` | Copy |
| BLAKE2b-256 | `3237a8fe1064a5d0601118afe3df5385e7331d85fa466f4595505e405da52958` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp312-cp312-macosx_10_12_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp312-cp312-macosx\_10\_12\_x86\_64.whl](https://files.pythonhosted.org/packages/82/31/d61e391b567237c7337a1311b3997797281c01dbfdc18e35f9942e46eb9b/wavelet_matrix-2.0.3-cp312-cp312-macosx_10_12_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 535.1 kB
* Tags: CPython 3.12, macOS 10.12+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp312-cp312-macosx\_10\_12\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `af077e7d2cb0b13459f2b394f282b6fb496aac67302932b5531a544e114814c3` | Copy |
| MD5 | `41ddd25a1438b88b9908b73b647a980b` | Copy |
| BLAKE2b-256 | `8231d61e391b567237c7337a1311b3997797281c01dbfdc18e35f9942e46eb9b` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-win_amd64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-win\_amd64.whl](https://files.pythonhosted.org/packages/e4/bf/938887971a928a61524d0b9e6d7b8621de98f4e79a4079b1479e22a902b6/wavelet_matrix-2.0.3-cp311-cp311-win_amd64.whl)
* Upload date:
  Dec 16, 2025
* Size: 389.9 kB
* Tags: CPython 3.11, Windows x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-win\_amd64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `1fbf1a29e7c64f05d558df4c6b51391c0147c09abeb76261805a2584dda486fe` | Copy |
| MD5 | `d08c9dcd4446113d48e7d1dc2c78961b` | Copy |
| BLAKE2b-256 | `e4bf938887971a928a61524d0b9e6d7b8621de98f4e79a4079b1479e22a902b6` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/dc/a6/24d4d9e194d9c615cb1a9be6ca502795e918c75b1aef2e880c5bffc873e1/wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 759.5 kB
* Tags: CPython 3.11, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `23bdd98cddbf694ffe04e18c7bf7be3989d24cce2543580626fed8ad3d697e30` | Copy |
| MD5 | `f30092b4957419ad0529207ee1271e5a` | Copy |
| BLAKE2b-256 | `dca624d4d9e194d9c615cb1a9be6ca502795e918c75b1aef2e880c5bffc873e1` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/38/67/3779e52a65e029ea46cbd8735c7604b26e8838a85dc6d3fb3181b5ab085f/wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 825.5 kB
* Tags: CPython 3.11, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `2c4eb4d8cc5991f91b3c942b4200fe24e87594a6ff3b1eeb3d704e142aee8c7d` | Copy |
| MD5 | `e65e6ad9741fd5bea21bc7fd2609dcc8` | Copy |
| BLAKE2b-256 | `38673779e52a65e029ea46cbd8735c7604b26e8838a85dc6d3fb3181b5ab085f` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/fa/6d/8d43f7b59eff75f9262122ee405a2fc128fe443edeb023eb5dfa5acb0110/wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 852.1 kB
* Tags: CPython 3.11, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `196331747f8686d44435439542487cbeb7e5067a5601aaae05f1c0c71da9f6b6` | Copy |
| MD5 | `add0f13dd11cc4b79eb26b5f71950813` | Copy |
| BLAKE2b-256 | `fa6d8d43f7b59eff75f9262122ee405a2fc128fe443edeb023eb5dfa5acb0110` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/13/18/032f03134a27a43294719c24a5439aa1b2261d1dd43d745dcdd10851da19/wavelet_matrix-2.0.3-cp311-cp311-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 701.0 kB
* Tags: CPython 3.11, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f2fffd6b59477530172c2aea1d835fae26f0b502b4714d7f4d2c5425bfbbabfa` | Copy |
| MD5 | `8d487c2cbc326bc122e54c786a7432a7` | Copy |
| BLAKE2b-256 | `1318032f03134a27a43294719c24a5439aa1b2261d1dd43d745dcdd10851da19` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/9a/00/365c574a53e3ebcca7a952f8bd52c8470e2a2743442d48b519bd947eab4d/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 553.2 kB
* Tags: CPython 3.11, manylinux: glibc 2.17+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `75aeafac5cd3d79b51938e30ef32c817eef39ea65dd84f7b186b028b9b2ed02e` | Copy |
| MD5 | `a764849081d1a1fca8caf3d25da04ca4` | Copy |
| BLAKE2b-256 | `9a00365c574a53e3ebcca7a952f8bd52c8470e2a2743442d48b519bd947eab4d` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/f2/11/190086b3ea16618e9240b5372482d0a9db901d5f39b7251b25e939d4446d/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 583.1 kB
* Tags: CPython 3.11, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `1b4a560fe49dc9b33cc0e240921a47d5adf582856b70a389e3c3ee5bfc0b7e79` | Copy |
| MD5 | `1d004fe8d16ebf56fa9201582d3fd4a7` | Copy |
| BLAKE2b-256 | `f211190086b3ea16618e9240b5372482d0a9db901d5f39b7251b25e939d4446d` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/4b/7b/25671096db37e0af815746d28fd3ae95fa95aed9c7b085bb6c97a328b92d/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 686.4 kB
* Tags: CPython 3.11, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `889092d7b2ebefafc3edeb737c7bdca7ff2cc87f08a7ed10f70c79714cd7e2ae` | Copy |
| MD5 | `d1a0f56882174f16683262b9888fcd0b` | Copy |
| BLAKE2b-256 | `4b7b25671096db37e0af815746d28fd3ae95fa95aed9c7b085bb6c97a328b92d` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/7f/be/0ee051c75b8b225316918320e5cbb5f19b77009b694dddcd9f0b6ad13393/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 581.4 kB
* Tags: CPython 3.11, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `582e79a3e6fa64bd16bc491c2b64acf4ae9b56f1da711edc02d49b6ce88933cb` | Copy |
| MD5 | `3a9022d86a77814e4331b50601557912` | Copy |
| BLAKE2b-256 | `7fbe0ee051c75b8b225316918320e5cbb5f19b77009b694dddcd9f0b6ad13393` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/a9/d2/f5b1094d1d1e3fa70b773e7b334693dbb77cc88e81dc6bd0810bb6a34c84/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 517.9 kB
* Tags: CPython 3.11, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `cd30354fe9bea425477f9c37b1ec8e6ba45167624b83400b0908eb16b330732b` | Copy |
| MD5 | `f182dba05a127b2130cd8df99051a21c` | Copy |
| BLAKE2b-256 | `a9d2f5b1094d1d1e3fa70b773e7b334693dbb77cc88e81dc6bd0810bb6a34c84` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/42/cf/d515fc393ab4f570e0aa15f52216efb6d41097c93ea96de266794e7eadf4/wavelet_matrix-2.0.3-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 635.4 kB
* Tags: CPython 3.11, manylinux: glibc 2.5+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-manylinux\_2\_5\_i686.manylinux1\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `06c516ce25e5a467a6a35d4077f5c1acd6e6688d2f74e51836402b9faf30560e` | Copy |
| MD5 | `f35ef361b04f315c79602dbe073cdadb` | Copy |
| BLAKE2b-256 | `42cfd515fc393ab4f570e0aa15f52216efb6d41097c93ea96de266794e7eadf4` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-macosx_11_0_arm64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-macosx\_11\_0\_arm64.whl](https://files.pythonhosted.org/packages/86/8d/1381cd46cc240775f58c717ed9481d5b2845f942824e4e7f71b317dc3be0/wavelet_matrix-2.0.3-cp311-cp311-macosx_11_0_arm64.whl)
* Upload date:
  Dec 16, 2025
* Size: 498.4 kB
* Tags: CPython 3.11, macOS 11.0+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-macosx\_11\_0\_arm64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `8ec89120b7cdb751b4eca3f5ca403eff7dd21dcb4bacbf4d2309c7aa3711b1eb` | Copy |
| MD5 | `b01302217f874adca75e9b599cbb468d` | Copy |
| BLAKE2b-256 | `868d1381cd46cc240775f58c717ed9481d5b2845f942824e4e7f71b317dc3be0` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp311-cp311-macosx_10_12_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp311-cp311-macosx\_10\_12\_x86\_64.whl](https://files.pythonhosted.org/packages/01/b6/4dab9a5060dc936c34c39e4ea3a8915b627f211ef9fa70fd589c917810bb/wavelet_matrix-2.0.3-cp311-cp311-macosx_10_12_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 534.6 kB
* Tags: CPython 3.11, macOS 10.12+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp311-cp311-macosx\_10\_12\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `c416ff811542debc4df260224f4848bd91d629c581cb52168003e91578c3b05a` | Copy |
| MD5 | `2ca95e57f6f750d8615a763eaa069403` | Copy |
| BLAKE2b-256 | `01b64dab9a5060dc936c34c39e4ea3a8915b627f211ef9fa70fd589c917810bb` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-win_amd64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-win\_amd64.whl](https://files.pythonhosted.org/packages/53/31/c4b9a1ae11046af45e2cffc68ce642a3333bb55dc0618ca1a037ba44d140/wavelet_matrix-2.0.3-cp310-cp310-win_amd64.whl)
* Upload date:
  Dec 16, 2025
* Size: 389.8 kB
* Tags: CPython 3.10, Windows x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-win\_amd64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `0e2ce3d717876589b77f99458eb40c8cca822b3e7e3384c32ba647bfb76a0135` | Copy |
| MD5 | `12d2a4dad9b816c3eb7ab332a46a49c9` | Copy |
| BLAKE2b-256 | `5331c4b9a1ae11046af45e2cffc68ce642a3333bb55dc0618ca1a037ba44d140` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/1d/aa/d409cb71fa4ccb3f63264657b9dc2a5d56aa0286403de0e9e845c8893913/wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 759.5 kB
* Tags: CPython 3.10, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `cff85060834d03cd739950dfce1955dc25da6fcf4d33c20dc9973242895a63bb` | Copy |
| MD5 | `bafb4b98ac585ed3b4b921f4ae99f873` | Copy |
| BLAKE2b-256 | `1daad409cb71fa4ccb3f63264657b9dc2a5d56aa0286403de0e9e845c8893913` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/79/7b/c5a0330466263be3dfd2b985b973b9ac85d8e567d55c5074eb573b83408d/wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 825.6 kB
* Tags: CPython 3.10, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `4445cc151e198263d4f69e69c5c9cde5f4df446129312c2cf64d166d4db69700` | Copy |
| MD5 | `ae6375c10e29dc5c5c370414e94ff04a` | Copy |
| BLAKE2b-256 | `797bc5a0330466263be3dfd2b985b973b9ac85d8e567d55c5074eb573b83408d` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/78/29/5738b1a0665c72078aaaa1b837163671a41588edb7441be12691f47b9040/wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 851.5 kB
* Tags: CPython 3.10, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `897f71ebde74e2f8beebd9bd694bb951d0e29da0b7917fedff6dee1333031b5b` | Copy |
| MD5 | `7b5c8275f3c62f9b71cafe9558fc9146` | Copy |
| BLAKE2b-256 | `78295738b1a0665c72078aaaa1b837163671a41588edb7441be12691f47b9040` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/53/15/42ab03f3309ffbf4740b6970deab952f960c0aece9b54c57d4c1475fa590/wavelet_matrix-2.0.3-cp310-cp310-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 700.7 kB
* Tags: CPython 3.10, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `66f353c5aef2198fd75d2b2c4d1f0684c74bba8bf7025d7026b7c23bd8860683` | Copy |
| MD5 | `a8f904a5e56ac4587981536c0a785156` | Copy |
| BLAKE2b-256 | `531542ab03f3309ffbf4740b6970deab952f960c0aece9b54c57d4c1475fa590` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/1d/1e/29d8e283ea79a3d881a68367f4e0f25603589bad3afd7cfdc9b782439ca2/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 553.3 kB
* Tags: CPython 3.10, manylinux: glibc 2.17+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `dba332a218b47663f27e2945945ee0fdda138dd6fb4ae5fbfc2b21c513a481b2` | Copy |
| MD5 | `b8d209d53dd2c6bb0dc8df40810d1071` | Copy |
| BLAKE2b-256 | `1d1e29d8e283ea79a3d881a68367f4e0f25603589bad3afd7cfdc9b782439ca2` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/85/8d/1d976332785e9632ea123a4499b1d291976fd54e2a1e73601fc52f451630/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 583.6 kB
* Tags: CPython 3.10, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `cb870c1b1e69f57bc1e007fdb059a0807ee7834776de10da8fbadceac76bfc4d` | Copy |
| MD5 | `481134f4acc28e07132c59bd03ec9321` | Copy |
| BLAKE2b-256 | `858d1d976332785e9632ea123a4499b1d291976fd54e2a1e73601fc52f451630` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/3d/65/e144fe146bf22f6bf16ee2483a8496361d173088cc000a7035c4bb66c7d2/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 686.2 kB
* Tags: CPython 3.10, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `ad69ee7e096ad5dc4be2a41c1964c0cf5f2a5d314b87e6ec600ebad821e17a16` | Copy |
| MD5 | `001f0c30f0ff4da75aa008f0dbc5d151` | Copy |
| BLAKE2b-256 | `3d65e144fe146bf22f6bf16ee2483a8496361d173088cc000a7035c4bb66c7d2` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/2e/74/85aeb0384983970ff7ca3acb6307db830369d5aac0b5389ca8e2909d83d4/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 580.8 kB
* Tags: CPython 3.10, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `2c53217cfad709c8c67e0f180bfe555e6a6565ec0e040dbdc9777415c69e3215` | Copy |
| MD5 | `9834001f191125fdfeffe1acd5f18bc5` | Copy |
| BLAKE2b-256 | `2e7485aeb0384983970ff7ca3acb6307db830369d5aac0b5389ca8e2909d83d4` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/78/84/9f435f181f1fdb00279840273a1900c38c02ff8190a34bf34fa6868fdd99/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 517.1 kB
* Tags: CPython 3.10, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `2a8794e70115a2a47f320beaa2fd247cab668ad43047c461d0e30881541340b5` | Copy |
| MD5 | `1f81721c7dd4db48afe6fc73ea3d9c0e` | Copy |
| BLAKE2b-256 | `78849f435f181f1fdb00279840273a1900c38c02ff8190a34bf34fa6868fdd99` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_5_i686.manylinux1_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/18/a7/221ba43f28619a43666979390290250aff96fd01059f6eef96d9a0f8208d/wavelet_matrix-2.0.3-cp310-cp310-manylinux_2_5_i686.manylinux1_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 635.6 kB
* Tags: CPython 3.10, manylinux: glibc 2.5+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp310-cp310-manylinux\_2\_5\_i686.manylinux1\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `cae7d18feb5ece81a37986f21019515b1648fedbd29e22d501e15c8b412f468b` | Copy |
| MD5 | `609d0bee70c00ee2afb535f375435805` | Copy |
| BLAKE2b-256 | `18a7221ba43f28619a43666979390290250aff96fd01059f6eef96d9a0f8208d` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-win_amd64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-win\_amd64.whl](https://files.pythonhosted.org/packages/fb/61/92a366b847ca026dce75601dff887a1915a8b4ad6ae50d97151c33c321a3/wavelet_matrix-2.0.3-cp39-cp39-win_amd64.whl)
* Upload date:
  Dec 16, 2025
* Size: 391.8 kB
* Tags: CPython 3.9, Windows x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-win\_amd64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `eced13465837f768c2d7997fd7f56732ef45cb58e1b95f174b05fed6dbddc3c7` | Copy |
| MD5 | `56dd9dcbfcdb8453082c03f0e1a397d4` | Copy |
| BLAKE2b-256 | `fb6192a366b847ca026dce75601dff887a1915a8b4ad6ae50d97151c33c321a3` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_x86\_64.whl](https://files.pythonhosted.org/packages/5b/45/68682d96f62da999b980339da512898edde1e1837aa28ffce076b1184b98/wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 761.3 kB
* Tags: CPython 3.9, musllinux: musl 1.2+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `6612b8bc20d20a4fe2c3dde0dd806580ae715f70206cca3d873df0af685b77e5` | Copy |
| MD5 | `26d87b387e984d597c4124f75e852a6e` | Copy |
| BLAKE2b-256 | `5b4568682d96f62da999b980339da512898edde1e1837aa28ffce076b1184b98` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_i686.whl](https://files.pythonhosted.org/packages/30/4a/26bd11d3757fed94e6e57d31dd308ca47c39abbc3a02a3badb056d4f1c40/wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 826.6 kB
* Tags: CPython 3.9, musllinux: musl 1.2+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `2a40cc13575df85febf4d1942beb968ad21d8092b04bbaef5082153b88373d2c` | Copy |
| MD5 | `8910793da9d228519a1ada4124fafe69` | Copy |
| BLAKE2b-256 | `304a26bd11d3757fed94e6e57d31dd308ca47c39abbc3a02a3badb056d4f1c40` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_armv7l.whl](https://files.pythonhosted.org/packages/fa/14/409c5941493cb500e00fb577f74208002486479d1a52549a27b8e087ee1e/wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 852.9 kB
* Tags: CPython 3.9, musllinux: musl 1.2+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `d5d8b0814c630e0510d86926eec0ee8bd56e9e0d4da0d4f5c49bc8c2d3512fb5` | Copy |
| MD5 | `5ffdb6a65e55a68fbe82e42bd5a897a0` | Copy |
| BLAKE2b-256 | `fa14409c5941493cb500e00fb577f74208002486479d1a52549a27b8e087ee1e` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_aarch64.whl](https://files.pythonhosted.org/packages/a9/6d/2202a699f3731b314374051e23314da9e7b7794d98c7c21bb5adc49dfa97/wavelet_matrix-2.0.3-cp39-cp39-musllinux_1_2_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 702.0 kB
* Tags: CPython 3.9, musllinux: musl 1.2+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-musllinux\_1\_2\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `e8ff70476d04889949f04bb3287442b92098d1d94542b325cb6077d0980af98b` | Copy |
| MD5 | `81e3a1a7f713190272adf059322c46ab` | Copy |
| BLAKE2b-256 | `a96d2202a699f3731b314374051e23314da9e7b7794d98c7c21bb5adc49dfa97` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl](https://files.pythonhosted.org/packages/09/a2/7f6c05c1ac95c6f56011bec510232c63379ed397f5108a93106b89157e31/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
* Upload date:
  Dec 16, 2025
* Size: 556.0 kB
* Tags: CPython 3.9, manylinux: glibc 2.17+ x86-64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_x86\_64.manylinux2014\_x86\_64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f393635f25833b29594481893ee89793c15ace4793b2938fee628aa472706c81` | Copy |
| MD5 | `33b0ae6397531557a91df75555e86a19` | Copy |
| BLAKE2b-256 | `09a27f6c05c1ac95c6f56011bec510232c63379ed397f5108a93106b89157e31` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_s390x.manylinux2014_s390x.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl](https://files.pythonhosted.org/packages/33/da/442c38dffc93cb77696559eaee13ffc39cebfdcb25cdc5575b5b0543dcfe/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_s390x.manylinux2014_s390x.whl)
* Upload date:
  Dec 16, 2025
* Size: 586.6 kB
* Tags: CPython 3.9, manylinux: glibc 2.17+ s390x
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_s390x.manylinux2014\_s390x.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `87566c546eeaae855a8958cc001e92425a3513e021b68ac54d727371fa937db0` | Copy |
| MD5 | `23d9ab7200ec7e24790eaca6ae7f7d9a` | Copy |
| BLAKE2b-256 | `33da442c38dffc93cb77696559eaee13ffc39cebfdcb25cdc5575b5b0543dcfe` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl](https://files.pythonhosted.org/packages/ed/90/0b38b11809f4c6c543a1d079c354019b8b7ea96063e2afb591749ca599b5/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl)
* Upload date:
  Dec 16, 2025
* Size: 688.2 kB
* Tags: CPython 3.9, manylinux: glibc 2.17+ ppc64le
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_ppc64le.manylinux2014\_ppc64le.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `f556d94bbcc7c4f54d9945ad05983d05f3746be53aa55f9a58951b23854fb65b` | Copy |
| MD5 | `b9c25d8735910e73b9a243848fefe285` | Copy |
| BLAKE2b-256 | `ed900b38b11809f4c6c543a1d079c354019b8b7ea96063e2afb591749ca599b5` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_armv7l.manylinux2014_armv7l.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl](https://files.pythonhosted.org/packages/6d/8a/5c519186a00c66f12dca2bdc0b5ff3a565c49fcc581ec695d648eb1e74d7/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_armv7l.manylinux2014_armv7l.whl)
* Upload date:
  Dec 16, 2025
* Size: 582.3 kB
* Tags: CPython 3.9, manylinux: glibc 2.17+ ARMv7l
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_armv7l.manylinux2014\_armv7l.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `17fd29742c06409fa08da648a9178ccec87192a648882a87fc261c994086c0e6` | Copy |
| MD5 | `783f7cb57c67631016d1f5400e104e47` | Copy |
| BLAKE2b-256 | `6d8a5c519186a00c66f12dca2bdc0b5ff3a565c49fcc581ec695d648eb1e74d7` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl](https://files.pythonhosted.org/packages/e5/97/f8c35656810d83d5e457c62714bb717fd4e325fef9c253c848f02fd2f7ed/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
* Upload date:
  Dec 16, 2025
* Size: 519.8 kB
* Tags: CPython 3.9, manylinux: glibc 2.17+ ARM64
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_17\_aarch64.manylinux2014\_aarch64.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `a47818c22c8bfc3ed2f76e775c2985e002eca7c8b4528fade13ae5b47508deb7` | Copy |
| MD5 | `3e4f172fdef73370aa700d6c745f18a2` | Copy |
| BLAKE2b-256 | `e597f8c35656810d83d5e457c62714bb717fd4e325fef9c253c848f02fd2f7ed` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

## File details

Details for the file `wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_5_i686.manylinux1_i686.whl`.

### File metadata

* Download URL: [wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_5\_i686.manylinux1\_i686.whl](https://files.pythonhosted.org/packages/a3/72/513f95a7ea7593f5374c03b92577cd3b2096907163d5aad01a68ffc27f1a/wavelet_matrix-2.0.3-cp39-cp39-manylinux_2_5_i686.manylinux1_i686.whl)
* Upload date:
  Dec 16, 2025
* Size: 636.9 kB
* Tags: CPython 3.9, manylinux: glibc 2.5+ i686
* Uploaded using Trusted Publishing? No
* Uploaded via: maturin/1.10.2

### File hashes

Hashes for wavelet\_matrix-2.0.3-cp39-cp39-manylinux\_2\_5\_i686.manylinux1\_i686.whl

| Algorithm | Hash digest |  |
| --- | --- | --- |
| SHA256 | `2893731c9a80e32e2e4a44c086cde44ad25b213b8f69c580a769c58f2e77b24a` | Copy |
| MD5 | `73fc3a8b1eba094096c2ce3f0d2b80a9` | Copy |
| BLAKE2b-256 | `a372513f95a7ea7593f5374c03b92577cd3b2096907163d5aad01a68ffc27f1a` | Copy |

[See more details on using hashes here.](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-checking-mode "External link")

![](/static/images/white-cube.2351a86c.svg)

## Help

* [Installing packages](https://packaging.python.org/tutorials/installing-packages/ "External link")
* [Uploading packages](https://packaging.python.org/tutorials/packaging-projects/ "External link")
* [User guide](https://packaging.python.org/ "External link")
* [Project name retention](https://www.python.org/dev/peps/pep-0541/ "External link")
* [FAQs](/help/)

## About PyPI

* [PyPI Blog](https://blog.pypi.org "External link")
* [Infrastructure dashboard](https://dtdg.co/pypi "External link")
* [Statistics](/stats/)
* [Logos & trademarks](/trademarks/)
* [Our sponsors](/sponsors/)

## Contributing to PyPI

* [Bugs and feedback](/help/#feedback)
* [Contribute on GitHub](https://github.com/pypi/warehouse "External link")
* [Translate PyPI](https://hosted.weblate.org/projects/pypa/warehouse/ "External link")
* [Sponsor PyPI](/sponsors/)
* [Development credits](https://github.com/pypi/warehouse/graphs/contributors "External link")

## Using PyPI

* [Terms of Service](https://policies.python.org/pypi.org/Terms-of-Service/ "External link")
* [Report security issue](/security/)
* [Code of conduct](https://policies.python.org/python.org/code-of-conduct/ "External link")
* [Privacy Notice](https://policies.python.org/pypi.org/Privacy-Notice/ "External link")
* [Acceptable Use Policy](https://policies.python.org/pypi.org/Acceptable-Use-Policy/ "External link")

---

Status:[all systems operational](https://status.python.org/ "External link")

Developed and maintained by the Python community, for the Python community.
[Donate today!](https://donate.pypi.org)

"PyPI", "Python Package Index", and the blocks logos are registered [trademarks](/trademarks/) of the [Python Software Foundation](https://www.python.org/psf-landing).

© 2025 [Python Software Foundation](https://www.python.org/psf-landing/ "External link")

[Site map](/sitemap/)

Switch to desktop version

* English
* español
* français
* 日本語
* português (Brasil)
* українська
* Ελληνικά
* Deutsch
* 中文 (简体)
* 中文 (繁體)
* русский
* עברית
* Esperanto
* 한국어

Supported by

[![](https://pypi-camo.freetls.fastly.net/ed7074cadad1a06f56bc520ad9bd3e00d0704c5b/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f707970692d6173736574732f73706f6e736f726c6f676f732f6177732d77686974652d6c6f676f2d7443615473387a432e706e67)
AWS

Cloud computing and Security Sponsor](https://aws.amazon.com/)
[![](https://pypi-camo.freetls.fastly.net/8855f7c063a3bdb5b0ce8d91bfc50cf851cc5c51/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f707970692d6173736574732f73706f6e736f726c6f676f732f64617461646f672d77686974652d6c6f676f2d6668644c4e666c6f2e706e67)
Datadog

Monitoring](https://www.datadoghq.com/)
[![](https://pypi-camo.freetls.fastly.net/60f709d24f3e4d469f9adc77c65e2f5291a3d165/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f707970692d6173736574732f73706f6e736f726c6f676f732f6465706f742d77686974652d6c6f676f2d7038506f476831302e706e67)
Depot

Continuous Integration](https://depot.dev)
[![](https://pypi-camo.freetls.fastly.net/df6fe8829cbff2d7f668d98571df1fd011f36192/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f707970692d6173736574732f73706f6e736f726c6f676f732f666173746c792d77686974652d6c6f676f2d65684d3077735f6f2e706e67)
Fastly

CDN](https://www.fastly.com/)
[![](https://pypi-camo.freetls.fastly.net/420cc8cf360bac879e24c923b2f50ba7d1314fb0/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f707970692d6173736574732f73706f6e736f726c6f676f732f676f6f676c652d77686974652d6c6f676f2d616734424e3774332e706e67)
Google

Download Analytics](https://careers.google.com/)
[![](https://pypi-camo.freetls.fastly.net/d01053c02f3a626b73ffcb06b96367fdbbf9e230/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f707970692d6173736574732f73706f6e736f726c6f676f732f70696e67646f6d2d77686974652d6c6f676f2d67355831547546362e706e67)
Pingdom

Monitoring](https://www.pingdom.com/)
[![](https://pypi-camo.freetls.fastly.net/67af7117035e2345bacb5a82e9aa8b5b3e70701d/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f707970692d6173736574732f73706f6e736f726c6f676f732f73656e7472792d77686974652d6c6f676f2d4a2d6b64742d706e2e706e67)
Sentry

Error logging](https://sentry.io/for/python/?utm_source=pypi&utm_medium=paid-community&utm_campaign=python-na-evergreen&utm_content=static-ad-pypi-sponsor-learnmore)
[![](https://pypi-camo.freetls.fastly.net/b611884ff90435a0575dbab7d9b0d3e60f136466/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f707970692d6173736574732f73706f6e736f726c6f676f732f737461747573706167652d77686974652d6c6f676f2d5467476c6a4a2d502e706e67)
StatusPage

Status page](https://statuspage.io)
