Improving your plots
====================


Why make a (good) plot?
-----------------------

* To communicate information
* "A picture is worth a thousand words"

### An example

The 1986 Challenger Shuttle disaster - taken from Edward Tufte's book [Visual Explanations: Images and Quantities, Evidence and Narrative](https://www.edwardtufte.com/book/visual-explanations-images-and-quantities-evidence-and-narrative/).

On January 28, 1986, the space shuttle Challenger exploded shortly after launch. Subsequent investigation blamed the explosion on a faulty O-ring seal caused by the cold weather.

![Challenger launch](./pics/plotting_challenger-launch.jpg) ![Challenger explosion](./pics/plotting_challenger-explosion.jpg)

The previous day, the engineers that designed the rocket had given a presentation arguing to delay the launch in response to the low temperature forecast due to the risk of O-ring failure. NASA responded skeptically, arguing that the evidence the engineers presented did not conclusively link O-ring problems to low temperature. Ultimately, the decision to launch was maintained.

Here are two of the slides from the presentation arguing for a launch delay (from Tufte's *Visual Explanations*):

![Tufte slide 1](./pics/plotting_tufte-challenger-slide1.jpg) ![Tufte slide 2](./pics/plotting_tufte-challenger-slide2.jpg)

Note that these slides were shown separately. How easy are they to understand without the accompanying oral presentation? Do you think a more visual presentation would be helpful?

The presentation also included this plot showing temperature and O-ring damage in previous test launches (from Tufte's *Visual Explanations*):

![Tufte slide 3](./pics/plotting_tufte-challenger-slide3.jpg)

Would you delay the launch based on this evidence?

Tufte argued that the engineers might have been more convincing if they had used a plot like this instead (from Tufte's *Visual Explanations*):

![Tufte plot](./pics/plotting_tufte-challenger-plot.png)

What do you think?


What makes a good plot?
-----------------------

[Presentation: plotting dos and don'ts](https://docs.google.com/presentation/d/1G0HcssmM5A5HbG-sKbXCXg07G4YdC7Pv2S2QyjlHT8c/edit?usp=sharing)


Color
-----

Color is a key element for creating clear, informationally dense, and aesthetically pleasing plots. Yet it is also one of the most frequently misused plotting elements.

![Crameri et al. (2020) Fig. 1](pics/plotting_crameri2020-fig1.jpg)

Tips for using color in plots:

* Use colors that shows your results clearly and honestly
* Consider colorblind readers
* Consider recommendations on best use of color (not always agreed on!) e.g. [Nature Reviews' guide to designing figures](https://www.nature.com/documents/natrev-artworkguide_PS.pdf)
* When in doubt, a perceptually uniform colormap is a usually good choice for continuous data

Resources for choosing colormaps:

* [Colorbrewer](https://colorbrewer2.org/): sequential, diverging, and qualitative colormaps
* [i want hue](https://medialab.github.io/iwanthue/): qualitative colormaps
* [Scientific Color Maps](https://www.fabiocrameri.ch/colourmaps/): perceptually uniform colormaps. See also the accompanying paper [Crameri et al. (2020) The misuse of color in scientific communication](https://doi.org/10.1038/s41467-020-19160-7). Fig. 6 is a decision tree for choosing an appropriate colormap based on your data.
* Matplotlib's [choosing colormaps page](https://matplotlib.org/stable/users/explain/colors/colormaps.html): lists the built-in colormaps and offers some guidance on their use.
* [NASA earth observations datasets](https://neo.gsfc.nasa.gov/): each dataset has a downloadable colormap (ACT format).
* [cmweather](https://cmweather.readthedocs.io/en/latest/api.html): colormaps for weather and climate data
* [cmocean](https://matplotlib.org/cmocean/): colormaps for common oceanographic variables.
* [NASA blog series on color](https://earthobservatory.nasa.gov/blogs/elegantfigures/2013/08/05/subtleties-of-color-part-1-of-6/): good overview of basic color theory, how and why to use color


Guidance and inspiration
------------------------

[Data-to-viz](https://www.data-to-viz.com/): decision trees that guide you to an appropriate plot type (scatterplot, histogram, line plot, ...) based on what kind of data you have (numeric, categorical, ...). It briefly explains what the visualization is good for, lists some common mistakes, and links to example plots with code in Python, R, D3.js, and React.

[Python Graph Gallery](https://python-graph-gallery.com/): a large collection of plots. You choose a plot type and then browse examples, each accompanied by the plot code, which usually uses [seaborn](https://seaborn.pydata.org/) or [matplotlib](https://matplotlib.org/).

[R Graph Gallery](https://r-graph-gallery.com/): like the Python Graph Gallery, but for R. While you won't find python code here, you may find some inspiring plots that aren't listed in the Python Graph Gallery. Most plots are made with [ggplot2]() and so should be straightforward to recreate with [plotnine](https://plotnine.org/).

[Nature Reviews's guide to designing figures](https://www.nature.com/documents/natrev-artworkguide_PS.pdf) provides recommendations and guiding principles for designing plots and conceptual figures.

Journal articles:

* [Rougier et al. (2014) Ten simple rules for better figures](https://doi.org/10.1371/journal.pcbi.1003833)
* [Kelleher & Wagener (2011) Ten guidelines for effective data visualization in scientific publications](https://doi.org/10.1016/j.envsoft.2010.12.006)
* [Gelman et al. (2002) Let's practice what we preach: turning tables into graphs](https://doi.org/10.1198/000313002317572790)


Python plotting libraries
-------------------------

[Matplotlib](https://matplotlib.org/) is by far the most widely-used python plotting library. It allows full customization of static, animated, and interactive visualizations. Data objects from [xarray](https://docs.xarray.dev/en/stable/) and [pandas](https://pandas.pydata.org/) have built-in methods for plotting with matplotlib. Because it is so popular, there are many tutorials and stack overflow questions to turn to if you need help. Large language models also tend to respond fairly well to matplotlib-related prompts. Matplotlib plotting code can be fairly verbose as complex plots often require explicit data preprocessing and mapping to plot elements.

[Cartopy](https://scitools.org.uk/cartopy/docs/latest/) extends matplotlib with methods for plotting geographic data. It implements several coordinate reference systems and provides methods for adding "features" (points, lines, polygons) to a plot, including global [Natural Earth](http://www.naturalearthdata.com/) vector data such as coastlines, lakes, rivers and country borders.

[Seaborn](https://seaborn.pydata.org/) is a wrapper for matplotlib focused on statistical visualization of data in [pandas](https://pandas.pydata.org/) data structures. It's goal is to automate many of the mechanics of drawing the plot, allowing you to focus more on what you want to show.

[Plotnine](https://plotnine.org/) is an implementation of [The Grammar of Graphics](https://link.springer.com/book/10.1007/0-387-28695-0) based on the [ggplot2](https://ggplot2.tidyverse.org/) R package. Like seaborn, plotnine provides many higher-level methods that let you concisely describe what you want to show without needing to specify every semantic mapping or implement every statistical aggregation. It may be of interest to people who are already familiar with ggplot2.

[Plotly](https://plotly.com/python/) is a multi-language graphing library (Python, R, Julia, Javascipt, MATLAB) focused on interactive graphics. Its main advantages are that figures are interactive by default (zoom, pan, hover for more information, filter data) and it lets you make plots in multiple programming languages with a consistent API rather than needing to learn a new library for each language.

[Bokeh](https://bokeh.org/) is another library focusing on interactive graphics.


Example plots
-------------

See [improving-plots.ipynb](improving-plots.ipynb)
