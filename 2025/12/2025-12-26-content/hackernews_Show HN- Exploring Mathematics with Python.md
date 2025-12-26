---
source: hackernews
title: Show HN: Exploring Mathematics with Python
url: https://coe.psu.ac.th/ad/explore/
date: 2025-12-26
---

# Exploring Mathematics with Python

## Arthur Engel Andrew Davison

```

```

!['Exploring Mathematics with your Computer' Cover](nml-35-cover.jpg)The heart of this book is Arthur Engel's 1993 textbook [*Exploring Mathematics with your Computer*](https://bookstore.ams.org/NML/35) which utilizes Turbo Pascal for its exploratory work, and is still available for purchase. The first six chapters (and Appendix A) are essentially that book, but with the programming language changed to Python, some rewording, reformatting in Latex, and a few additions. Those chapters masterfully cover over 60 topics, that are mostly independent of each other.

Sadly, Engel passed away in 2022, and so the revisions were carried out by the second author (Andrew Davison). I'm also responsible for the new chapters, a potpourri of math topics, starting with fractals in chapter 7.

Engel rightly claimed that his book was a mathematics, not a programming, text. However, one of the shifts in programming since the 1990's is the appearance of a vast number of math coding libraries. For example, in Python we have Matplotlib, Numpy, SciPy, Sympy, and many more. In addition, hardware has advanced so much that an average laptop is certain to have multiple CPUs, a GPU, and gigabytes of RAM at its disposal.

Taking advantage of this increased functionality requires a deeper knowledge of computing than was necessary back in the 1990's. As a result, I would classify this version of the book as a mathematics *and* programming text.

The increased complexity means that this book is probably most suitable for readers with a year's exposure to maths and programming at the university level. However, I hope that much of the material is understandable by advanced high school students who have studied additional maths and been introduced to Python. This latter point should be emphasized – I assume that the reader already knows Python; this is not a 'Learn Python' book, although I've included a short refresher appendix.

One of my aims is to try to retain Engel's emphasis on maths by keeping the programming relatively simple, so that even someone with a short introduction to Python should be able to follow (most of) the code. As a consequence, I've deliberately excluded advanced scientific Python modules such as Numpy, SciPy, and Sympy. However, I've relented over charts, and made extensive use of Matplotlib (which is introduced in its own appendix). The other visualization library that's heavily represented is Python's turtle module, which most students will have met during their introduction to Python, but there's an appendix on it if you need to brush up. Nevertheless, you will encounter some tricky programming topics: recursion, Python list comprehensions, generators, classes, higher-order functions, and references to big-Oh running times. They're not a major element, but they do crop up.

One issue that I've been uncertain about is the use of "I" versus "We". This book would have been impossible without Engel's inspiration, and the first six chapters are his. However, should I use "We" for the later chapters which haven't benefited from input by Engel? As you'll see, I've gone with "We", but any errors or poorly explained material are solely the responsibility of Andrew Davison.

I would like to thank the Mathematical Association of America (MAA) for permission to update Engel's book, and my regards also to Ralf Engel, Arthur's son, for his encouragement.

Unfortunately, after lengthy discussions with the MAA, my hopes of publishing this (rather large) expansion have proved impossible, and so I've decided to put it online, hopefully to be of use to others.

```

```

## Contents ([pdf](pdfs/contents.pdf))

* 1. Introductory Problems:
  [PDF](pdfs/intro.pdf);
  [code directory](http://coe.psu.ac.th/~ad/explore/code/01_Intro/); [zipped code](code/zips/01_Intro.zip)

  * 2. Algorithms in Number Theory:
    [PDF](pdfs/numTheory.pdf);
    [code directory](http://coe.psu.ac.th/~ad/explore/code/02_NumTheory/); [zipped code](code/zips/02_NumTheory.zip)

    * 3. Probability:
      [PDF](pdfs/probability.pdf);
      [code directory](http://coe.psu.ac.th/~ad/explore/code/03_Prob/); [zipped code](code/zips/03_Prob.zip)

      * 4. Statistics:
        [PDF](pdfs/stats.pdf);
        [code directory](http://coe.psu.ac.th/~ad/explore/code/04_Stats/); [zipped code](code/zips/04_Stats.zip)

        * 5. Combinatorial Algorithms:
          [PDF](pdfs/combs.pdf);
          [code directory](http://coe.psu.ac.th/~ad/explore/code/05_Combs/); [zipped code](code/zips/05_Combs.zip)

          * 6. Numerical Algorithms:
            [PDF](pdfs/numerical.pdf);
            [code directory](http://coe.psu.ac.th/~ad/explore/code/06_Numer/); [zipped code](code/zips/06_Numer.zip)

            * 7. Fractals
              [PDF](pdfs/fractals.pdf);
              [code directory](http://coe.psu.ac.th/~ad/explore/code/Fractals/); [zipped code](code/zips/Fractals.zip)

              * 8. Chaos
                [PDF](pdfs/chaos.pdf);
                [code directory](http://coe.psu.ac.th/~ad/explore/code/Chaos/); [zipped code](code/zips/Chaos.zip)

                * 9. Computer Graphics
                  [PDF](pdfs/compGrap.pdf);
                  [code directory](http://coe.psu.ac.th/~ad/explore/code/CompGrap/); [zipped code](code/zips/CompGrap.zip)

                  * 10. Computational Geometry
                    [PDF](pdfs/compGeometry.pdf);
                    [code directory](http://coe.psu.ac.th/~ad/explore/code/CompGeometry/); [zipped code](code/zips/CompGeometry.zip)

                    * 11. Continued Fractions
                      [PDF](pdfs/continuedFracs.pdf);
                      [code directory](http://coe.psu.ac.th/~ad/explore/code/ContinuedFracs/); [zipped code](code/zips/ContinuedFracs.zip)

                      * 12. Markov Chains
                        [PDF](pdfs/markov.pdf);
                        [code directory](http://coe.psu.ac.th/~ad/explore/code/Markov/); [zipped code](code/zips/Markov.zip)

                        * 13. Navigating the Earth
                          [PDF](pdfs/navigate.pdf);
                          [code directory](http://coe.psu.ac.th/~ad/explore/code/navigate/); [zipped code](code/zips/navigate.zip)

                          * 14. Quaternions
                            [PDF](pdfs/quat.pdf);
                            [code directory](http://coe.psu.ac.th/~ad/explore/code/quat/); [zipped code](code/zips/quat.zip)

                            * 15. Curve Fitting
                              [PDF](pdfs/curveFit.pdf);
                              [code directory](http://coe.psu.ac.th/~ad/explore/code/curveFit/); [zipped code](code/zips/curveFit.zip)

                              * 16. Conic Sections
                                [PDF](pdfs/conics.pdf);
                                [code directory](http://coe.psu.ac.th/~ad/explore/code/conics/); [zipped code](code/zips/conics.zip)

                                * 17. Solving Cubic Equations
                                  [PDF](pdfs/cubic.pdf);
                                  [code directory](http://coe.psu.ac.th/~ad/explore/code/cubic/); [zipped code](code/zips/cubic.zip)

                                  * 18. The Catenary
                                    [PDF](pdfs/catenary.pdf);
                                    [code directory](http://coe.psu.ac.th/~ad/explore/code/catenary/); [zipped code](code/zips/catenary.zip)

                                    * Bibliography: [PDF](pdfs/bib.pdf);

## Exercises

We have included solutions to many of the exercises. A PDF version
can be found in Appendix B below. Code solutions for section/subsection
exercises can be found in the zip files for their chapters (see above). However,
solutions to the "Addtional Exercises" which cover multiple chapters
are in their own [zip file](code/zips/ExChaps.zip).

## Appendices

The zipped code used in appendices D (Python), E (Numbers),
F (Matplotlib), G (Turtle), H (Graphviz), and I (Matrices), J (Complex Numbers) is
[here](code/zips/apps.zip).

* A) A Crash Course in Probability:
  [PDF](pdfs/A_Crash_Prob.pdf);

  * B) Solutions to Some Exercises:
    [PDF](pdfs/B_Solns_Exs.pdf)

    * C) Summary of Notation and Formulas:
      [PDF](pdfs/C_Notation.pdf)

      * D) A Summary of Python:
        [PDF](pdfs/D_Python.pdf);
        [code directory](http://coe.psu.ac.th/~ad/explore/code/D_Python/)

        * E) Numbers in Python:
          [PDF](pdfs/E_Numbers.pdf);
          [code directory](http://coe.psu.ac.th/~ad/explore/code/E_Nums/)

          * F) Matplotlib:
            [PDF](pdfs/F_Matplotlib.pdf);
            [code directory](http://coe.psu.ac.th/~ad/explore/code/F_Matplotlib/)

            * G) Turtle Graphics:
              [PDF](pdfs/G_Turtles.pdf);
              [code directory](http://coe.psu.ac.th/~ad/explore/code/G_Turtles/)

              * H) Graphviz:
                [PDF](pdfs/H_Graphviz.pdf);
                [code directory](http://coe.psu.ac.th/~ad/explore/code/H_Graphviz/)

                * I) Matrices:
                  [PDF](pdfs/I_Matrices.pdf);
                  [code directory](http://coe.psu.ac.th/~ad/explore/code/I_Matrices/)

                  * J) Complex Numbers:
                    [PDF](pdfs/J_Complex.pdf);
                    [code directory](http://coe.psu.ac.th/~ad/explore/code/J_Complex/)

All the book's code as a
[single zipped file](code.zip) (1.12 MB).

```

```

---

Dr. Andrew Davison
E-mail: ad@coe.psu.ac.th
Back to [my home page](../index.html)
