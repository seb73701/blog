# You’ll Never Walk Alone: Use Pygwalker to Visualize Data in Jupyter Notebook

As a data scientist, you’re always looking for ways to simplify your workflow and create interactive and engaging visualizations of your data. You might wish there was a more straightforward way to interact with your data, similar to using Tableau.

Then, you should take a look at [PygWalker](https://github.com/Kanaries/pygwalker) — the lightweight, easy-to-use Python binding of [Graphic Walker](https://github.com/Kanaries/graphic-walker).

## What is PyGWalker?
PyGWalker integrates Jupyter Notebook (or other jupyter-based notebooks) with Graphic Walker, an open-source alternative to Tableau. The name “PyGWalker” is an abbreviation of “Python binding of Graphic Walker.” It allows data scientists to analyze and visualize data patterns with simple drag-and-drop operations, instead of using complex Python code.

In this article, we’ll walk you through how to set up Pygwalker in Jupyter Notebook, and explore some of the cool things you can do with Graphic Walker to make your data analysis and visualization workflow much more manageable. You can also try PygWalker in [Google Colab](https://colab.research.google.com/drive/171QUQeq-uTLgSj1u-P9DQig7Md1kpXQ2?usp=sharing), [Binder](https://mybinder.org/v2/gh/Kanaries/pygwalker/main?labpath=tests%2Fmain.ipynb), or [Graphic Walker Online Demo](https://graphic-walker.kanaries.net/) to test it out.

## Getting Started
Before using Pygwalker, make sure to install the package using pip. You can do this by running the following command in your terminal or command prompt:

```shell
pip install pygwalker
```

Once installed, you can start using Pygwalker in Jupyter Notebook by importing the pandas and pygwalker libraries:

```python
import pandas as pd
import pygwalker as pyg
```

To call up Graphic Walker with your data, load your data into a pandas dataframe and pass it to Pygwalker’s `walk` method:

```python
df = pd.read_csv('./bike_sharing_dc.csv', parse_dates=['date'])
gwalker = pyg.walk(df)
```

You can give it an on-hand try on [Binder](https://mybinder.org/v2/gh/Kanaries/pygwalker/main?labpath=tests%2Fmain.ipynb) or [Google Colab](https://colab.research.google.com/drive/171QUQeq-uTLgSj1u-P9DQig7Md1kpXQ2?usp=sharing).

![1_y4ok335dhakpfwvgiccnhg.gif](/assets/img/developpement/python/1_y4ok335dhakpfwvgiccnhg.gif)

And that’s it! You now have a Tableau-like interface to explore and visualize your data.

![1_ciimzwakypec9r3whuyzjq.gif](/assets/img/developpement/python/1_ciimzwakypec9r3whuyzjq.gif)

## Exploring Your Data with Graphic Walker
With Graphic Walker, you can do some cool things like changing the mark type to create different types of charts. For example, to create a line chart, simply change the mark type to the line:

![1_qflqrvtunz3bueaz-smama.webp](/assets/img/developpement/python/1_qflqrvtunz3bueaz-smama.webp)

To compare different measures, you can create a concat view by adding more than one measure into rows/columns:

![1_sozdphqyk7jzflzudjcxvw.webp](/assets/img/developpement/python/1_sozdphqyk7jzflzudjcxvw.webp)

To create a facet view of several subviews divided by the value in dimension, put dimensions into rows or columns to make a facets view:

![1_2p29swgl14zi0tq8sdhmhw.webp](/assets/img/developpement/python/1_2p29swgl14zi0tq8sdhmhw.webp)

## Conclusion
Pygwalker is an excellent tool for data scientists who want to analyze and visualize their data in a more interactive and engaging way. By combining Jupyter Notebook with Graphic Walker, you can simplify your workflow and create stunning visualizations with simple drag-and-drop operations.

In addition to Pygwalker, you can also check out [RATH](https://kanaries.net/), an open-source, AI-powered, automated exploratory data analysis tool that further simplifies your data analysis and visualization workflow. ([RATH GitHub](https://github.com/Kanaries/Rath))

Give Pygwalker a try, and you’ll be amazed at how easy and fun it is to use!

https://github.com/Kanaries/pygwalker

source : https://python.plainenglish.io/youll-never-walk-alone-use-pygwalker-to-visualize-data-in-jupyter-notebook-7084506d33cd