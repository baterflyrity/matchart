# MatChart

MatChart is a convenient one-line wrapper around *matplotlib* plotting library.

## Usage

```python
from matchart import plot

plot([x1, y1], [y2], [x3, y3], y4, ..., 
         # common parameters
         kind='plot',         
         show: bool = True,
         # plotter explict parameters
         label: ClippedArguments = None,
         color: CycledArguments = None,
         marker: ClippedArguments = None,
         linestyle: ClippedArguments = None,
         linewidth: ClippedArguments = None,
         markersize: ClippedArguments = None,
         # figure and axes parameters
         legend: Optional[bool] = None,
         legend_kwargs: Optional[Dict[str, Any]] = None,
         title: Optional[str] = None,
         title_kwargs: Optional[Dict[str, Any]] = None,
         xlabel: Optional[str] = None,
         xlabel_kwargs: Optional[Dict[str, Any]] = None,
         ylabel: Optional[str] = None,
         ylabel_kwargs: Optional[Dict[str, Any]] = None,
         limit: Union[Tuple[Any, Any, Any, Any], bool] = True,
         figsize: Tuple[float, float] = (10, 8),         
         dpi: float = 100,
         subplots_kwargs: Optional[Dict[str, Any]] = None,
         grid: Optional[bool] = False,
         grid_kwargs: Optional[Dict[str, Any]] = None,
         theme='seaborn-v0_8-deep',
         # plotter rest parameters
         **plotter_kwargs  #
         ) -> Tuple[Figure, Axes, List[Artist]]
```

`kind` - pyplot [function name](https://matplotlib.org/stable/plot_types/index.html), e.g. _plot_ or _scatter_.

`show` - whether to show plot or just to draw it.

`label`, `color`, `marker`, `linestyle`, `linewidth`, `markersize` and rest `plotter_kwargs` - plotter parameters. Can differ per kind. See [common parameters](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html) for `kind='plot'`. Can be `list` or `tuple` to define per-dataset values. Values can also be `None` or even clipped to skip definition for particular dataset.

`legend` - set `True`, `False` or `None` to autodetect. `legend_kwargs` - [additional parameters](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.legend.html).

`title` - plot's title. `title_kwargs` - [additional parameters](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_title.html).

`xlabel` - horizontal axis label. `xlabel_kwargs` - [additional parameters](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xlabel.html).

`ylabel` - vertical axis label. `ylabel_kwargs` - [additional parameters](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_ylabel.html).

`limit` - set `True` to autodetect 2D plot's borders or `False` to use default matplot's behaviour. Set to `(left, right, bottom, top)` to use [custom borders](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_ylim.html).

`figsize` - plot's size as `(width, height)` in inches. By default, is overriden with *10x8*.

`dpi` - plot's resolution in dots-per-inch.

`subplots_kwargs` - [additional plot's parameters](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplots.html) and [Figure's parameters](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.figure.html#matplotlib.pyplot.figure).

`grid` - plot's [grid visibility](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.grid.html) with `grid_kwargs` as additional parameters.

`theme` - plot's [style](https://matplotlib.org/stable/api/style_api.html#matplotlib.style.use). By default is overriden with *seaborn-v0_8-deep*.

## Example

### Just plot something

Firstly prepare data:

```python
import numpy as np

x1 = np.arange(21)
y1 = x1 ** 2
y2 = x1 ** 1.9
x3 = 5, 15
y3 = 300, 100
y4 = x1 ** 2.1
```

Then plot data:

```python
from matchart import plot

plot([x1, y1], [y2], [x3, y3], y4,  
     label=['$x^{2}$', '$x^{1.9}$', 'Points'],  
     xlabel='X', ylabel='Y',  
     title='Power of $x$ comparison.',  
     grid=True,  
     color=[None, 'green', 'red', 'gray'],  
     marker=['o', None, '*'],  
     linestyle=[None, '--'],  
     linewidth=[None, 3, 0],  
     markersize=20,  
     fillstyle='top')
```

![](https://raw.githubusercontent.com/baterflyrity/matchart/main/docs/example1.png)

### Migrate from matplotlib simple

Let's take [FiveThirtyEight style sheet example](https://matplotlib.org/stable/gallery/style_sheets/fivethirtyeight.html#sphx-glr-gallery-style-sheets-fivethirtyeight-py):

```python
import matplotlib.pyplot as plt
import numpy as np

plt.style.use('fivethirtyeight')

x = np.linspace(0, 10)
np.random.seed(19680801)  # Fixing random state for reproducibility

fig, ax = plt.subplots()
ax.plot(x, np.sin(x) + x + np.random.randn(50))
ax.plot(x, np.sin(x) + 0.5 * x + np.random.randn(50))
ax.plot(x, np.sin(x) + 2 * x + np.random.randn(50))
ax.plot(x, np.sin(x) - 0.5 * x + np.random.randn(50))
ax.plot(x, np.sin(x) - 2 * x + np.random.randn(50))
ax.plot(x, np.sin(x) + np.random.randn(50))
ax.set_title("'fivethirtyeight' style sheet")
plt.show()
```

![](https://raw.githubusercontent.com/baterflyrity/matchart/main/docs/simple_matplotlib.png)

and rewrite it with MatChart:

```python
from matchart import plot
import numpy as np

x = np.linspace(0, 10)
np.random.seed(19680801)  # Fixing random state for reproducibility

plot([x, np.sin(x) + x + np.random.randn(50)], 
	[x, np.sin(x) + 0.5 * x + np.random.randn(50)],
	[x, np.sin(x) + 2 * x + np.random.randn(50)],
	[x, np.sin(x) - 0.5 * x + np.random.randn(50)],
	[x, np.sin(x) - 2 * x + np.random.randn(50)],
	[x, np.sin(x) + np.random.randn(50)],
	title="'fivethirtyeight' style sheet",
	theme='fivethirtyeight')
```

![](https://raw.githubusercontent.com/baterflyrity/matchart/main/docs/simple_matchart.png)

*Note that default figure size differs.*

### Migrate from matplotlib stackplot

Let's take [stackplots example](https://matplotlib.org/stable/gallery/lines_bars_and_markers/stackplot_demo.html#sphx-glr-gallery-lines-bars-and-markers-stackplot-demo-py):

```python
import matplotlib.pyplot as plt

# data from United Nations World Population Prospects (Revision 2019)
# https://population.un.org/wpp/, license: CC BY 3.0 IGO
year = [1950, 1960, 1970, 1980, 1990, 2000, 2010, 2018]
population_by_continent = {
	'africa':   [228, 284, 365, 477, 631, 814, 1044, 1275],
	'americas': [340, 425, 519, 619, 727, 840, 943, 1006],
	'asia':     [1394, 1686, 2120, 2625, 3202, 3714, 4169, 4560],
	'europe':   [220, 253, 276, 295, 310, 303, 294, 293],
	'oceania':  [12, 15, 19, 22, 26, 31, 36, 39],
}

fig, ax = plt.subplots()
ax.stackplot(year, population_by_continent.values(),
             labels=population_by_continent.keys(), alpha=0.8)
ax.legend(loc='upper left')
ax.set_title('World population')
ax.set_xlabel('Year')
ax.set_ylabel('Number of people (millions)')

plt.show()
```

![](https://raw.githubusercontent.com/baterflyrity/matchart/main/docs/stackplot_matplotlib.png)

and rewrite it with MatChart:

```python
from matchart import plot

# data from United Nations World Population Prospects (Revision 2019)
# https://population.un.org/wpp/, license: CC BY 3.0 IGO
year = [1950, 1960, 1970, 1980, 1990, 2000, 2010, 2018]
population_by_continent = {
	'africa':   [228, 284, 365, 477, 631, 814, 1044, 1275],
	'americas': [340, 425, 519, 619, 727, 840, 943, 1006],
	'asia':     [1394, 1686, 2120, 2625, 3202, 3714, 4169, 4560],
	'europe':   [220, 253, 276, 295, 310, 303, 294, 293],
	'oceania':  [12, 15, 19, 22, 26, 31, 36, 39],
}

plot([year, population_by_continent.values()], 
	kind='stackplot', 
	labels=population_by_continent.keys(), 
	alpha=0.8, 
	legend=True, 
	legend_kwargs=dict(loc='upper left'), 
	limit=False, 
	title='World population', 
	xlabel='Year', 
	ylabel='Number of people (millions)')
```

![](https://raw.githubusercontent.com/baterflyrity/matchart/main/docs/stackplot_matchart.png)

*Note that default figure size and theme differs.*
