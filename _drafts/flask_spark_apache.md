---
layout: post
title:  "Flask Spark Apache"
date:   2017-04-2- 16:16:01 +0700
categories: jekyll update
---

# Hi!

Now I have a project about a text mining. I think python would be a proper language because there is a lot of libraries to use in this field. I have to concern how to dead with a lot of data which flow to my service. Spark might be a good option to deal with this issue. However, I have not much time to study to create service using python. I have found one framework that easy to implement. It's called 'flask'.

Although, flask is easy to implement. It's not good for using in production. If you want to go for product, It would be better if you deploy with web service, such as, Apache, Nginx. I already had Apache so I go for Apache.

# Let's get start
1. Get a python

   In linux, It is normally built-in with the os. For windows, you can get via this link [https://www.python.org/](https://www.python.org/)

2. Install package with pip
> pip install flask

3. Create some service

   Create file `hello.py`

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0',port=3000)
```

 - You can edit port at `app.run()`

> Test this service by using command `python hello.py` it will show that service is running on port 3000

Finish in flask part, about Spark I do not tell how to install it(lol). There is a lot of things to set it up. You can find guides on its official website. Also for Apache.

# Let's connect flask to apache




