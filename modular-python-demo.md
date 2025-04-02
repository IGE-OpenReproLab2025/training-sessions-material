# Demonstration: Towards modular Python code

## Objective
Start with functional code and apply incremental improvement to obtain modular Python code.

## Before we start
This document describes the main stages the demonstration will go through and the functional states of the code for each. It serves as a framework for the presenter and as a reminder for the students afterwards.

Ideally, you should copy the first code snippet below and copy the “temperatures.csv” data required for its execution. Then, this starting example can be improved by going through the steps below, highlighting the interest of each change described below.

## The Nine Quantum Leaps
1. Getting started with a functional notebook
2. Break down code to make it more readable
3. Make a few unoptimal functional additions
4. Abstract code into a function
5. Add more functions
6. Transform stateful functions into pure functions
7. Move functions into a script
8. Move script in subfolder
9. Add a test

## Getting started with a functional notebook

We imagine that we assemble a working script from various StackOverflow recommendations and arrive at:

```python
import pandas as pd
from matplotlib import pyplot as plt

num_measurements = 25
data = pd.read_csv("temperatures.csv", nrows=num_measurements)
temperatures = data["Air temperature (degC)"]
mean = sum(temperatures) / num_measurements

plt.plot(temperatures, "r-")
plt.axhline(y=mean, color="b", linestyle="--")
plt.show()
plt.savefig("temperatures.png")
plt.clf()
```

## Break down code to make it more readable
Let's tidy things up a bit and divide the code into separate parts: imports at the top, variables next and finally a few commented code cells. Also move input data and output plots into separate folders.

```python
# Imports
import pandas as pd
from matplotlib import pyplot as plt
```

```python
# Variables
dataset_file = "data/temperatures.csv"
data_column_name = "Air temperature (degC)"
plots_folder = "plots"
num_measurements = 25
```

```python
# Processing
data = pd.read_csv(dataset_file, nrows=num_measurements)
temperatures = data[data_column_name]
mean = sum(temperatures) / num_measurements
```

```python
# Plotting
plt.plot(temperatures, "r-")
plt.axhline(y=mean, color="b", linestyle="--")
plt.show()
output_plot = os.path.join(plots_folder, f"{num_measurements}.png"
plt.savefig(output_plot)
plt.clf()
```

## Make a few unoptimal functional additions
It's not the best placement but it works and later it will bite us (only the first plot will have labels) and we will improve it:

```diff
import pandas as pd
from matplotlib import pyplot as plt

plt.xlabel("measurements")
plt.ylabel("air temperature (deg C)")

num_measurements = 25

# read data from file
data = pd.read_csv("temperatures.csv", nrows=num_measurements)
temperatures = data["Air temperature (degC)"]

# compute statistics
mean = sum(temperatures) / num_measurements

# plot results
plt.plot(temperatures, "r-")
plt.axhline(y=mean, color="b", linestyle="--")
plt.show()
plt.savefig(os.path.join(plots_folder, f"{num_measurements}.png")
plt.clf()
```

Once we get this working for 25 measurements, our task changes to also plot the first 100 and the first 500 measurements in two additional plots.

## Plotting also 100 and 500 measurements

- Next idea is perhaps code duplication.
- Then a for-loop to iterate over `[25, 100, 500]`:

```python
import pandas as pd
from matplotlib import pyplot as plt

plt.xlabel("measurements")
plt.ylabel("air temperature (deg C)")

for num_measurements in [25, 100, 500]:

    # read data from file
    data = pd.read_csv("temperatures.csv", nrows=num_measurements)
    temperatures = data["Air temperature (degC)"]

    # compute statistics
    mean = sum(temperatures) / num_measurements

    # plot results
    plt.plot(temperatures, "r-")
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(f"{num_measurements}.png")
    plt.clf()
```


## Abstracting the plotting part into a function

```python
import pandas as pd
from matplotlib import pyplot as plt

plt.xlabel("measurements")
plt.ylabel("air temperature (deg C)")


def plot_temperatures(temperatures):
    plt.plot(temperatures, "r-")
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(f"{num_measurements}.png")
    plt.clf()


for num_measurements in [25, 100, 500]:

    # read data from file
    data = pd.read_csv("temperatures.csv", nrows=num_measurements)
    temperatures = data["Air temperature (degC)"]

    # compute statistics
    mean = sum(temperatures) / num_measurements

    # plot results
    #   plt.plot(temperatures, 'r-')
    #   plt.axhline(y=mean, color='b', linestyle='--')
    #   plt.show()
    #   plt.savefig(f'{num_measurements}.png')
    #   plt.clf()
    plot_temperatures(temperatures)
```

- Discuss what we expect before running it (some will expect this not to work because variables seem undefined).
- Then try it out (it actually works).
- Discuss problems with this solution (what if we copy-paste the function to a different file?).

The point of this step was that abstracting code into functions can be really good for reusability but just the fact that we created a function does not mean that the function is reusable since in this case it depends on a variable defined outside the function and hence there are side-effects.

## Small improvements

- Abstracting into more functions.
- Notice how the comments got redundant:

```python
import pandas as pd
from matplotlib import pyplot as plt


def plot_data(data, xlabel, ylabel):
    plt.plot(data, "r-")
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(f"{num_measurements}.png")
    plt.clf()


def compute_statistics(data):
    mean = sum(data) / num_measurements
    return mean


def read_data(file_name, column):
    data = pd.read_csv(file_name, nrows=num_measurements)
    return data[column]


for num_measurements in [25, 100, 500]:

    temperatures = read_data(
        file_name="temperatures.csv", column="Air temperature (degC)"
    )

    mean = compute_statistics(temperatures)

    plot_data(
        data=temperatures, xlabel="measurements", ylabel="air temperature (deg C)"
    )
```


Discuss what would happen if we copy-paste the functions to another project (these functions are stateful/time-dependent).

Emphasize how stateful functions and order of execution in Jupyter notebooks can produce unexpected results and explain why we motivate to rerun all cells before sharing the notebook.

## Towards functions without side-effects

Improve to more stateless functions:

```python
import pandas as pd
from matplotlib import pyplot as plt


def plot_data(data, mean, xlabel, ylabel, file_name):
    plt.plot(data, "r-")
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.axhline(y=mean, color="b", linestyle="--")
    plt.show()
    plt.savefig(file_name)
    plt.clf()


def compute_mean(data):
    mean = sum(data) / len(data)
    return mean


def read_data(file_name, nrows, column):
    data = pd.read_csv(file_name, nrows=nrows)
    return data[column]


for num_measurements in [25, 100, 500]:

    temperatures = read_data(
        file_name="temperatures.csv",
        nrows=num_measurements,
        column="Air temperature (degC)",
    )

    mean = compute_mean(temperatures)

    plot_data(
        data=temperatures,
        mean=mean,
        xlabel="measurements",
        ylabel="air temperature (deg C)",
        file_name=f"{num_measurements}.png",
    )
```

These functions can now be copy-pasted to a different notebook or project and they will still work.

## Move from notebook to script

Adding unit tests is often the moment when notebook is not the right fit anymore.

But before we add tests:

- "File" -> "Save and Export Notebook As ..." -> "Executable Script"
- `git init` and commit the working version.
- Add `requirements.txt` and motivate how that can be useful to have later.

As we continue from here, **create commits after meaningful changes** and later also share the repository with learners. This nicely connects to other lessons of the workshop.

## Unit tests

Design code for testing.

- Move the main scope code into a main function.
- Discuss where to add a test and add a test to the statistics function:

```python
import pandas as pd
from matplotlib import pyplot as plt
import pytest


def plot_data(data, mean, xlabel, ylabel, file_name):
    plt.plot(data, "r-")
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.axhline(y=mean, color="b", linestyle="--")
    #   plt.show()
    plt.savefig(file_name)
    plt.clf()


def compute_mean(data):
    mean = sum(data) / len(data)
    return mean


def test_compute_mean():
    result = compute_mean([1.0, 2.0, 3.0, 4.0])
    assert result == pytest.approx(2.5)


def read_data(file_name, nrows, column):
    data = pd.read_csv(file_name, nrows=nrows)
    return data[column]


def main():
    for num_measurements in [25, 100, 500]:

        temperatures = read_data(
            file_name="temperatures.csv",
            nrows=num_measurements,
            column="Air temperature (degC)",
        )

        mean = compute_mean(temperatures)

        plot_data(
            data=temperatures,
            mean=mean,
            xlabel="measurements",
            ylabel="air temperature (deg C)",
            file_name=f"{num_measurements}.png",
        )


if __name__ == "__main__":
    main()
```


## Split long script into modules

- Discuss how you would move some functions out and organize them into separate modules which can be imported to other projects: For instance `compute_mean` can be moved to `statistics.py`.
- Discuss naming.
- Discuss interface design.

## Summarize in the collaborative document

- Now return to initial questions on the collaborative document and discuss questions and comments. If there is time left, there are additional questions and exercises.
- It is easier and more fun to teach this as a pair with somebody else where one person can type and the other person helps watching the questions and commends and relays them to the co-instructor.
