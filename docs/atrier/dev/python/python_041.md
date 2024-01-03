# How to Generate Automated PDF Documents with Python

Leveraging automation to create dazzling PDF documents effortlessly

When was the last time you grappled with a PDF document? You probably don’t have to look too far back to find the answer to that question. We deal with a multitude of documents on a daily basis in our lives and an overwhelmingly large number of those are indeed PDF documents. It is fair to claim that a lot of these documents are tediously repetitive and agonizingly painful to formulate. It is about time we consider leveraging the power of automation with Python to mechanize the tedious so that we may reallocate our precious time to more pressing tasks in our lives.

Mind you, there is absolutely no need to be tech-savvy and what we are going to do here should be trivial enough that our inner unsavvy laymen can tackle in short order. After reading this tutorial you will learn how to automatically generate PDF documents with your own data, charts and images all bundled together with a dazzling look and structure.

Specifically, in this tutorial we will automate the following actions:
+ Creating PDF documents
+ Inserting images
+ Inserting text and numbers
+ Visualizing data



## Creating PDF Documents

```python
pip install FPDF
```

Then import the stack of libraries that we’ll be using to render our document:
```python
import numpy as np
import pandas as pd
from fpdf import FPDF
import matplotlib as mpl
import matplotlib.pyplot as plt
from matplotlib.ticker import ScalarFormatter
```

Subsequently, proceed with creating the first page of your PDF document and set the font with its size and color:

```python
pdf = FPDF(orientation = 'P', unit = 'mm', format = 'A4')
pdf.add_page()
pdf.set_font('helvetica', 'bold', 10)
pdf.set_text_color(255, 255, 255)
```

You can however change the font whenever you like if you need to have various typefaces.

## Inserting Images

The next logical step would be to give our document a background image that sets the structure for the rest of our page. For this tutorial, I used Microsoft PowerPoint to render the formatting for my background image. I simply used text boxes and other visuals to create the desired format and once I was done I grouped everything together by selecting all the elements and hitting Ctrl-G. Finally, I saved the grouped elements as a PNG image by right-clicking on them and selecting ‘save as picture’.

![](https://miro.medium.com/max/2400/1*-Y_0EMtlOEx8GYeIabSW2g.png)

As you can see above, the background image sets the structure for our page and includes space for charts, figures, text, and numbers that will be generated later on. The specific PowerPoint file used to generate this image can be downloaded [here](https://github.com/mkhorasani/Bank_Scan/blob/master/background.pptx).

Subsequently insert the background image into your PDF document and configure its position with the following:

```python
pdf.image('C:/Users/.../image.png', x = 0, y = 0, w = 210, h = 297)
```


Please note that you can insert as many images as you like by extending the method shown above.


## Inserting Text and Numbers

Adding text and numbers can be done in two ways. We can either specify the exact location we want to place the text:

```python
pdf.text(x, y, txt)
```

Or alternatively, we can create a cell and then place the text within it. This method would be more suitable for aligning or centering variable or dynamic text:
```python
pdf.set_xy(x, y)
pdf.cell(w, h, txt, border, align, fill)
```

Please note that in the methods above:

+ `x` and `y` refer to the specified location on our page
+ `w` and `h` refer to the dimensions of our cell
+ `txt` is the string or number that is to be displayed
+ `border` indicates if a line must be drawn around the cell (0: no, 1: yes or L: left, T: top, R: right, B: bottom)
+ `align` indicates the alignment of the text (L: left, C: center, R: right)
+ `fill` indicates whether the cell background should be filled or not (True, False).


## Visualizing Data

In this part, we are going to create a bar chart that will display a time-series dataset of our credit, debit, and balance values versus time. For this we will use Matplotlib to render our figures as such:

```python
def bar_chart(credit, debit, balance):

    x = [x[0:6] for (x,y) in balance]
    y1 = [y for (x,y) in credit]
    y2 = [y for (x,y) in debit]
    y3 = [y for (x,y) in balance]

    n = len(x)
    index = np.arange(n)

    fig, ax = plt.subplots(1, 1, figsize=(16,7), dpi= 96)
    plt.plot(x,y3,label='Balance',color='black',linewidth=2.0)
    b1=plt.bar(index-0.0625,y1,label='Credit',color=[(0.06667,0.5647,0.7961)],width=0.125)
    b2=plt.bar(index+0.0625,y2,label='Debit',color=[(1,0.4118,0.4118)],width=0.125)

    plt.xticks(fontsize=14, rotation = 45, horizontalalignment='center',color='darkgrey')
    plt.yticks(fontsize=14,color='darkgrey')
    plt.xlim(-1.0)
    ax.yaxis.grid(alpha=0.5)
    plt.yscale('log', basey=1.00001)

    for axis in [ax.yaxis]:
        axis.set_major_formatter(ScalarFormatter())

    plt.gca().spines["top"].set_alpha(0)
    plt.gca().spines["bottom"].set_alpha(0.5)
    plt.gca().spines["right"].set_alpha(0)
    plt.gca().spines["left"].set_alpha(0)
    plt.savefig('fig1.png',orientation='portrait',transparent=True, bbox_inches=None, pad_inches=0)
```

In the snippet above, credit, debit, and balance are 2-dimensional lists with values for date and transaction amount respectively. Once the chart is generated and saved, it can then be inserted into our PDF document using the method shown in the previous sections.

Similarly, we can generate donut charts with the following snippet of code:

```python
def donut_chart(value):

    if value >= 80:
        color = [0.4392,0.6784,0.2784]
    if (value < 80) and (value >= 60):
        color = [0.9569,0.6941,0.5137]
    if value < 60:
        color = [1.0000,0.4118,0.4118]
    
    my_circle=plt.Circle( (0,0), 0.8, color=[0.4902,0.8000,1.0000])

    if value > 0:
        values = [value, 100 - value]
        plt.pie(values,
                wedgeprops = { 'linewidth' : 0, 'edgecolor' : 'white' }, colors=[color,[1,1,1]],labeldistance=1.1)

    if value < 0:
        values = [-value, 100 + value]
        plt.pie(values,
                wedgeprops = { 'linewidth' : 0, 'edgecolor' : 'white' }, colors=[color,color_b],labeldistance=1.1, counterclock=False)

    p=plt.gcf()
    p.gca().add_artist(my_circle)
    plt.savefig('fig2.png',orientation='portrait',transparent=True, bbox_inches=None, pad_inches=0)
```

And once you are all done, you can wrap it up by generating the automated PDF document as such:

```python
pdf.output('Automated PDF Report.pdf')
```

## Conclusion
And there you have it, your very own automatically generated PDF report! Now you’ve learned how to create PDF documents, insert text and images into them and you’ve also learned how to generate and embed charts and figures. But you are by no means limited to just that, in fact, you can extend these techniques to include other visuals with multiple page documents too. The sky is truly the limit.

![](https://miro.medium.com/max/2400/1*dp0DYSbGRJgv-NRe7QfjKg.png)

---

Source : https://towardsdatascience.com/how-to-generate-automated-pdf-documents-with-python-55981f4d9e3