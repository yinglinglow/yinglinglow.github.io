---
layout: post
title: "Visualising Data"
date: 2020-01-26
---

After you actually have a somewhat complete, clean and organised dataset, it's time to understand what the data is actually saying! Are there any obvious trends?

__Table of Contents__
 * Data visualisation
    - [General](#general)
    - [matplotlib](#matplotlib)
    - [seaborn](#seaborn)
    - [Pandas](#pandas)
    - [Plotly](#plotly)

<a id="general"></a>
### General

General code that can be used across matplotlib, pandas, or seaborn, since the latter are based off of matplotlib.

On a side note for beginners, I highly recommend to learn matplotlib without pandas first if you intend to customise your plots.

Also, the below is more for individuals who already know how to structure their code for plotting - this is just a quick list for reference on what to use when you need to achieve something.

```python
#to start
%matplotlib inline #for jupyter notebook to display plots automatically without plt.show()

plt.style.use('ggplot') #displays plots in ggplot style

plt.figure(figsize=(8,2)) #creates a new figure and sets figure size

#graph labelling
plt.xlabel('X label')
plt.ylabel('Y label')
plt.title('Title')

#legend
legend_list = ['line1', 'line2', 'line3']
plt.legend(legend_list) #to label all lines at one go
plt.legend(loc='upper right') #set legend, use strings (such as 'upper right') or, location code (integers)
plt.legend(bbox_to_anchor=(1,1)) #sets legend outside of figure

#ticks
plt.xticks(rotation=45) #rotates ticks.
plt.xticks([0,1], ['No','Yes']) #renames ticks

#others
plt.tight_layout() #automatic reshuffling to minimize overlaps
plt.savefig('picture.png', dpi=200) #saves as an image. put this instead of plt.show()

```

To customise the aesthetics (tab+shift for full details): e.g. 

```python
df.plot.scatter(x='col1',y='col2', c='col3', color='red', edgecolor='black', lw=1, s=50, figsize=(12,3))

```
- bins: for histogram; number of 'bars' (intervals) e.g. `bins=10`

- c: varies color by the value in this column e.g. `c='col3'`
- s: varies size by the value in this column e.g. `s='col4'`

- color: insert 'b' for blue, 'r' for red, etc or rgb hexcode (#000000). e.g. `color='red'`
- cmap: uses a color palette, for when varied colors are required e.g. `cmap='coolwarm'`

- alpha: transparency (0 to 1) e.g.`alpha=0.5`
- lw: linewidth (integers) e.g. `lw=3`
- ls: linestyle ('-', '--', ':', etc) e.g.`ls='--'`
- edgecolor: ('black', etc) e.g. `edgecolor='black'`
- marker: 'o', '*', '+', etc
    
Others: markerfacecolor, markeredgewidth, markeredgecolor, etc

<a id="matplotlib"></a>
### matplotlib

```python
import matplotlib.pyplot as plt

plt.plot(x,y) #single line graph
plt.plot(df['col1'], df['col2']) #single line graph - col1 on x, col2 on y
plt.plot(df) #plots entire df (can be multiple lines)

```

Plotting more than one figure

```python
fig = plt.figure(figsize=(10,8)) #affects size of all figures
ax1 = fig.add_axes([0,0,0.5,0.5]) #specify where the axes are positioned
ax2 = fig.add_axes([0.5,0.5,0.5,0.5]) 

ax1.plot(x1,y1, label = 'line one') #to plot the line
ax1.xlabel('x1 label')
ax1.ylabel('y1 label')
ax1.title('ax1')

ax2.plot(x2,y2, label = 'line two')
ax2.xlabel('x2 label')
ax2.ylabel('y2 label')
ax2.title('ax2')

```

Plotting multiple figures (e.g. 2 x 2 or 3 x 3)

```python

fig, axes = plt.subplots(nrows=2, ncols=2)

axes[0,0].set_title('A')
df['A'].plot(ax=axes[0,0])

axes[0,1].set_title('B')
df['B'].plot(ax=axes[0,1])

axes[1,0].set_title('C')
df['C'].plot(ax=axes[1,0])

axes[1,1].set_title('D')
df['D'].plot(ax=axes[1,1])

```

<a id="seaborn"></a>
### seaborn

Seaborn comes with some datasets that you can play around with, like titanic:

```python
titanic = sns.load_dataset('titanic') #to load dataset

```

```python
#for aesthetics
sns.set_palette('GnBu_d')
sns.set_style('whitegrid') #or darkgrid, etc

#different types of plots
sns.countplot(x='col1', data=df) #data is the dataframe. count plot gives count for categorical data. palette='RdBu' gives a red blue color.
sns.distplot(df['col1'], bins=50, kde=False, rug=True) #histogram; kde=True smooths the histogram, rug=False removes markers at bottom of chart to indicate density
sns.barplot(x='col1', y='col2', data=df, estimator=np.median) #estimator is the chosen method used in the plot. if unspecified it uses mean. 
sns.pointplot(x='col1', y='col2', data=df) #line plot with point markers
sns.boxplot(x='col1', y='col2', data=df)
sns.swarmplot(x='col1', y='col2', data=df)
sns.heatmap(df, square=True) #dataframe has to be a matrix, eg df.corr()
sns.clustermap(df) #similar to a heatmap but clustered
sns.jointplot(x='col1', y='col2', data=df) #plots x against y, scatter plot with frequency hist on the side, can change type = 'hex'
sns.regplot(x='col1', y='col2', data=df) #scatterplot with regression line (to remove, fit_reg=False)
sns.kdeplot(x='col1', y='col2', data=df) #kde
sns.pairplot(df) #plots all variables against each other
sns.lmplot(x='col1', y='col2', data=df) #regression plot. also able to split into subplots by col='col1', row='col2'

sns.despine() #removes spines from plot

```

To customise the aesthetics (tab+shift for full details): e.g. 

```python
sns.heatmap(df, square=True, cmap='RdYlGn', linewidths=3)
sns.kdeplot(x='col1', y='col2', data=df, cmap='plasma', shade=True, shade_lowest=False)

```
- cmap - color schemes like 'RdYlGn', 'viridis', etc. e.g. `cmap='RdYlGn'`
- palette - color palette e.g. `palette='RdBu'`, `palette='Dark2'`
- annot - labels e.g. `annot=True`
- hue - additional split within graph, by the value in this column e.g. `hue='col3'`


Plotting multiple figures

```python
g = sns.FacetGrid(df, col="col1",  row="col2") #splits total data into respective subplots, cut by col1 and col2
g = g.map(plt.hist, "col_total") #col_total is the column that you wan to split

```

<a id="pandas"></a>
### pandas

```python
import pandas as pd
import matplotlib.pyplot as plt

df['col'].plot() #single line graph with index as x
df.plot(x='col1', y='col2') #single line graph

df['col'].plot.hist() #histogram (df.hist() also works)
df[['col1','col2']].plot.box() #more than one box plot
df.plot.line(x='col1', y='col2')
df.plot.scatter(x='col1', y='col2') #scatter plot
df.plot.density() #kde
df.plot.area() #area under graph
df.plot.hexbin(x='col1', y='col2', gridsize=2) #change gridsize for size of hexagons
df.plot(kind='bar') #barplot
df.plot(kind='barh') #horizontal barplot
df.scatter_matrix() #similar to pair plot in seaborn

```

Shortcuts for pandas formatting:

- To include the title - `df.plot(title='_')`
- To include figsize - `df.plot(figsize=(12,3))`
- To include the legend - `df.hist().legend(bbox_to_anchor=(1,1))`
- To plot subplots - `df.plot(subplots=True)`

<a id="plotly"></a>
### Plotly (and cufflinks)

```python
import cufflinks as cf

from plotly.offline import download_plotlyjs,init_notebook_mode,plot,iplot 
init_notebook_mode(connected=True)
cf.go_offline #to work offline

df.plot() #normal plot
df.iplot() #interactive plot

#examples of different types of plots
df.iplot(kind='scatter', x='col1', y='col2', mode='markers')
df.iplot(kind='bar', x='col1', y='col2')
df.iplot(kind='box')
df.iplot(kind='surface')
df.iplot(kind='bubble', x='col1', y='col2', size='col3')
df['col1'].iplot(kind='hist')
df[['col1','col2']].iplot(kind='spread') #line plot with spread underneath

```