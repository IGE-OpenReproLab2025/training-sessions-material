# Python environmnents 

## What we will cover 
 - how to install new packages 
 - what are environments and why we need them
 - the environment available on our Jupyterhub
 - how to customize your environment
 - documenting you environment in Jupyter notebook

## How to install new packages 
(JLS)

## What are environments and why we need them
(JLS)

## The environment available on our Jupyterhub
Managing software environments can sometimes become complex and confusing. To avoid recurring problems, use a single dependency manager. This applies to all languages!

So don't mix `pip` commands with `conda` or `pipx` commands. The only exception are `conda` and `mamba` commands which are interchangeable. Think of them as a single command, `mamba` is simply faster.

> [!IMPORTANT]
> Here we'll use [**Mamba**](https://mamba.readthedocs.io) with the `mamba`command. 

**Mamba** (or similarly **Conda**) is a very complete package manager, allowing you to install not only **Python** packages, but also system packages and other languages such as **R**. That's why, in practice, **Mamba** is enough to handle any situation you may encounter.

By default, a **Mamba** environment is installed in your online **JupyterLab**. This is a **Pangeo** environment known simply as "_notebook_". **Pangeo** is a community of geoscientists who have listed the most commonly used packages in **Python**.

> [!NOTE]
> Run the `mamba list` command to obtain a list of the packages **in the environment you're using**.

Every terminal you open in **JupyterLab** automatically loads this "_notebook_" environment by default (this behavior may change if you tinker). So if you're lost, just relaunch your terminal.

> [!NOTE]
> Run the `mamba info` command to learn more **about your currently loaded environment**.

When you open a notebook via the **JupyterLab** launcher, there is a choice available: "_Python 3 (ipykernel)_" kernel (see below).

<p align=center><img width="127" alt="Capture d’écran 2025-03-26 à 16 24 26" src="https://github.com/user-attachments/assets/1e7e4fb0-03eb-4def-a972-55fae9918d0d" /></p>

A "_kernel_" is a **Mamba** environment ready to be used in a notebook. Here, the _"Python 3 (ipykernel)"_ kernel corresponds to the **Pangeo** "_notebook_" environment.

> [!WARNING]
> Each terminal can load a different **Mamba** environment and a notebook kernel is not linked to any terminal environment. These are different contexts which must be configured separately.

## How to customize your environment

Example 1: you need a Python library whose documentation recommends using “pip install my-lib”? There's a very good chance that “mamba install my-lib” will install the same package, or perhaps “mamba install my_lib” - check it out on the Web!

Example 2: You need the “curl” system command? Then “mamba install curl” will let you use it in a terminal! No need to run “apt install curl”.

## Documenting your environment in jupyter notebook
(AA)
