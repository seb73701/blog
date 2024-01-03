# 5 Python Automation Scripts I Use Every Day

TL;DR- A quick list of the best Python scripts I use on a daily basis, plus some possible modifications that could be made for other functionalities.

![0_p-zmvfbvdha8z_i-.webp](/assets/img/developpement/python/0_p-zmvfbvdha8z_i-.webp)

## Introduction
Python is a powerful **programming language** that can be used to automate a variety of tasks. Whether you’re developing a **small project** or a **large enterprise app**, Python can help save time and streamline your workflow.

Python is a great language because of its **incredibly simple syntax**. What a decent programmer can code in **10 lines of Python** would take them 20 lines in a language like **Javascript** or **C++**. Here’s an example with a simple web request

```python
import requests 
r = requests.get(“https://www.python.org") 
print(r.status_code)
print(r.text)
```

In comparison, here’s the **Javascript code** that does the exact same thing

```js
fetch(“https://www.python.org")
.then(res => {
  if(res.ok) {
    return res.text();
  } else {
    throw new Error(“HTTP error, status = “ + res.status);
  }
})
.then(text => {
  console.log(text);
})
.catch(error => {
  console.log(error);
});
```

As you can see, the **Python code** is far **easier to digest** than the **Javascript code**, and this makes it ideal for automating repetitive tasks for things like **web scraping**, **data collection**, or **translation**. Here are five of my favorite repetitive tasks that **I automate with Python**

## A URL Shortener

```python
import pyshorteners

s = pyshorteners.Shortener(api_key="YOUR_KEY")

long_url = input("Enter the URL to shorten: ")

short_url = s.bitly.short(long_url)

print("The shortened URL is: " + short_url)
```

The **Pyshorteners** library is one of my favorites when it comes to **URL shortening**, which can be used for all sorts of projects. You’ll need an **API key** for most link shorteners, but they’re usually free unless you anticipate **hundreds of thousands of requests**. I’ve found that APIs like [Bit.ly](http://bit.ly/), [Adf.ly](http://adf.ly/), and [Tinyurl](http://tinyurl.com/) are great for things like **SaaS apps** and [Telegram bots](https://grahamzemel.com/projects/hecker-bot).

## Creating Fake Information

```python
import pandas as pd
from faker import Faker

# Create object
fake = Faker()

# Generate data
fake.name()
fake.text()
fake.address()
fake.email()
fake.date()
fake.country()
fake.phone_number()
fake.random_number(digits=5)

# Dataframe creation
fakeDataframe = pd.DataFrame({‘date’:[fake.date() for i in range(5)],
 ‘name’:[fake.name() for i in range(5)],
 ‘email’:[fake.email() for i in range(5)],
 ‘text’:[fake.text() for i in range(5)]})
print(fakeDataframe)
```

If you ever need to create a fake person, this **faker** library provides you with a Faker class that **automatically** generates an entire person. This script **creates** a couple of different people and **stores them** in a **Data Frame**, which is a slightly more complicated concept. I use these fake people if I ever have to **give out information** to sketchy sites or if I don’t want anything traced back to me.

## Youtube Video Downloader

```python
from pytube import YouTube

link = input("Enter a youtube video's URL") # i.e. https://youtu.be/dQw4w9WgXcQ

yt = Youtube(link)
yt.streams.first().download()

print("downloaded", link)
```

Pretty simple. It uses the **pytube** library to convert any link you provide it into a file, and then downloads it. With five lines of code and no API rate-limiting, you could combine this with another script to transcribe the videos and use [sentiment analysis](https://thegrayarea.tech/tweet-sentiment-analysis-w-textblob-d688a60c6451) to determine what kind of content the video contains.

## NATO Phonetic Alphabet Encryptor

```python
def encrypt_message(message):
   nato_alphabet = {
   ‘A’: ‘Alfa’, ‘B’: ‘Bravo’, ‘C’: ‘Charlie’, ‘D’: ‘Delta’,
   ‘E’: ‘Echo’, ‘F’: ‘Foxtrot’, ‘G’: ‘Golf’, ‘H’: ‘Hotel’,
   ‘I’: ‘India’, ‘J’: ‘Juliet’, ‘K’: ‘Kilo’, ‘L’: ‘Lima’,
   ‘M’: ‘Mike’, ’N’: ‘November’, ‘O’: ‘Oscar’, ‘P’: ‘Papa’,
   ‘Q’: ‘Quebec’, ‘R’: ‘Romeo’, ‘S’: ‘Sierra’, ‘T’: ‘Tango’,
   ‘U’: ‘Uniform’, ‘V’: ‘Victor’, ‘W’: ‘Whiskey’, ‘X’: ‘Xray’,
   ‘Y’: ‘Yankee’, ‘Z’: ‘Zulu’
   }

   encrypted_message = “”
  
# Iterate through each letter in the message
   for letter in message:
   
  # If the letter is in the dictionary, add the corresponding codeword to the encrypted message
     if letter.upper() in nato_alphabet:
     encrypted_message += nato_alphabet[letter.upper()] + “ “
   
  # If the letter is not in the dictionary, add the original letter to the encrypted message
     else:
     encrypted_message += letter

  return encrypted_message


message = "Hello World"
encrypted_message = encrypt_message(message)
print("Encrypted message: ", encrypted_message)
```

This function encodes any message passed into its input parameter, and outputs the corresponding NATO word sequence. This works correctly because it checks if each character is in the nato_alphabet dictionary, and if it is, it will be appended to the encrypted message. If the character is not found within the dictionary (if it’s a space, colon, or anything that isn’t a-z), it will be appended without any special encoding. Thus, “Hello World” becomes “Hotel Echo Lima Lima Oscar” “Whiskey Oscar Romeo Lima Delta”.

## Social Media Login Automation

```python
from selenium import webdriver

driver = webdriver.Firefox()
driver.get(“https://www.facebook.com/")

# Find the email or phone field and enter the email or phone number
email_field = driver.find_element_by_id(“email”)
email_field.send_keys(“your_email_or_phone”)

# Find the password field and enter the password
password_field = driver.find_element_by_id(“pass”)
password_field.send_keys(“your_password”)

# Find the login button and click it
login_button = driver.find_element_by_id(“loginbutton”)
login_button.click()
```

This code utilizes Selenium, a popular library for web automation. It opens a web browser and navigates based on a variety of commands given in the code. In this particular code block, the browser will go to Facebook, and find specific elements to modify on the web page. Here, we’re entering certain keystrokes into the email and password field, and clicking the ‘login’ button. This will automatically log in a user if valid credentials are supplied.

Thanks for reading about automation in Python! If you’d like to see more posts on computer science and cybersecurity, check out The Gray Area.

The Gray Area
For all kinds of developers💻, hackers👾, and tech-savvy readers👨‍💻 | Free newsletters each Wednesday on the newest…
thegrayarea.tech

If you’d like to support me as a writer, subscribe to a Medium membership using my referral link (giving you access to all of my posts, and all of everyone else’s posts on Medium!) →

Join Medium with my referral link - Graham Zemel
Read all of Graham Zemel's posts, and any other post from thousands of Medium writers! You'll get full access to every…

source : https://medium.com/codex/5-python-automation-scripts-to-use-every-day-909b057cd71