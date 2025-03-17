# Documenting software

Although it is difficult to pin down who first said _"code is read more often than it is written"_, they were probably right.

Writing adequate documentation for your software will help yourself, other contributors, and users of your code. Some documentation tools (such as parameter annotation) can even help you prevent bugs.

Below is one attempt at identifying different levels of achievement in documenting code:

 1. Modular code with self-documenting names for variables, functions, ...

 2. Good comments and docstrings.

 3. Python annotations.

 4. Well-formatted documentation, using tools such as:

    - [Sphynx](https://www.sphinx-doc.org/en/master/)

    - [MkDocs](https://www.mkdocs.org/)

    - [Jupyter Book](https://jupyterbook.org/en/stable/intro.html)

 5. Standardized docstrings and automatic documentation (with the tools listed above)

 6. Automatic deployment with tools such as [Read the Docs](https://about.readthedocs.com/)

For M2 internships, levels 1 and 2 are expected of all. Levels 3 through 5 are mostly relevant if you develop an
external library containing more than a few functions. Level 6 is for wide-scale deployment.
