# Documenting software

## Introduction

Although it is difficult to pin down who first said _"code is read more often than it is written"_, they were probably right.

Writing adequate documentation for your software will help yourself, other contributors, and users of your code. Some documentation tools (such as parameter annotation) can even help you prevent bugs.

Below is one attempt at identifying different levels of achievement in documenting code:

 1. Self-documenting code (good names for variables, functions, etc.)

 2. Comments and docstrings.

 3. Python annotations.

 4. Well-formatted documentation, using tools such as:

    - [Sphynx](https://www.sphinx-doc.org/en/master/)

    - [MkDocs](https://www.mkdocs.org/)

    - [Jupyter Book](https://jupyterbook.org/en/stable/intro.html)

 5. Standardized docstrings and automatic documentation (with the tools listed above)

 6. Automatic deployment with tools such as [Read the Docs](https://about.readthedocs.com/)

For M2 internships, levels 1 and 2 are expected of all. Levels 3 through 5 are mostly relevant if you develop an external library containing more than a few functions. Level 6 is for wide-scale deployment.

## Self documenting-code

Consider this short Python function:

```Python
def fct(a, b):
    v1 = a.mean()
    v2 = b.mean()
    x = ((a - v1) * (b - v2)).sum() / ((a - v1)**2).sum()
    y = v2 - x * v1
    return x, y
```

What does this function do? It is difficult to known without squinting at the inner code for some time. Let us re-write it, just by changing some of the names:

```Python
def linear_regression(x, y):
    xmean = x.mean()
    ymean = y.mean()
    slope = ((x - xmean) * (y - ymean)).sum() / ((x - xmean)**2).sum()
    intercept = ymean - slope * xmean
    return slope, intercept
```

It is now quite obvious that this function calculates the coefficients of the linear regression `y = slope * x + intercept`. The code is exactly the same as before as far as the Python interpreter is concerned, but now the function is self-documenting as far as humans are concerned. There is no golden or unique rule for chosing good names in your code, but here is some advice:

 - Use names that are descriptive and short, but avoid ambiguity. The [FORTRAN style guide](https://fortran-lang.org/learn/best_practices/style_guide/) give a good example:

> "spline interpolation" can be shortened to `spline_interpolation`, `spline_interpolate`, `spline_interp`, `spline`, but not to `splineint` ("int" could mean integration, integer, etc. - too much ambiguity, even in the clear context of a computational code). This is in contrast to `get_argument()` where `getarg()` is perfectly clean and clear.

 - Follow the conventions used by the community. For example, in Python, CamelCase is used for names of classes while functions, variables, and class instances use lower-case names, with underscores when necessary.

## Comments and docstrings
