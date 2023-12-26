# Visualization Tools with Python

Common Viz Commands with Python


<img src="https://miro.medium.com/max/1400/1*KgXXwcLfNb53DJIQ3k_YSQ.webp">

Image by Author
Visualization plays a big part in exploratory data analysis (EDA). It not only helps us understand the relationship between features, but also an effective tool to communicate our findings.

In this article, I would like to share some commonly used visualization tools and commands that most of my analysis rely heavily on. I will walk you through examples with the “Adoptable Dogs in the US” dataset (allDogDescriptions.csv) from Kaggle.

Before we jump into the walk-through, I want to give you a brief overview of this dataset.

<img src="https://miro.medium.com/max/1400/1*dsWaeHyBkhdABRm7_Sc6eg.webp">

Adoptable Dogs in the US Data Overview — Screenshot by Author
We have 36 columns and 58180 rows in this dataset. We have information (e.g. breed, color, age, location, etc.) on each adoptable dog in the US. The dataset is to help the researchers understand (source from Kaggle):

Finding out how many of each type and breed of dog are brought into shelters across the USA in a given year.
Seeing which states have the most imports of dogs and what breeds/types those are.
Determining if there are any trends in the types/breeds of dogs being brought into shelters (e.g. more pit bulls than golden retrievers)
Missingno
It’s very common to have missing values in a dataset. We, as data scientists/analysts need to make the call on how to handle the missing values. Understanding the distribution of these missing values across the dataset is extremely crucial before we can make that decision.

Missingno (aliased as “MSNO”) is a great tool to visualize missing values (NaNs) in a dataset.

To install this library, simply type in the code below in your command line:

```
pip install missingno
```

This library offers us 3 different ways of displaying the missing values:

1. Matrix

```python
# Importing the libraries
import pandas as pd
import missingno as msno
  
# Visualizing NaNs with msno matrix
missingValueColumns = df.columns[df.isnull().any()].tolist()
msno.matrix(df[missingValueColumns], width_ratios=(10, 1), \
           figsize=(20,11), fontsize=12, sparkline=True, labels=True)
```
https://gist.github.com/m3redithw/7a6ac17e87d95d00d46f1f90b3bf05f0#file-msno_matrix-py

In this example, I specified to only display the columns with missing values and ignore the columns without any NaNs.

<img src="https://miro.medium.com/max/1400/1*Zlbg2AGkgKYuKioViK7RVQ.webp">

MSNO Matrix — Image by Author
It’s very straight forward and easy to understand — the gray line represents existing values and white line represents missing values. So the more values are missing from a column, the more “empty” the column on this matrix appear to be.

2. Bar Chart
Similarly, we can also use a bar chart to find patterns of “missingness”.

```python
# Importing the libraries
import pandas as pd
import missingno as msno
  
# Visualizing NaNs with msno bar chart
msno.bar(df)
```
https://gist.github.com/m3redithw/b285ca080613271bf176c1f60761c001#file-msno_bar-py

Here I simply put the entire dataframe as input instead of selecting columns with missing values like the above example. It’s up to you what columns you would like to include.

<img src="https://miro.medium.com/max/1400/1*BkZvkF8_ZqbkALz-TxcBgQ.webp">

MSNO Bar — Image by Author
Now we can see all columns, regardless if it contains missing values or not, are displayed on this chart. The height of the bar represents how many existing data a column has. So the shorter the bar is, the more missing values it has.

3. Heatmap
Heatmap is another option this library gives us.

```python
# Importing the libraries
import pandas as pd
import missingno as msno
  
# Visualizing NaNs with msno heatmap
msno.heatmap(df)
```
https://gist.github.com/m3redithw/d03fee7f2ec5021d283c81001a5e2778#file-msno_heatmap-py

As we see here the function is very simple and intuitive. Here is the output:

<img src="https://miro.medium.com/max/1400/1*sNTtzWdxD-jLghnScj9LNg.webp">

MSNO Heatmap — Image by Author
When the value is close to -1, it means the one variable appears then the other variable is very likely to be missing.
When the value is close to 0, it means that there is no dependence between the occurrence of missing values of the two variables.
When the value is close to 1, it means one variable appears then the other variable is very likely to be present.
(Source of above explanation: 
[Geeks For Geeks](https://medium.com/u/158bf765767a?source=post_page-----2d6c6a0d7a02--------------------------------)
)

Seaborn
Seaborn is another commonly used Python visualization library based on matplotlib. In fact, I use Seaborn more than anything else in most of my analysis. As much as I love plotly for its interactivity, Seaborn’s rendering time is way faster and has enough customization freedom.

Here I’m only going to list a few frequently used charts, and you can explore on their documentation page for more.

seaborn.barplot
A simple bar chart has always been one of my favorite charts to create. In data storytelling, it’s almost never about how “fancy” your chart is, but how you use your power to convey your audience in the most easy-to-understand way. And bar chart, the presentation itself, serves this purpose better than many others.

```python
# Importing the libraries
import seaborn as sns
import matplotlib.pyplot as plt

# Creating a dataframe for the top 10 breeds with the most count
count = pd.DataFrame(df.breed_primary.value_counts().head(10).reset_index())

# Visualizing the top 10 most prevalent adoptable dog breeds
plt.figure(figsize=(22,10))
sns.barplot(data=count, x='index', y='breed_primary', palette='copper')
plt.title('Most Prevalent Adoptable Dogs Breed', fontsize = 20)
plt.xlabel('Breed', fontsize=15)
plt.ylabel('Count', fontsize=15)
```
https://gist.github.com/m3redithw/3959d4925c374578f3c3a4d35fde11b3#file-sns_barplot-py

Result:

<img src="https://miro.medium.com/max/1400/1*UNIPydACWxvpXKoders0uA.webp">

In this example, I used the matplotlib functions to set the plot’s title and axes labels. We can clearly see the comparison and what’s the most prevalent dog breeds that are adoptable.

seaborn.countplot
The countplot, which visually similar to bar chart, is a great way to visualize the quantitive differences across categorical variables. For this dataset, it’s a perfect way to display how many adoptable dogs in the U.S. are house trained vs. those who are not.

```python
# Importing the libraries
import seaborn as sns
import matplotlib.pyplot as plt

# Visualizing how many adoptable dogs are house trained vs. not
plt.figure(figsize=(20,12))
sns.countplot(data=df, x='house_trained', palette='binary_r')
plt.title('House Trained Adoptable Dogs in the U.S.', fontsize=20)
plt.xlabel('House Trained', fontsize=15)
plt.ylabel('Count', fontsize=15)
```
https://gist.github.com/m3redithw/147c9276660b6d136d777f5aedce82ed#file-sns_countplot-py

Result:

<img src="https://miro.medium.com/max/1400/1*MFH_swsgeZIjxfVAvylCXQ.webp">

Seaborn Countplot — Image by Author
For color palette, I chose the ‘binary_r’ for this graph. The diversity of Seaborn color palettes is another reason that I love this library, if you are interested in all the built-in values feel free to reference this article.

matplotlib
pandas.DataFrame.plot
This pandas function comes in handy when you need to generate a line plot. By default, this function utilizes matplotlib.

For example if I want to know the amount of adoptable dogs over time since 2018, I can easily generate the plot with one line of code.

```python
# Importing the libraries
import pandas as pd
import matplotlib.pyplot as plt

# Create new df with date as index
date = df.set_index('post_date')
date = date.sort_index()

# Generate line plot - amount of adopatble dogs over time
date[date.index>'2018-01-01'].index.value_counts().plot(
    figsize=(15,8),linewidth=1.5,
    color='#7f4f21', title = 'Amount of Adopatble Dogs Over Time')
```
https://gist.github.com/m3redithw/ea092227863b71f8ea50a296e623e0b9#file-pandas_plot-py

Output:

<img src="https://miro.medium.com/max/1400/1*PH-PXTNbeYZIiFESL97b1Q.webp">

I normally use these pandas function generated graphs to serve for my own understanding — easy and simple to make but most of them don’t make it to the final report.

Plotly
Plotly is known for its interactivity, animation, and the diverse chart types it supports. It’s my absolute favorite visualization tool so far. The only downside is that the rendering time is longer than other static charts. If you’re presenting the visuals via Jupyter Notebook, the chart won’t show up unless the audience run the notebook themselves.

Bar Charts
For example, let’s first look at this bar chart I created with Plotly:

```python
# Importing the libraries
import plotly.express as px

# Bar Chart
fig = px.bar(count,
             x='index', y='breed_primary', color = 'index',
             color_discrete_map = {'Pit Bull Terrier': '#3c2f2f', 'Labrador Retriever': '#4b3832',
                                   'Chihuahua': '#b8916e', 'Mixed Breed': '#be9b7b',
                                   'Terrier': '#c4a588', 'Hound': '#d1b9a2',
                                   'German Shepherd': '#e5d7ca', 'Boxer': '#fff4e6'},
             labels = {'index': 'Dog Breed', 'breed_primary': 'Count'},
             title='Most Prevalent Adoptable Dog Breeds in the U.S.')

# Customize Layout
fig.update_xaxes(categoryorder='array', categoryarray= ['Pit Bull Terrier', 'Labrador Retriever',
                'Chihuahua', 'Mixed Breed', 'Terrier', 'Hound', 'German Shepherd', 'Boxer'])
fig.update_layout(paper_bgcolor="#fffffe", plot_bgcolor='#fffffe', font_color='#231717',
                  font_size = 16, legend=dict(
                  orientation="h", y=-0.8, font = dict(size = 12)))
                  
```
https://gist.github.com/m3redithw/1ca09c1656f34c3091df645102d31384#file-plotly_bar-py

Output:

<img src="https://miro.medium.com/max/4800/1*i0q-1K2pk4OgJ-HMoDtEnA.gif">

Image by Author
Notice visually it looks practically the same with the Seaborn bar chart above. But it allows us to click on the legend, bar, etc. to selectively view specific data we want.

Images in Python
Besides making the charts interactive and more dynamic, Plotly also has the image feature where you can add your own image to the chart — truly allows us the customize every visual elements to a T.

For example, if I want to add an image to the bar:

```python
# Importing the libraries
from PIL import Image
import plotly.express as px

# Adding image to chart
pitbull = Image.open('img/pitbull.png')
fig.add_layout_image(
    dict(
        source=pitbull,
        xref="x",
        yref="y",
        x=-0.35,
        y=7000,
        sizex=5900,
        sizey=5500,
        opacity=1,
        layer = 'above'
    )
)
```
https://gist.github.com/m3redithw/fe5c9bd846645932ad6fde26610c69e6#file-plotly_image-py

Make sure that you import the PIL (Python Image Library) to read the image and input the file path correctly.

Output:

<img src="https://miro.medium.com/max/1400/1*0Y3bqC983o4wsfqT2lyu1Q.webp">

Image by Author
You can specify the location, size of the image, opacity, layer, etc. in the argument, and the image will display according to your preference.

RoughViz
This visualization package developed by Jared Wilber also is one of my top favorites. I can’t begin to tell you how much I love this sketch-board aesthetics! It’s different from the traditional visuals that prioritize on accuracy, and represents the generality of the data in its unique way.

<img src="https://miro.medium.com/max/1400/1*x562RQ21PKPV4BCdt2TkJg.gif">

To install the package:

```
pip install py-roughviz
```

Donut Plot
The package offers a variety of charts, but here I’m going to use “donut plot” as an example to visualize the amount of mix breed vs. non-mixed breed dogs.



To create the chart, the function takes in data in a certain format. So I first did a value counts to get the quantitative values, then manually feed them into the function.

<img src="https://miro.medium.com/max/1400/1*6lQe8t-Wt7Hmgcj12qY8Uw.webp">

Screenshot by Author

```python
# Importing the libraries
import sys
sys.path.append("..")
from roughviz.charts.donut import Donut

# Creating donut plot
donut = Donut(data={"labels": ['Mixed Breed', 'Non-Mixed Breed'],
                    "values": [41569, 16578]},
              title="Mixed Breed among Adoptable Dogs in the U.S.",
              title_fontsize=2)
donut.show()
```
https://gist.github.com/m3redithw/318baeebde6829ab84472fdb85ffeffb#file-roughviz_donut-py

Output:

<img src="https://miro.medium.com/max/640/1*4K98ufR9EsaeOLqbm26f_g.gif">

As you see here, simple yet creative aesthetics that also has interactivity. I highly recommend you clone the package’s repo and play with the charts yourself. Make sure you read the documentation and go through the examples.

I hope you find these tools and commands helpful! If you know any other useful/popular visualization tools please leave a comment and I would love to check it out. And feel free to reach out if you have any questions :)

source = https://medium.com/mlearning-ai/visualization-tools-with-python-2d6c6a0d7a02