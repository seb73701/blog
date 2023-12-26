# 10 Python Scripts for Everyday Automation

A handy collection of scripts for your daily life tasks and problems

In **Everyday life**, we do a lot of tasks some of them are repetitive and some of them need handmade scripts. Think of some tasks like **Parsing CSV files**, **Fetching daily headlines**, **Editing Photos**, **Sending Mail**, and much more. This task can now be automated with Python. In this article, I will show you **10 Python Scripts for the Daily Automation of your task**. So put this article on your list and let's get started.

> It always seems impossible until its done…
> 
> — Nelson Mandela

## Parse CSV
No need to install extensive modules like Pandas when you have a built-in excellent CSV parsing library. In the below python script, I will show you how you can read and write CSV without any external module.

This script is very handy when you need a lightweight module for reading and writing multiple CSV files.

- Light-weight module for CSV
- Can be used in your Python Projects

```python
# Parse CSV
import csv
def Parse_CSV(filename):
    with open(filename, 'rb') as csvfile:
        reader = csv.reader(csvfile, delimiter=',', quotechar='|')
        for row in reader:
            print(', '.join(row))
def Write_CSV():
    with open('test.csv', 'wb') as csvfile:
        w = csv.writer(csvfile, delimiter=',')
        w.writerow(['Name', 'Age'])
        w.writerow(['John', '23'])
        w.writerow(['Mary', '22'])
Parse_CSV("test.csv")
Write_CSV()
```


## Compress Large Files
This automation script uses the built-in **Zip file module** that helps you to make your **large files smaller** by compressing their sizes. Below you can find the script that can compress multiple files in a single zip file.

- Compress multiple Files
- Can be used for Decompressing files
- Much more

```python
# Compress Large Files
import zipfile as Zip
def Compressor(files):
    zip = Zip.ZipFile("output.zip", "w", Zip.ZIP_DEFLATED)
    for f in files:
        zip.write(f, compress_type=Zip.ZIP_DEFLATED)
    zip.close()
    print("Compressed")
Compressor(["video.mkv", "image.jpg"])
```

## Share Files with QR
Need to share files with someone in an easier way. Then this automation script will help you to create a **QR code** of your file that you can share with anyone and when someone scans the Qrcode the file that you share is now **downloadable**.

- Share Files with anyone
- Use in your project
- Share any file format
- Much more

```python
# Share File QrCode
# pip install share-file-qr
# pip install qrcode
import subprocess as proc
import qrcode
def Share_Files_Qr(file):
    qr = qrcode.QRCode(
        version=1,
        box_size=10,
        border=4,
    )
    qr.add_data(f"http://192.168.0.105:4000/file/{file}")
    qr.make(fit=True)
    qr.make_image().save("qrcode.png")
    proc.call(f"share-file-qr {file}", stderr=proc.STDOUT)
Share_Files_Qr("test.png")
```


## Python Photoshop
Edit your Photos in a **Pythonic way** by using the below automation script that will let you Photoshop your photos programmatically. This script is handy when you are working on **an image processing project** or need a hand to edit bulk images like cropping, flipping, enhancing the quality, compressing the size, and much more.

- Bulk Photo Editing
- Can be used in Projects
- much more

```python
# Python Photoshop
# pip install Pillow
from PIL import Image
import PIL
# loading image
im = Image.open("python.png")
# get image info
print(im.format, im.size, im.mode)
# Crop 
img = im.crop((0, 0, 100, 100))
img.save("cropped.png")
# Resize
img = im.resize((100, 100), Image.ANTIALIAS)
img.save("resized.png")
# Rotate
img = im.rotate(180)
img.save("rotated.png")
# Flip
img = im.transpose(Image.FLIP_LEFT_RIGHT)
img.save("flip.png")
# Compress size
img.save("python.png", optimize=True, quality=95)
# Greyscale
img = im.convert('L')
img.save("grey.png")
# Enhance image
enhancer = PIL.ImageEnhance.Contrast(im)
enhancer.enhance(1.3)
img.save("enhanced_image.png")
# Blur
img = im.filter(PIL.ImageFilter.BLUR)
img.save("bluring.png")
# Drop shadow Effect
img = im.filter(PIL.ImageFilter.GaussianBlur(2))
img.save("drop_shadow.png")
# Increase brightness
img = im.point(lambda b: b * 1.2)
img.save("bright.png")
# merge two images
img2 = Image.open("python2.png")
img = Image.blend(im, img2, 0.5)
img.save("merge.png")
```


## Fetch Top headlines
Keep yourself up-to-date every day by fetching **top news headlines** with this python script. This script scrapes daily fresh news from the **NBCnews** website and provides you the headline and full article URL.

- Create News App
- Use in your News project
- Much more

```python
# Fetch Top Headlines
# pip install requests
# pip install bs4
import requests
from bs4 import BeautifulSoup
url = "https://www.nbcnews.com/"
response = requests.get(url)
html = BeautifulSoup(response.text, "html.parser")
for news in html.find_all("h3" ):
    print("Headline:", news.text)
    print("Link:", news.find("a")["href"])
```


## Control keyboard and Mouse
**Simulate the actions** of your keyboard and mouse programmatically by using these Python scripts. This killer script uses the **mouse and keyboard module** and lets you control them by moving the mouse, clicking keys, pressing and typing, etc.

- Can use to automate desktop
- Can be used in Projects
- Make mouse actions
- Much more

```python
# Control keyboard and Mouse
# pip install mouse
# pip install keyboard
import mouse
import keyboard
# Move mouse cursor
mouse.move(100, 100, duration=0.5)
# Click mouse left btn
mouse.click('left')
# Click mouse right btn
mouse.click('right')
# Click mouse middle btn
mouse.click('middle')
# Drag mouse
mouse.drag(100, 100, duration=0.5)
# get mouse position
mouse.get_position()
# Press keyboard key
keyboard.press('a')
# Release keyboard key
keyboard.release('a')
# Type on keyboard
keyboard.write('Python Coding')
# Press and release keyboard key
keyboard.press_and_release('a')
# Combination of keys
keyboard.add_hotkey('ctrl + shift + a')
```

## Web Status Checker
Check the current status of any website by this automation script that uses the **Urllib3 module** that sends the **HTTP request** to the website and in response we can get the current web status.

```python
# Web Status Checker
# pip install urllib3
import urllib3
import time
web = "https://www.medium.com"
http = urllib3.PoolManager()
response = http.request('GET', web)
if response.status == 200:
    print("Web is online")
else:
    print("Web is offline")
```    

## Tkinter GUI
Create a beautiful and modern GUI with **Tkinter** built-in module. This Python original module lets you create a modern **eye-catching GUI** App with simple coding.

```python
# Tkinter GUI
from tkinter import *
pygui = Tk()
# Set Screen Title
pygui.title("Python GUI APP")
# Set Screen Size
pygui.geometry("1920x1080")
# Set Screen Bg Color
pygui.configure(bg="White")
# Set Screen Icon
pygui.iconbitmap("icon.ico")
# Add Label Text
Text = Label(pygui, text="Medium", font=("Arial", 50))
Text.place(x=100, y=100)
# Add Buttons
btn = Button(pygui, text="Hi! Click Me", font=("Arial", 20))
btn.place(x=100, y=200)
# Add Input Box
var = StringVar()
input = Entry(pygui, textvariable=var, font=("Arial", 20))
input.place(x=100, y=300)
# Add Text Box
text = Text(pygui, width=50, height=10)
text.place(x=100, y=400)
# Add Radio btns
var = IntVar()
radio1 = Radiobutton(pygui, text="Option 1", variable=var, value=1)
radio1.place(x=100, y=500)
# Add Checkboxes
var = IntVar()
check = Checkbutton(pygui, text="Check Me", variable=var)
check.place(x=100, y=600)
# Add Image
img = PhotoImage(file="image.png")
image = Label(pygui, image=img)
image.place(x=100, y=600)
# loop
pygui.mainloop()
```


## Tiktok Downloader
Now you can download your favorite Tiktok videos **programmatically**. The below automation script will make it easier for you to download a bunch of TikTok videos with few lines of code.

- Create a TikTok downloader app
- Use in Python Projects
- Download bulk videos
- Much more

```python
# Tiktok Video Downloader
# pip install selenium 
# pip install webdriver-manager
# pip install beautifulsoup4
from selenium import webdriver as web
from webdriver_manager.chrome import *
import time
from bs4 import BeautifulSoup as bs
import urllib.request
def tiktok_downloader(url):
    op = web.ChromeOptions()
    op.add_argument('--headless')
    driver = web.Chrome(ChromeDriverManager().install(), options=op)
    driver.get(url)
    time.sleep(5)
    html = driver.page_source
    soup = bs(html, 'html.parser')
    video = soup.find('video')
    urllib.request.urlretrieve(video['src'], 'video.mp4')
tiktok_downloader("https://www.tiktok.com/video/xxxx")
```

## Request HTML
Fetch the static and dynamic webpages with this awesome script that uses the **requests_html module** which is a modified version of the **request module** for fetching **JS load webpages**.

```python
# Request HTML
# pip install requests_html
from requests_html import HTMLSession
url = 'https://www.google.com/search?q=python'
session = HTMLSession()
req = session.get(url, timeout=20)
req.html.render(sleep=1)
print(req.status_code)
print(req.html.html)
```

Source : https://python.plainenglish.io/10-python-scripts-for-everyday-automation-57b75785aaf