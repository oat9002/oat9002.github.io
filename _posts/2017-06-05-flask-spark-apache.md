---
layout: post
title: Flask Spark Apache
---
# Hi

In the 4th year of being a student of university, I had a senior project which was about a text mining. I had to create a system that could predict an emotion from texts. Texts were from social media, such as, Twitter. So, I thought python would be a proper language for this kind of work because there were a lot of libraries to use in this field. I also had to concern how to deal with a lot of data which flew to my service. Spark might be a good option to deal with this issue. However, I had not much time to study to create service using python. Fortunately, I had found one framework that easy to implement. It's called 'flask'.

Although, flask is easy to implement. It's not good for using in production. If you want to go for product, It would be better if you deploy with web service, such as, Apache, Nginx. I already had Apache so I went for Apache.

*For location for the files, It shouldn't be in home(Linux) because it might have a problem about a permission in Apache*

# Let's get start

1. Get a python 2

   For this instruction, I prefer python 2 because python 3 might have a problem with spark. For windows, you can get via this [link](https://www.python.org/). For Linux, you can use theses commands

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

4. Install Apache
   - Ubuntu
   > sudo apt-get apche2

   - Arch Linux
   > sudo pacman -S Apache

   You can test by open a browser an go to `http://localhost`. It will show Apache's homepage

5. Install Spark

   Download pre-built version from [official website](http://spark.apache.org/downloads.html) and place somewhere


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
    os.environ['SPARK_HOME'] = path_to_spark
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

4. Open a browser an type `http://localhost` you will see hello world and you can test other routes that we've just created
