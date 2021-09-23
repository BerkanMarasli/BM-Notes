# Matplotlib

## Libraries

matplotlib.pyplot: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.html

numpy: https://numpy.org/doc/stable/

pandas: https://pandas.pydata.org/docs/

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
```



## Basic Graphing

### `.plot()`

`matplotlib.pyplot.plot(X, Y)` or  `plt.pyplot.plot(X, Y)`: default plot Y against X where X, Y are commonly a 1D array

```python
x = [1, 2, 3]
y = [2, 4, 6]
plt.plot(x, y) # Default plot
plt.show() # View plot
```

There are a huge number of properities that can be passed as attributes:

- `color = `
- `linestyle = `
- `linewidth = `
- `marker = `
- `markersize = `
- `markeredgecolor = `

There is a shorthand for styling: `'[color][marker][linestyle]'`

```python
plt.plot(x, y, 'ro--')
plt.show()
```

### `.title()`,  `.xlabel()` and `.ylabel()`

`matplotlib.pyplot.title('title text')` or  `plt.pyplot.title('title text')`: add title to plot

`matplotlib.pyplot.xlabel('axis text')` or  `plt.pyplot.xlabel('axis text')`: add label to x-axis

`matplotlib.pyplot.ylabel('axis text')` or  `plt.pyplot.ylabel('axis text')`: add label to y-axis

All have common additional attribute(s):

- `fontdict = {'fontsize: ', 'fontname': , 'fontweight: ', 'color: ', 'verticalalignment: ', 'horiztonalalignment: '}`
    1. Available font names: https://jonathansoma.com/lede/data-studio/matplotlib/list-all-fonts-available-in-matplotlib-plus-samples/

```python
plt.title('Basic Graph')
plt.xlabel('X-axis')
plt.ylabel('Y-axis')
```

### `.xticks()` and  `.yticks()`

`matplotlib.pyplot.xticks(X)` or  `plt.pyplot.xticks(X)`: changes x-axis tick labels where X is commonly a 1D array

`matplotlib.pyplot.yticks(Y)` or  `plt.pyplot.yticks(Y)`: changes y-axis tick labels where Y is commonly a 1D array

```python
plt.xticks([0, 1, 2, 3, 4])
plt.yticks([0, 2, 4, 6, 8])
# Ticks do not have to be evenly spaced. The tick label will be placed in its appropiate position
```

### `.legend()`

`matplotlib.pyplot.legend(X)` or  `plt.pyplot.legend(X)`: add legend to plot where X is the legend labels in a 1D array

```python
plt.legend(['A basic line'])
```

### =============== use numpy to generate array of equally spaced values =================

Use numpy to create array of equally spaced values: `numpy.arange(X,Y,Z)` or `np.arange(X,Y,Z)` where X is the starting value (inclusive), Y is the end value (not inclusive) and Z is the step size of the increments. X, Y and Z are integers

```python
equally_spaced_array = np.arange(0,5.5,0.5)
# equally_spaced_array == [0, 0.5, 1, 1.5, 2, 2.5, 3, 3.5, 4, 4.5, 5]
plt.plot(equally_spaced_array, equally_spaced_array**2) # Quadratic curve plotted
plt.show()
```

### `.figure()`

`matplotlib.pyplot.figure()` or  `plt.pyplot.figure()`: creates new figure (a.k.a plot)

To resize plot, there are a number of attributes that can be passed in:

- `figsize = (X,Y)` where X and Y is the ratio of the x-axis to the y-axis respectively. X and Y are integers
- `dpi = X` where X is the pixels per inch and is an integer

```python
plt.figure(figsize=(5,3), dpi=300)
```

### `.savefig()`

`matplotlib.pyplot.savefig('X')` or  `plt.pyplot.savefig('X')`: saves figure (a.k.a plot) to directory where X is the filename. The dpi can be specified as an additional attribute.

```python
plt.savefig('saved_graph.png', dpi=400)
```



## Bar Graph

### `.bar()`

`matplotlib.pyplot.bar(X, Y)` or  `plt.pyplot.bar(X, Y)`: bar plot where X are the labels and Y are the values. X andY are commonly a 1D array

```python
labels = ['A', 'B', 'C']
values = [1, 4, 2]
plt.bar(labels, values)
plt.show()
```

Graphing properites:

- Title
- X-axis label
- Y-axis label
- Legend
- Resize graph

### `.set_hatch()`

`.set_hatch('X')`: adds a pattern to the specified bar where X is the pattern identifier

```python
bars = plt.bar(labels, values)
bars[0].set_hatch('/')
bars[1].set_hatch('o')
bars[2].set_hatch('*')
plt.show()
######## Cleaner Code ########
bars = plt.bar(labels, values)
patterns = ['/', 'o', '*']
for bar in bars:
    bar.set_hatch(patterns.pop(0))
plt.show()
```




