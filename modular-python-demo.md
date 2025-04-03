# Demonstration: Towards modular Python code

## Objective
Start with functional code and apply incremental improvement to obtain modular Python code.

## Before we start
This document describes the main stages the demonstration will go through and the functional states of the code for each. It serves as a framework for the presenter and as a reminder for the students afterwards.

Ideally, you should copy the first code snippet below and copy the “temperatures.csv” data required for its execution. Then, this starting example can be improved by going through the steps below, highlighting the interest of each change described below.

There is also a complete git repository to track each workspace state with tags: https://github.com/IGE-OpenReproLab2025/modular-python-code-demo

## The Eight Quantum Leaps
1. Getting started with a functional notebook
2. Make a few unoptimal additions
3. Avoiding duplication with a loop
4. Abstract code into a function
5. Add more functions
6. Transform stateful functions into pure functions
7. Move functions into a module
8. Upgrade the module to a package

## v1 Getting started with a functional notebook
We imagine that we assemble a working script from various StackOverflow recommendations and arrive at:

```python
import pandas as pd
from matplotlib import pyplot as plt

num_measurements = 25

# Read data from file
data = pd.read_csv("temperatures.csv", nrows=num_measurements)
temperatures = data["Air temperature (degC)"]

# Compute statistics
mean = sum(temperatures) / num_measurements

# Plotting
plt.plot(temperatures, "r-")
plt.axhline(y=mean, color="b", linestyle="--")
plt.show()
plt.savefig("25.png")
plt.clf()
```

## v2 Make a few unoptimal functional additions
New phase of adding features to the code: we also want a plot for 100 measurements and add a legend. Let's do this naively and see what pitfalls we can already fall into.

```python
import pandas as pd
from matplotlib import pyplot as plt

num_measurements = 25

# Read data from file
data = pd.read_csv("temperatures.csv", nrows=num_measurements)
temperatures = data["Air temperature (degC)"]

# Compute statistics
mean = sum(temperatures) / num_measurements

# Plotting
plt.plot(temperatures, "r-")
plt.axhline(y=mean, color="b", linestyle="--")
plt.show()
plt.savefig("25.png")
plt.clf()

num_measurements = 100

# Read data from file
data = pd.read_csv("temperatures.csv", nrows=num_measurements)
temperatures = data["Air temperature (degC)"]

# Compute statistics
mean = sum(temperatures) / num_measurements

# Plotting
plt.plot(temperatures, "r-")
plt.axhline(y=mean, color="b", linestyle="--")
plt.show()
plt.savefig("100.png")
plt.clf()
```

Problems:
- The code has been duplicated, which is a bad sign if we want to add more plots.
- We also need to remember to change the “num_measurements” variable and change the plot output name each time.
- (bonus) The input file is reread each time a new plot is made, wasting computing resources and adding time processing.

## v3 Avoiding duplication with a loop
Let's add a for loop to at least avoid code duplication and format the name of the output file.

```python
import pandas as pd
from matplotlib import pyplot as plt

for num_measurements in [25, 100, 500]:
    # Read data from file
    data = pd.read_csv("temperatures.csv", nrows=num_measurements)
    temperatures = data["Air temperature (degC)"]
    
    # Compute statistics
    mean = sum(temperatures) / num_measurements
    
    # Plotting
    plt.plot(temperatures, "r-")
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(f"{num_measurements}.png")
    plt.clf()
```

## v4 Abstracting the plotting part into a function
If we want to compare plots with different processing, we'll have to copy and paste the whole loop, and end up duplicating code again. Let's avoid this by adding a new layer of abstraction to this code with a function that will handle only the plotting part.

```python
import pandas as pd
from matplotlib import pyplot as plt

def plot_temperatures(temperatures):
    plt.xlabel("measurements")
    plt.ylabel("air temperature (deg C)")
    plt.plot(temperatures, "r-")
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(f"{num_measurements}.png")
    plt.clf()

for num_measurements in [25, 100, 500]:
    # Read data from file
    data = pd.read_csv("temperatures.csv", nrows=num_measurements)
    temperatures = data["Air temperature (degC)"]
    
    # Compute statistics
    mean = sum(temperatures) / num_measurements
    
    plot_temperatures(temperatures)
```

Can you also see that this way you'll be sure to apply the same treatment every time you reuse this function somewhere?

## v5 Add more functions
Let's transform all processing operations into functions.

```python
import pandas as pd
from matplotlib import pyplot as plt

def plot_temperatures(temperatures):
    plt.xlabel("measurements")
    plt.ylabel("air temperature (deg C)")
    plt.plot(temperatures, "r-")
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(f"{num_measurements}.png")
    plt.clf()

def get_data(file, column):
    data = pd.read_csv(file, nrows=num_measurements)
    return data[column]

def compute_mean(data):
    return sum(data) / num_measurements

for num_measurements in [25, 100, 500]:
    temperatures = get_data("temperatures.csv", "Air temperature (degC)")
    
    mean = compute_mean(temperatures)
    
    plot_temperatures(temperatures)
```

A quick glance at the "processing cell is all that's needed to know what the processing steps are, which is very clear to any reader.

But these functions are “stateful”, i.e. they have side effects. They use variables that are outside their definition (such as “mean” and “num_measurements”, for example). It's a problem if, for example, we reuse one of these functions outside this notebook.

## v6 Transform stateful functions into pure functions
"Pure" (or "Stateless") functions depends ONLY on their input parameters. Thanks to that, they can be reused anywhere, tested, parallelized and much more. Let's refactor these functions to make them pure (and a bit more generic by the way).

```python
import pandas as pd
from matplotlib import pyplot as plt

def plot_data(data, mean, xlabel, ylabel, output_plot_file):
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.plot(data, "r-")
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(output_plot_file)
    plt.clf()

def get_data(file, column, num_rows):
    data = pd.read_csv(file, nrows=num_rows)
    return data[column]

def compute_mean(data):
    return sum(data) / len(data)

for num_measurements in [25, 100, 500]:
    temperatures = get_data(
        file="temperatures.csv",
        column="Air temperature (degC)",
        num_rows=num_measurements
    )
    
    temperature_mean = compute_mean(temperatures)
    
    plot_data(
        data=temperatures,
        mean=temperature_mean,
        xlabel="measurements",
        ylabel="Air temperature (deg C)",
        output_plot_file=f"{plots_folder}{num_measurements}.png"
    )
```

These functions can now be copy-pasted to a different notebook or project and they will still work.

## v7 Move functions into a module

By staying in a notebook, a function can't be reused elsewhere without being copy-pasted. Because with several notebooks, we want to produce comparable results, we also want a single definition of our functions for the whole project.

We now want a new Python file called `data_processing.py` in which we will move all our fonction definitions and their dependencies.

```python
import pandas as pd
from matplotlib import pyplot as plt

def plot_data(data, mean, xlabel, ylabel, output_plot_file):
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.plot(data, "r-")
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(output_plot_file)
    plt.clf()

def get_data(file, column, num_rows):
    data = pd.read_csv(file, nrows=num_rows)
    return data[column]

def compute_mean(data):
    return sum(data) / len(data)
```

It behaves and handles almost like a classic mamba/conda dependency. So we're going to greatly simplify the temperature plotting notebook.

```python
import data_processing as dapro

for num_measurements in [25, 100, 500]:
    temperatures = dapro.get_data(
        file="temperatures.csv",
        column="Air temperature (degC)",
        num_rows=num_measurements
    )
    
    temperature_mean = dapro.compute_mean(temperatures)
    
    dapro.plot_data(
        data=temperatures,
        mean=temperature_mean,
        xlabel="measurements",
        ylabel="Air temperature (deg C)",
        output_plot_file=f"{num_measurements}.png"
    )
```

The notebook now contains only the very high-level instructions that are sufficient to understand the general idea of the treatment applied to them. Separated in this way, both your functions and your notebook are much clearer, and reuse is greatly simplified.

## v8 Upgrade the module to a package

Sometimes a python module can be found elsewhere on the system, like in a "modules" folder. Let's see how to import them if they're not in the same folder as your notebook. Move your `data_processing.py` into a new subfolder called `modules` and create an empty `__init__.py` file within too.

```python
import sys
import modules.data_processing as dapro

for num_measurements in [25, 100, 500]:
    temperatures = dapro.get_data(
        file="temperatures.csv",
        column="Air temperature (degC)",
        num_rows=num_measurements
    )
    
    temperature_mean = dapro.compute_mean(temperatures)
    
    dapro.plot_data(
        data=temperatures,
        mean=temperature_mean,
        xlabel="measurements",
        ylabel="Air temperature (deg C)",
        output_plot_file=f"{num_measurements}.png"
    )
```

From now on, if several of our notebooks need these functions, they are all defined here.
- We can add parameters to make them more generic (i.e. able to handle more situations).
- If they become too long, it's best to cut them up and simplify them into smaller parts.
- If the module becomes too long, it may be a good idea to add others and divide up the functions according to usage categories.

## Conclusion
What we've just done is no longer just called “programming” but “developing”. It's about giving your code the right software structure to give it the essential quality that drives all the others: maintainability. A maintainable code is simple, manageable and adaptable. It is then reused, improved and can thus become increasingly efficient, generic and reliable.

## Perspectives
Now that our functions are well separated, there are lots of other things we can do to improve them:
- modules, can be divided into theme-based submodules and enhanced
- packages, publish them to package repositories like PyPI or Conda and install them anywhere easily with `mamba install`.
- tests, to make code more reliable and resistant to modifications, perfect for improving it without breaking anything
- documentation, like docstrings and typing to make code more understandable and accessible

## Acknowledgements
This work is largely inspired by CodeRefinery: https://github.com/coderefinery/modular-type-along
