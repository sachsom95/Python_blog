---
layout: post
title: "__str__  and  __repr__"
date: 2020-09-08 14:56:26 +0100
permalink: chapter4
author: "Sachin Soman"
comments: true
---

Most of the time begineers and seasoned veterans of python try to debug their code ussing the trust old `print()` statement. Lets take a very simple example where we try to see what happens when we append elements to a list and try to see it using `print()`

{% highlight ruby %}
lst = list()
    for x in range(3):
    lst.append(x)
    print(lst)
>>[0]
>>[0, 1]
>>[0, 1, 2]
{% endhighlight %}

We see that we get a user understandable output for the `list` object. Now lets try to do something similar again. But this time we will try to print a user defined object. Lets define a simple class 'Bike'

{% highlight python %}
class Bike:
    def __init__(self,brand,size):
        self.brand = brand
        self.size = size

object_bike = Bike("Fixie","Medium")
print(object_bike)

>> <__main__.Bike object at 0x7fda39054e50>
{% endhighlight %}

Here when we defined our own object and tried a print statement we get a rather esotoric output `<__main__.Bike object at 0x7fda39054e50>`. Now this output is not useful to end user or a developer.

This is where `__str__` and `__repr__`  comes to the play. These are special functions which can be overriden to make our objects more understandable. Developers who use jave have used something similar called `toString()` to acheive the same.

If we go to python documentation we get a cryptic explaination as shown below
![image]({{site.github.url}}/assets/images/chapter4_str_docs.png)_The python docs explaination_

`__str__` and `__repr__` allows us to make a string representation of the objects. Now the more confusing aspect is why have two different methods to achive the same thing? well both the functions are aimed at providing string interpretaion of the objects. But for different audience. The `__str__` function is aimed at users who want an informal definition of the object which is understandable. While `__repr__` is used to give a more formal definition of the object. This will make much more sense when we impliment them to our previous 'Bike' class.

{% highlight python %}
class Bike:
    def __init__(self,brand,size):
        self.brand = brand
        self.size = size
    
    def __str__(self):
        return f" ----Bike object----\n     brand:{self.brand} \n     size:{self.size} "

object_bike = Bike("Fixie","Medium")
print(object_bike)

>>  ----Bike object----
        brand:Fixie 
        size:Medium 
{% endhighlight %}









These are some of the interesting observations I had while studying about `__str__` and `__repr__`. If I made some egregious mistakes do send me a mail
or put a pull request on my repo. Also do check out my other blogs as well links in About section :)

<h4>Reference</h4>
-  [RealPython](https://realpython.com)
-  [PythonDocs](https://docs.python.org/3/tutorial/datastructures.html)
-  UCD Python programing class
