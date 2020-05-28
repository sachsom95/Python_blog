---
layout: post
title:  "flask, templating and inheritance"
date:   2020-05-27 14:56:26 +0100
permalink: chapter2
author: "Sachin Soman"
comments: true
---
## Running the Hello,World in Flask
<hr>
<br>
We all know that Flask is a python based micro-framework to make backend of web applications. I will not go into the basics 
of how this is different from from the like of Django and other server side frameworks instead will focus on some parts 
that needs some attention.



If we go to the official documentation of [flask](https://flask.palletsprojects.com/en/1.1.x/quickstart/) we see the following 
code block.

{% highlight ruby %}
from flask import Flask
app = Flask(__name__)
@app.route('/')
def hello_world():
    return 'Hello, World!'
{% endhighlight %}

First line we import flask class from the Flask module. Then we set an instance of flask on to the the app variable. Now
__name__ is name of module. If we run flask directly from python we have __ name __ == __ main __. This we will look into in next 
few examples. This basically tells flask where to look for all the templates and static files.

Next we define `routes`. Routes are links we have in a website. Basically flask looks at link which we are currenly on to 
do run methods. We will have a clearer understanding after next example.

 {% highlight ruby %}
 @app.route('/hello')
 def say_hello():
     return 'Hello, World!'
     
  @app.route('/about')
  def write_about():
      return 'About'
 {% endhighlight %}
 
 Here we say that if we go to link `/hello` then execute function say_hello() and if we go to link `/about` then should we execute the 
 function write_about(). This feature is made possible by the use of `@app.route()` <b>decorator</b> which will take care of all the
 complicated back-end stuff to bind functions with different links. 
 
 So coming back to the original example. In the example we see the line `@app.route('/')` the route('/') simply means the homepage of the website.
 So in our homepage run function hello_world(). Now that we made the web-app we need to run it.
 
 Now to run flask there are two ways. I'll explain both the ways and some potential error you can face when doing so. In method 1 we
 open up the terminal go to directory where you saved the flask python file. Now we need to set an environmental variable by typing:
  {% highlight ruby %}
export FLASK_APP=YOUR_FLASK_APP_NAME.py
  {% endhighlight %}
  
  <b>NOTE:</b> One thing to note is that you shouldn't leave any space between the '=' operator or you will get bad assignment error<br>
  In windows if you use command prompt it is :
   {% highlight ruby %}
  C:\path\to\app>set FLASK_APP=YOUR_FLASK_APP_NAME.py
   {% endhighlight %}
   
with the environmental variable set we can now type in terminal:

   {% highlight ruby %}
flask run
   {% endhighlight %}
   
   if all went well we should get something like this
    <br>
    
   ![image]({{site.github.url}}/assets/images/flask_run.png)*Flask server starting up*
   
   We see that the flask app is being server in ip address 127.0.0.1:5000 at port 5000. Sometimes we get an error that 
   the port is already in use. This basically means some other application is using the port and we need to terminate it.
   or we can bind flask to some other ports check this [link](https://pybit.es/flask-ports.html) on how you can do that.
   
   So to view your app paste the IP address into your browser.
   
   Now method the next method involves running flask directly from python without setting up the environmental variable. This 
   involves adding a few extra lines to our original code.
   
   {% highlight ruby %}
   from flask import Flask
   app = Flask(__name__)
   @app.route('/')
   def hello_world():
       return 'Hello, World!'
       
   if __ name __ == __ main __:
        app.run()
   {% endhighlight %}

When we write the code we must ensure that the flask app is run directly. Now type: 
   {% highlight ruby %}
python YOUR_FLASK_APP.py
   {% endhighlight %}
   
We get to run the app directly. This is not the recommended way when we bring the app into production.

Now that we have the app running we will go into the next important topic in Flask templating
<br>
<br>

## Templating
<hr>
<br>
Templating is by far the most important feature when we are making a flask app. Whats the point of designing 
fancy website if you can't render them. Rendering a template in flask is the act of displaying a pre-made web page with essential 
information filled in by flask. Now how is this useful?. Lets look at an example.

   {% highlight ruby %}
   from flask import Flask
   app = Flask(__name__)
   @app.route('/')
   def hello_world():
       return '<h1>Hello, World!</h1>'
       
   if __ name __ == __ main __:
        app.run()
   {% endhighlight %}
   
   If we run this example we can see a website with Hello,World rendered in an <h1> tag is rendered. But a normal website is not that
   simple it can have thousands of lines of markdown, javascript , css and other dependencies. Dont get me wrong you can definitly put
   all the website code into a return statement. Like this.
   
   {% highlight ruby %}
   from flask import Flask
   app = Flask(__name__)
   @app.route('/')
   def hello_world():
       return '<!DOCTYPE html>
               <html lang="en">
               <head>
               <title>Page Title</title>
               <meta charset="UTF-8">
               <meta name="viewport" content="width=device-width, initial-scale=1">
               <style>
               body {
                 font-family: Arial, Helvetica, sans-serif;
               }
               </style>
               </head>
               <body>
               
               <h1>My Website</h1>
               <p>A website created by me.</p>
               
               </body>
               </html>'
       
   if __ name __ == __ main __:
        app.run()
   {% endhighlight %}  
   
 But you will be called a psychopath by your co-workers. Coding like this is inefficent, not reusable, prone to errors and
 difficult to maintain. Instead we use the concept of templates. In templating we make the html file to render in a seprate folder called
 `templates`. After that we can use the following code.
 
   {% highlight ruby %}
   from flask import Flask, render_template
   app = Flask(__name__)
   @app.route('/')
   def hello_world():
       render_template('YOUR_HTML_FILE.html')
       
   if __ name __ == __ main __:
        app.run()
   {% endhighlight %}
  
 Flask will go into the `templates` folder look for the html file and renders it. Now you can see how we made the website 
 much more maintainable by using the template technique. One thing to not before using the command `render_template()` is to 
 import `render_template` from flask package as shown in first line of the example.
 
 Rendering just a static site is not that useful we need a website to be dynamic this can be achived with template rendering as well.
 For this lets take an another example.
 
 
 MORE STUFF COMMING SOONN :)!!!
