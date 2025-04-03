# Modular Python basics

## Introduction and motivation

The goal of this session is to provide you with the basic tools to organize the code you develop during your internship. You will deliver this code in a GitHub repository that will document your project.

By following the advice given today, you will make your results more robust (by minimizing the risk of errors) and increase their visibility (by maximizing the chances that your work can be reused later).

## Functions
Here's a Python function:

```python
def get_data(file, column, num_rows):
    table = pd.read_csv(file, nrows=num_rows)
    data = table[column]
    return data
```

It has:
- A header, called "signature", which gives the function's name and parameters.
- A function body, which performs the processing.
- Optionally, a function return to get the output.

A function breaks down code into smaller parts, making it less complex, more reusable and more reliable.

> [!TIP]
> - build your code around functions
> - break down your code into more functions if, too indented, too long, too complex, too weird
> - use [pure functions](https://cicero.xyz/v3/remark/0.14.0/github.com/coderefinery/modular-code-development/master/talk.md/#8), i.e. functions that don't keep a state between executions and always return the same output with the same input
> - avoid premature optimization, simplicity and clarity first and foremost
> - simple is better than complex

## Modules (sometimes called "scripts")
A Python module is a simple file .py which contains collection of functions. It behaves like a Python library, but must be imported from a local source.

Grouping functions around themes or similiarities in modules allows them to be reused even more widely in different parts of your project. This ensures the consistency of operations performed in different places.

> [!TIP]
> - a module is separate from its use, so it's even more important to document it properly
> - expose the "what", hide the "how"
> - only include pure functions in a module
> - like functions, creating several modules is better than one big one
> - group functions that work together or by theme

## Packages (often called "libraries")
A package is a python module published on the Internet in a package repository, such as PyPI or Conda. This makes it easy to share with anyone, and especially between several projects. It contains at least an empty `__init__.py` file to declare that the module is a real package.

> [!TIP]
> - keep the `__init__.py` file minimal, an empty one is perfectly fine
> - documenting is not an option anymore (and never was)
> - it's probably time to test the functions
> - if optimization of functions is nedded, measure, do not guess

## Daily workflow

As a start, we recommand that you start storing your module in your *labbook* GitHub repo and share them with your collaborators in your *collaboration* GitHub repo. By the end of the project these tools will be delivered in the *project* GitHub repo. We will discuss later how to do this optimally.  

## Interactive Session 5 part
[A demonstration about how to improve incrementally a Python code](modular-python-demo.md)

## Conclusions and future perspectives

Monsieur Jourdain, in Molière’s Le Bourgeois Gentilhomme, was unknowingly writing prose. During your internship so far, you have been practicing software design without realizing it.

In this session, we have defined the key concepts and practical steps that will allow you to build robust tools throughout your internship, improving the reproducibility of your results.

By following this approach, you will deliver a set of notebooks alongside your project, based on libraries that will gather the functions you have developed.

Later, we will explore how to make these libraries reusable beyond your project.

## Acknowledgment and further materials
- https://cicero.xyz/v3/remark/0.14.0/github.com/coderefinery/modular-code-development/master/talk.md/
- https://coderefinery.github.io/modular-type-along/
