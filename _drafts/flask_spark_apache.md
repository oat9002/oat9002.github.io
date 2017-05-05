---
layout: post
title: Flask Spark Apache
date: '2017-05-5 16:28:01 +0700'
categories: tutorial
---

# Hi!

Now I have a project about a text mining. I think python would be a proper language because there is a lot of libraries to use in this field. I have to concern how to dead with a lot of data which flow to my service. Spark might be a good option to deal with this issue. However, I have not much time to study to create service using python. I have found one framework that easy to implement. It's called 'flask'.

Although, flask is easy to implement. It's not good for using in production. If you want to go for product, It would be better if you deploy with web service, such as, Apache, Nginx. I already had Apache so I go for Apache.

# Let's get start

1. Get a python

   For this instruction, I prefer python 2 because python 3 might have a problem with spark. For windows, you can get via this link <https://www.python.org/> For Linux, you can use theses commands

     - Ubuntu
     > sudo apt-get install python

     - Arch Linux
     > sudo pacman -s python2

2. Install package with pip
   > pip install flask

3. Create some service with spark

   - Create file `people.json`
      ```json
      {"name": "Jame"}
      {"name": "John"}
      ```
   - Create file `hello.py`
      ```python
      from flask import Flask
      from pyspark.sql import *

      app = Flask(__name__)
      spark = SparkSession \
          .builder \
          .appName("Example") \
          .getOrCreate()

      @app.route('/')
      def hello_world():
        return 'Hello, World!'

      @app.route('/john')
      def hello_john():
        df = spark.read.json('./people.json')
        name = df.where(df.name == 'John').select(df.name).collect()[0].asDict()['name']
        return 'Hello, ' + name + '!'

      @app.route('/jame')
      def hello_jame():
        df = spark.read.json('./people.json')
        name = df.where(df.name == 'Jame').select(df.name).collect()[0].asDict()['name']
        return 'Hello, ' + name + '!'

      if __name__ == "__main__":
        app.run(host='0.0.0.0',port=3000)
      ```

- You can edit port at `app.run()`

> Test this service by using command `python hello.py` it will show that service is running on port 3000

Finish in flask part, about Spark I do not tell how to install it. There is a lot of things to set it up. You can find guides on its official website. Also for Apache.

# Let's connect flask to Apache

Before you can connect to Apache, You have to install `mod_wsgi`. This thing will make flask can connect to Apache.

1. Install mod_wsgi

   [How to intall](http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/)

2. Create `hello.wsgi` file in your project's directory and config like this
    ```python
    import sys
    import os
    sys.path.insert(0, path_to_your_directory)
    sys.path.append('path_to_pyspark')
    os.environ['SPARK_HOME'] = path_to_spark_home
    from your_flask_application import app as application
    ```

3. Open `httpd.conf` in `/etc/httpd/conf/httpd.conf` and add following this line
    ```
    <VirtualHost *>
        ServerName localhost

        WSGIDaemonProcess your_application_name home=path_to_your_application user=your_user group=your_group threads=5
        WSGIScriptAlias / path_to_your_applicatio.wsgi

        <Directory path_to_your_application>
            WSGIProcessGroup your_application_name
            WSGIApplicationGroup %{GLOBAL}
            Order deny,allow
            Allow from all
        </Directory>
    </VirtualHost>
    ```

   - if you have httpd 2.4, change
      ```
      Order deny,allow
      Allow from all
      ```
   to
      ```
      Require all granted
      ```

4. Open a browser an type `http://localhost` you will see hello world
