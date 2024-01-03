# 7 Must-Try GUI Libraries in Python
Tools to help you build interactive GUIs screen design templates sitting on a desk next to a book on how to design graphical interfaces

![](https://miro.medium.com/max/1400/1*uhNMoPjt8uLnaWxsnVHKMQ.jpeg)

*~Photo_by_UX_Store_on_Unsplash~*

The GUI or graphical user interface is the one thing that the user sees and interacts with when they open your application. A good-looking GUI increases the reputation of your product. In this article, I will introduce you to seven GUI libraries in Python that you must try.

## 1. PyQt5
[PyQt5](https://www.riverbankcomputing.com/software/pyqt/) was developed by Riverbank Computing. It is built around the Qt framework, which is a cross-platform framework used for creating applications for various platforms. PyQt brings together Qt and Python. It is more than a GUI toolkit. It includes threads, Unicode, regular expression, SQL databases, SVG, OpenGL, XML, and a fully functional web browser, as well as many rich collections of GUI widgets.

**Installation**: 

```shell
pip install PyQt5
```

Let’s see a simple example of a GUI window using the library.

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget,QLabel, QVBoxLayout
app = QApplication(sys.argv)
w = QWidget()
w.resize(500,500)
w.layout = QVBoxLayout()
w.label = QLabel("Hello World!")
w.label.setStyleSheet("font-size:25px;margin-left:155px;")
w.setWindowTitle("PyQt5 Window")
w.layout.addWidget(w.label)
w.setLayout(w.layout)
w.show()
sys.exit(app.exec_())
```
*A simple window code using PyQt5*

![](https://miro.medium.com/max/491/1*dlqei-mzVAhLb8Uc7DiKjw.png)
*PyQt5 simple GUI (Image by Author)*

Here are the [full documentation](https://riverbankcomputing.com/software/pyqt/intro) and a [tutorial link](https://www.guru99.com/pyqt-tutorial.html) to get you started with the library.

## 2. Tkinter
[Tkinter](https://docs.python.org/3/library/tkinter.html) is one of the most popular GUI libraries in Python. It is one of the first choices for beginners to GUI development because of its simple and easy-to-learn syntax. Tkinter provides diverse widgets such as labels, buttons, text fields, checkboxes, and scroll buttons. It supports a grid system which makes making a complex design very easy to develop.

**Installation**: 

```
pip install tkinter
```

Here is a simple GUI using Tkinter that takes weight and height as input and returns the BMI as output in a popup box.

```python
'''
author : abhayparashar31
This Program Take Height(CM) and Weight(KG) and Returns The BMI(Body Mass Index) Value As Output In a Pop up Box.
'''
from tkinter import *
from tkinter import messagebox
def get_height():
    height = float(ENTRY2.get())
    return height
def get_weight():
    weight = float(ENTRY1.get())
    return weight
def calculate_bmi(): 
    try:
        height = get_height()
        weight = get_weight()
        height = height / 100.0
        bmi = weight / (height ** 2)
    except ZeroDivisionError:
        messagebox.showinfo("Result", "Please enter positive height!!")
    except ValueError:
        messagebox.showinfo("Result", "Please enter valid data!")
    else:
        messagebox.showinfo("Your BMI Calculated is : ", bmi)

if __name__ == '__main__':
    TOP = Tk()
    TOP.bind("<Return>", calculate_bmi)
    TOP.geometry("400x400")
    TOP.configure(background="#8c52ff")
    TOP.title("BMI Calculator")
    TOP.resizable(width=False, height=False)
    LABLE = Label(TOP, bg="#8c52ff",fg="#ffffff", text="Welcome to BMI Calculator", font=("Helvetica", 15, "bold"), pady=10)
    LABLE.place(x=55, y=0)
    LABLE1 = Label(TOP, bg="#ffffff", text="Enter Weight (in kg):", bd=6,
                   font=("Helvetica", 10, "bold"), pady=5)
    LABLE1.place(x=55, y=60)
    ENTRY1 = Entry(TOP, bd=8, width=10, font="Roboto 11")
    ENTRY1.place(x=240, y=60)
    LABLE2 = Label(TOP, bg="#ffffff", text="Enter Height (in cm):", bd=6,
                   font=("Helvetica", 10, "bold"), pady=5)
    LABLE2.place(x=55, y=121)
    ENTRY2 = Entry(TOP, bd=8, width=10, font="Roboto 11")
    ENTRY2.place(x=240, y=121)
    BUTTON = Button(bg="#000000",fg='#ffffff', bd=12, text="BMI", padx=33, pady=10, command=calculate_bmi,
                    font=("Helvetica", 20, "bold"))
    BUTTON.grid(row=5, column=0, sticky=W)
    BUTTON.place(x=115, y=250)
    TOP.mainloop()
```

![](https://miro.medium.com/max/488/1*GMheIN2YPz6quVuJ3vKY2Q.gif)
*GIF showing the calculator working (Image by Author)*

## 3. Kivy
[Kivy](https://kivy.org/#home) is another open source Python library for the rapid development of applications that make use of multi-touch apps and innovative user interfaces. Kivy runs on different platforms, including Windows, OS X, Android, and Raspberry Pi. It is free to use and comes under an MIT license. The toolkit comes with more than 20 widgets.

**Installation**: 

```
pip install Kivy
```


```python
from kivy.app import App
from kivy.uix.button import Button
 
class TestApp(App):
    def build(self):
        return Button(text= " Hello Kivy World ")
 
TestApp().run()
```

![](https://miro.medium.com/max/2400/1*SLet_WQbiMUkWiL5I4JR0w.png)
*Kivy basic window with Hello World! message*

## 4. wxPython
wxPython is a cross-platform GUI toolkit for Python that allows the creation of highly robust functional GUIs with less effort. It is written in C++. Currently it supports Windows, Mac OS X, macOS, and Linux.

**Installation**: 

```
pip install wxPython
```

**Documentation Link**: [wx Python official website](https://www.wxpython.org/)

**Tutorial**: [Hotboytrue YouTube playlist](https://www.youtube.com/watch?v=RHvhfjVpSdE&list=PLejTrt5hn2r1uzZ53GDeUElXRkRFbUmQd)

Following is an abasic GUI example using wxPython. The below code will create a simple empty window.

```python
import wx
myapp = wx.App()
init_frame = wx.Frame(parent=None, title='WxPython Window')

init_frame.Show()
myapp.MainLoop()
```
*Basic wxPython window code — a starter template for you*

## 5. PySimpleGUI
PySimpleGUI is a GUI framework for Python which supports Python 3. It makes it so easy to make custom and attractive GUIs. It takes four of the most widely popular GUI frameworks — QT, Tkinter, WxPython, and Remi — and turns down their difficulty a few notches by implementing most of the boilerplate code.

**Installation**: 

```
pip install PySimpleGUI
```

Read the [PySimpleGUI documentation](https://pysimplegui.readthedocs.io/en/latest/) to get a good insight into the library.

## 6. PyGUI
PyGUI is a GUI framework mostly famous for its simple API that enables developers to create user interfaces using native elements. It is a very lightweight API that makes your application run smoother and faster. It is an open source, cross-platform project that currently runs on Unix-based systems, Windows machine,s and Mac OS devices. It is available for Python 2 and 3 both.

**Installation**: 

```
pip install PyGUI
```

**Documentation**: [PyGUI official docs](https://www.cosc.canterbury.ac.nz/greg.ewing/python_gui/)

Here is a simple window creation using PyGUI.

```python
import PySimpleGUI as sg
layout = [[sg.Text("Hello from PySimpleGUI")], [sg.Button("OK")]]
window = sg.Window("Demo", layout)
while True:
    event, values = window.read()
    if event == "OK" or event == sg.WIN_CLOSED:
        break
window.close()
```
*Simple boilerplate template for a GUI built using PyGUI*

Here is a [complete tutorial](https://realpython.com/pysimplegui-python/) to get you started with the library.

## 7. Pyforms
Pyforms is a cross-platform framework for developing GUI applications. It offers a Python layer of desktop forms based on PyQt, OpenGL, and other libraries, and it allows applications to run on the desktop, web, and terminal without any requirement for modification of the code.

**Installation**: 

```
pip install PyFroms
```
**Documentation**: [Pyforms official docs](https://pyforms.readthedocs.io/en/v4/)

Source : https://betterprogramming.pub/7-must-try-gui-libraries-in-python-34c8f6266363