# 10 Powerful Python One-Liners

Python one-liners can be just as powerful as a long and tedious program written in another language designed to do the same thing.


## 1) Share your Wi-Fi password By Printing it as QR Code

```python
import wifi_qrcode_generator as qr
qr.wifi_qrcode('wifi name ', False, 'WPA', 'password')
```

## 2) CSV to JSON

```python
import csv,json
print (json.dumps(list(csv.reader(open('csv_file.csv')))))
```


## 3) Apply regular expression to lines from stdin

```python
import sys,re
[sys.stdout.write(re.sub('PATTERN', 'SUBSTITUTION', line)) for line in sys.stdin]
```

## 4) Profile a Python script

```python
python3 -m cProfile foo.py
```

## 5) Start a Webserver on your current directory

```python
python -m SimpleHTTPServer 8000
```

## 6) Finding all subsets of a set in one line

```python
from itertools import combinations
print(list(combinations([1, 2, 3, 4], 2)))
```

## 7) Decode a base64 encoded file

```python
import base64, sys 
base64.decode(open(sys.argv[1], "rb"), open(sys.argv[2], "wb"))
```

## 8) Display List of all users on Unix-like systems

```python
print '\n'.join(line.split(":",1)[0] for line in open("/etc/passwd"))
```

## 9) Largest 8-Bytes Number

```python
print '\n'.join("%i Byte = %i Bit = largest number: %i" % (j, j*8, 256**j-1) for j in (1 << i for i in range(8)))
```

## 10) Retrieve content text from HTTP data

```python
import sys 
print sys.stdin.read().replace('\r','').split('\n\n',2)[1]
```

## Final Thoughts
Well, here are my 10 Powerful Python One-Liners that every developer must-have. I hope you find this article helpful and learned some new things. Share this article with your developer friends or who are just starting development.

Have a good day…