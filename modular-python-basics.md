# Modular Python basics

## Introduction and motivation

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
> - break down your code into more functions
>   - if you have too many indentation levels
>   - if a function becomes too long
>   - if a function does more than one thing
>   - if you have trouble naming a function
> - use pure functions, i.e. functions that have no "state" and always return the same value with the same input
> - avoid premature optimization, simplicity and clarity first and foremost
> - simple is better than complex

## Scripts
A Python script is a simple file .py which contains collection of functions. It behaves like a Python library, but must be imported from a local source.

Grouping functions around themes or similiarities in scripts allows them to be reused even more widely in different parts of your project. This ensures the consistency of operations performed in different places.

> [!TIP]
> - a script is separate from its use, so it's even more important to document it properly
> - expose the "what", hide the "how"
> - only include pure functions in a script
> - like functions, creating several scripts is better than one big one
> - group functions that work together or by theme

## Package or library
A library is a python script published on the Internet in a package repository, such as PyPI or Conda. This makes it easy to share with anyone, and especially between several projects. 

> [!TIP]
> - documenting is not an option anymore (and never was)
> - it's probably time to test the functions
> - if optimization of functions is nedded, measure, do not guess

## Daily routines

## Interactive demonstration
[It's this way.](modular-python-demo.md)

## Conclusions and future perspectives

## Acknowledgment and further materials
- https://cicero.xyz/v3/remark/0.14.0/github.com/coderefinery/modular-code-development/master/talk.md/
- https://coderefinery.github.io/modular-type-along/
