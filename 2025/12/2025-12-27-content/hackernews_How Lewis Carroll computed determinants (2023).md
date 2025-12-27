---
source: hackernews
title: How Lewis Carroll computed determinants (2023)
url: https://www.johndcook.com/blog/2023/07/10/lewis-carroll-determinants/
date: 2025-12-27
---

[![John D. Cook](https://www.johndcook.com/wp-content/themes/ThemeAlley.Business.Pro/images/Logo.svg)](/)

[Skip to content](#content "Skip to content")

* [MATH](https://www.johndcook.com/blog/applied-math/)
  + [PROBABILITY](https://www.johndcook.com/blog/applied-probability/)
  + [SIGNAL PROCESSING](https://www.johndcook.com/blog/digital-signal-processing-and-time-series-analysis/)
  + [NUMERICAL COMPUTING](https://www.johndcook.com/blog/numerical-computation/)
  + [SEE ALL …](https://www.johndcook.com/blog/applied-math/)
* [STATS](https://www.johndcook.com/blog/applied-statistics/)
  + [EXPERT TESTIMONY](https://www.johndcook.com/blog/expert-testimony/)
  + [WEB ANALYTICS](https://www.johndcook.com/blog/web-analytics/)
  + [FORECASTING](https://www.johndcook.com/blog/forecasting/)
  + [RNG TESTING](https://www.johndcook.com/blog/rng-testing/)
  + [SEE ALL …](https://www.johndcook.com/blog/applied-statistics/)
* [PRIVACY](https://www.johndcook.com/blog/data-privacy/)
  + [HIPAA](https://www.johndcook.com/blog/expert-hipaa-deidentification/)
  + [SAFE HARBOR](https://www.johndcook.com/blog/hipaa-identifiers-explained/)
  + [CRYPTOGRAPHY](https://www.johndcook.com/blog/cryptography/)
  + [DIFFERENTIAL PRIVACY](https://www.johndcook.com/blog/differential-privacy/)
  + [PRIVACY FAQ](https://www.johndcook.com/blog/data-privacy-faq/)
* [WRITING](https://www.johndcook.com/blog/writing/)
  + [BLOG](https://www.johndcook.com/blog/)
  + [RSS FEED](https://www.johndcook.com/blog/rss-feed/)
  + [TWITTER](https://www.johndcook.com/blog/twitter_page/)
  + [SUBSTACK](https://www.johndcook.com/blog/newsletter/)
  + [ARTICLES](https://www.johndcook.com/blog/articles/)
  + [TECH NOTES](https://www.johndcook.com/blog/notes/)
* [ABOUT](https://www.johndcook.com/blog/about-2/)
  + [CLIENTS](https://www.johndcook.com/blog/clients-new/)
  + [ENDORSEMENTS](https://www.johndcook.com/blog/endorsements/)
  + [TEAM](https://www.johndcook.com/blog/team/)
  + [SERVICES](https://www.johndcook.com/blog/services-2/)

(832) 422-8646

[Contact](https://www.johndcook.com/blog/contact/)

# How Lewis Carroll computed determinants

Posted on [10 July 2023](https://www.johndcook.com/blog/2023/07/10/lewis-carroll-determinants/ "10:53") by [John](https://www.johndcook.com/blog/author/john/ "View all posts by John")

![Mad hatter from Alice in Wonderland by Lewis Carroll](https://www.johndcook.com/madhatter.png)

Charles Dodgson, better known by his pen name Lewis Carroll, discovered a method of calculating determinants now known variously as the method of contractants, Dodgson condensation, or simply condensation.

The method was devised for ease of computation by hand, but it has features that make it a practical method for computation by machine.

## Overview

The basic idea is to repeatedly condense a matrix, replacing it by a matrix with one less row and one less column. Each element is replaced by the determinant of the 2×2 matrix formed by that element and its neighbors to the south, east, and southeast. The bottom row and rightmost column have no such neighbors and are removed. There is one additional part of the algorithm that will be easier to describe after introducing some notation.

## Details

Let *A* be the matrix whose determinant we want to compute and let *A*(*k*) be the matrix obtained after *k* steps of the condensation algorithm.

The matrix *A*(1) is computed as described in the overview:

![a^{(1)}_{ij} = a_{ij} a_{i+1, j+1} - a_{i, j+1} a_{i+1, j}.](https://www.johndcook.com/dodgson1.svg)

Starting with *A*(2) the terms are similar, except each 2×2 determinant is divided by an element from two steps back:

![a^{(k+1)}_{ij} = \frac{1}{a^{(k-1)}_{i+1,j+1}}\left(a^{(k)}_{ij} a^{(k)}_{i+1, j+1} - a^{(k)}_{i, j+1} a^{(k)}_{i+1, j} \right)](https://www.johndcook.com/dodgson2.svg)

[Dodgson’s original paper](https://www.gutenberg.org/files/37354/37354-pdf.pdf) from 1867 is quite readable, surprisingly so given that math notation and terminology changes over time.

One criticism I have of the paper is that it is hard to understand which element should be in the denominator, whether the subscripts should be *i* and *j* or *i*+1 and *j*+1. His first example doesn’t clarify this because these elements happen to be equal in the example.

## Example

Here’s an example using condensation to find the determinant of a 4×4 matrix.

![\begin{align*} A^{(0)} &= \begin{bmatrix} 3 & 1 & 4 & 1 \\ 5 & 9 & 2 & 6 \\ 0 & 7 & 1 & 0 \\ 2 & 0 & 2 & 3 \end{bmatrix} \\ A^{(1)} &= \begin{bmatrix} 22 &-34 & 22 \\ 35 & -5 &-6 \\ -14 & 14 & 3 \end{bmatrix} \\ A^{(2)} &= \begin{bmatrix} 120 & 157 \\ 60 & 69 \end{bmatrix} \\ A^{(3)} &= \begin{bmatrix} 228 \end{bmatrix} \end{align*}](https://www.johndcook.com/dodgson4.svg)

We can verify this with Mathematica:

```
    Det[{{3, 1, 4, 1}, {5, 9, 2, 6},
         {0, 7, 1, 0}, {2, 0, 2, 3}}]
```

which also produces 228.

## Division

The algorithm above involves a division and so we should avoid dividing by zero. Dodgson says to

> Arrange the given block, if necessary, so that no ciphers [zeros] occur in its interior. This may be done either by transposing rows or columns, or by adding to certain rows the several terms of other rows multiplied by certain multipliers.

He expands on this remark and gives examples. I’m not sure whether this preparation is necessary only to avoid division by zero, but it does avoid the problem of dividing by a zero.

If the original matrix has all integer entries, then the division in Dodgson’s condensation algorithm is exact. The sequence of matrices produced by the algorithm will all have integer entries.

## Efficiency

Students usually learn cofactor expansion as their first method of calculating determinants. This rule is easy to explain, but inefficient since the number of steps required is *O*(*n*!).

The more efficient way to compute determinants is by Gaussian elimination with partial pivoting. As with condensation, one must avoid dividing by zero, hence the partial pivoting.

Gaussian elimination takes *O*(*n*³) operations, and so does Dodgson’s condensation algorithm. Condensation is easy to teach and easy to carry out by hand, but unlike cofactor expansion it scales well.

If a matrix has all integer entries, Gaussian elimination can produce non-integer values in intermediate steps. Condensation does not. Also, condensation is inherently parallelizable: each of the 2 × 2 determinants can be calculated simultaneously.

## Related posts

* [Cofactors, determinants, and adjugates](https://www.johndcook.com/blog/2023/05/16/matrix-adjugate/)
* [Why determinants with a column of 1s?](https://www.johndcook.com/blog/2022/12/10/determinants-ones/)
* [Applied linear algebra](https://www.johndcook.com/blog/applied-linear-algebra/)

Categories : [Math](https://www.johndcook.com/blog/category/math/)

Tags : [Linear algebra](https://www.johndcook.com/blog/tag/linear-algebra/)

Bookmark the [permalink](https://www.johndcook.com/blog/2023/07/10/lewis-carroll-determinants/ "Permalink to How Lewis Carroll computed determinants")

# Post navigation

Previous Post[Gram matrix](https://www.johndcook.com/blog/2023/07/09/gram-matrix/)

Next Post[Visualizing a determinant identity](https://www.johndcook.com/blog/2023/07/10/visualizing-a-determinant-identity/)

## 3 thoughts on “How Lewis Carroll computed determinants”

1. Kevin Schoenecker

   [14 July 2023 at 12:44](https://www.johndcook.com/blog/2023/07/10/lewis-carroll-determinants/#comment-1157093)

   Thank you for such an interesting article! I have enjoyed reading your blog. I have one suggestion though. I think you meant to write Laplace expansion or cofactor expansion rather than Cramer’s Rule. Cramer’s rule is used to solve systems of equations with n variables and n equations, which have a unique answer.
2. Stuart Brorson

   [14 July 2023 at 20:52](https://www.johndcook.com/blog/2023/07/10/lewis-carroll-determinants/#comment-1157125)

   This smells like the technique called “deflation” when computing eigenvalues using e.g. the QR algo. In deflation the goal is to eliminate the matrix element at the bottom left corner before proceeding to process the rest of the matrix. Is there any deep connection here, or just a superficial analogy?
3. Stuart Brorson

   [15 July 2023 at 06:51](https://www.johndcook.com/blog/2023/07/10/lewis-carroll-determinants/#comment-1157161)

   Doh! Reading my comment about deflation … I made a mistake. Deflation removes the matrix element on the lower right, not the lower left.

Comments are closed.

Search for:

[![John D. Cook](//www.johndcook.com/jdc_20170630.jpg)](//www.johndcook.com)

**John D. Cook, PhD**

My colleagues and I have decades of consulting experience helping companies solve complex problems involving [data privacy](https://www.johndcook.com/blog/expert-hipaa-deidentification/), [applied math](https://www.johndcook.com/blog/applied-math/), and [statistics](https://www.johndcook.com/blog/applied-statistics/).

Let’s talk. We look forward to exploring the opportunity to help your company too.

### [John D. Cook](https://www.johndcook.com/blog/)

© All rights reserved.

Search for:

(832) 422-8646

[EMAIL](//www.johndcook.com/blog/contact/)
