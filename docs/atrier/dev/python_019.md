# 12 Python Snippets That Will Boost Your Productivity
A list of 12 ‘how to’ scripts in Python.

## Introduction
I love Python snippets. I just love sitting down in from of an ipython shell and write simple and useful code. So, today I want to share some of the many Python snippets I wrote, gathered or found throughout my pythonic explorations.

### 1. How to get combinations of n numbers in a list

```python
from itertools import combinations
lista = [1,2,3,4,5]
print(list(combinations(lista,2)))
# Output
[(1, 2), (1, 3), (1, 4), (1, 5), (2, 3), (2, 4), (2, 5), (3, 4), (3, 5), (4, 5)]
```


### 2. How to get computer information on a windows machine

```python
import win32api
def printInfo():
    device = win32api.EnumDisplayDevices()
    print((device.DeviceName, device.DeviceString))
    settings = win32api.EnumDisplaySettings(device.DeviceName, -1)
    for varName in ['Color', 'BitsPerPel', 'DisplayFrequency']:
        print("%s: %s"%(varName, getattr(settings, varName)))
```

### 3. How to write a simple gaussian kernel

```python
import numpy as np
from scipy import signal
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
def gkern(kernlen=21, std=3):
    """Returns a 2D Gaussian kernel array."""
    gkern1d = signal.gaussian(kernlen, std=std).reshape(kernlen, 1)
    gkern2d = np.outer(gkern1d, gkern1d)
    return gkern2d
```

### 4. How to get a list of colors in seaborn

```python
import seaborn as sns
number_of_colors = 10
colors = sns.color_palette("hls", number_of_colors)
print(colors)
# Output:
[(0.86, 0.3712, 0.33999999999999997), (0.86, 0.6832, 0.33999999999999997), (0.7247999999999999, 0.86, 0.33999999999999997), (0.41279999999999994, 0.86, 0.33999999999999997), (0.33999999999999997, 0.86, 0.5792000000000002), (0.33999999999999997, 0.8287999999999999, 0.86), (0.33999999999999997, 0.5167999999999995, 0.86), (0.4752000000000003, 0.33999999999999997, 0.86), (0.7871999999999999, 0.33999999999999997, 0.86), (0.86, 0.33999999999999997, 0.6207999999999999)]
```

### 5. How to get the most frequent elements from a list with Counter

```python
from collections import Counter
import numpy as np
random_elements = np.random.randint(0,10,100)
dict_freq = Counter(random_elements)
# Output
Counter({7: 15, 4: 9, 3: 9, 2: 11, 0: 5, 1: 12, 8: 10, 5: 11, 6: 10, 9: 8})
```

### 6. How to plot a 3D surface of the softmax function

```python
'''
======================
3D surface (color map)
======================
Plotting a softmax function in 3D
'''
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
import numpy as np
#Creating the figure
fig = plt.figure()
ax = fig.gca(projection='3d')
#Mking the data
X = np.arange(-5, 5, 0.25)
Y = np.arange(-5, 5, 0.25)
X, Y = np.meshgrid(X, Y)
#Softmax
Z = (np.exp(X)/ (np.exp(X)+ np.exp(Y)))
#Ploting the surface.
surf = ax.plot_surface(X, Y, Z, cmap=cm.coolwarm,
                       linewidth=10)
#Customize the z axis setting it t0 the range(0,1)
ax.set_zlim(0, 1.0)
# Add a color bar which maps values to colors.
fig.colorbar(surf, shrink=0.5, aspect=5)
plt.show()
```

Output:

![](https://miro.medium.com/max/640/1*WFRXd-thHt5XGQ3B2CsfHQ.png)

image by author


### 7. How to write a gaussian white noise stimulus vector
```python
def get_white_noise_stim_vector(dims, ch, n):
    # We will have as many stimuli as time_bins so there will be   
    # overlap
    stim_vector = np.zeros((dims, dims, ch, n))
    for i in range(stim_vector.shape[-1]):
        # Gaussian noise vector of dimension k for the time window    # 200(milliseconds)
        stim_vector[:, :, :, i] = np.random.normal(0.0, 1.0, size=       (dims, dims, ch))
    return stim_vector
```

### 8. How to get the memory size of globals() output in GB
```python
def check_sizes(gb_min=1):
    for x in globals():
        size = sys.getsizeof(eval(x)) / 1e9
        if size > gb_min:
            print(f'Object: {x:10}\tSize: {size} GB.')
9. How to interleave lists
a = [1,2,3]
b = [4,5,6]
interleaved_list = [val for pair in zip(a,b) for val in pair]
print(interleaved_list)
# Output
[1, 4, 2, 5, 3, 6]
```


### 10. How to make fake cluster data
```python
import numpy as np
from sklearn.cluster import KMeans
from sklearn import metrics
import matplotlib.pyplot as plt
from sklearn.datasets import make_blobs
import seaborn as sns
sns.set()
# First I want to remember how to apply kmeans
 #Make features and targets with 1000 samples,
features, target = make_blobs(n_samples=1000, n_features=2,centers=10,
                              cluster_std=0.60, shuffle=True)
#Now we visualize our data
colors = sns.color_palette("hls", 10)
plt.scatter(features[:,0], features[:,1], label="Features") 
plt.legend()
plt.show()
```

Output

![](https://miro.medium.com/max/640/1*mhW007b93sRS3TPwZAuvLQ.png)

image by author


### 11. How to place a label outside of a plot in matplotlib
```python
import matplotlib.pyplot as plt
w = 4
h = 3
d = 70
plt.figure(figsize=(w, h), dpi=d)
y1 = [2, 3, 4.5]
y2 = [1, 1.5, 5]
plt.plot(y1)
plt.plot(y2)
ax = plt.subplot(111)
box = ax.get_position()
ax.set_position([box.x0, box.y0, box.width*0.65, box.height])
legend_x = 1
legend_y = 0.5
plt.legend(["blue", "green"], loc='center left', bbox_to_anchor=(legend_x, legend_y))
plt.savefig("out.png")
plt.show()
```

Output

![](https://miro.medium.com/max/280/1*j5-myu0_nvZkurXOmZogyQ.png)

image by author

### 12. How to put a rectangle on a plot in matplotlib
```python
import matplotlib.pyplot as plt
plt.axes()
rectangle = plt.Rectangle((10, 10), 100, 100, fc='r', fill=False,edgecolor="r")
plt.gca().add_patch(rectangle)
plt.axis('scaled')
plt.show()
```

Output

![](https://miro.medium.com/max/640/1*C_jQOgaOjeKaH1aLGZ13GA.png)

image by author

## Conclusion on Python Snippets
This post was aimed at just providing Python scripts for you to play around with, as well as showcase the amazing space of possibilities that Python offers from accessing system information to doing numeric computing and plotting.
Scripting simplistic tools is a great way to find out all that Python can do for you and it has been something that I do weekly and take a lot of pleasure in.

Source: https://python.plainenglish.io/python-snippets-7e8dcbeae26e