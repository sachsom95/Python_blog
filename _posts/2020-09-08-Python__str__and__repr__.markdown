---
layout: post
title: "__str__()  and  __repr__()"
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

`__str__()` and `__repr__()` allows us to make a string representation of the objects. Now the more confusing aspect is why have two different methods to achive the same thing? well both the functions are aimed at providing string interpretaion of the objects. But for different audience. The `__str__` function is aimed at users who want an informal definition of the object which is understandable. While `__repr__` is used to give a more formal definition of the object. This will make much more sense when we impliment them to our previous 'Bike' class.

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


Here now when we use print() on the object instance we get a more useful output that explains what the object is about. Now lets look at `__repr__` implimentation for this object. 

{% highlight python %}
class Bike:
    def __init__(self,brand,size):
        self.brand = brand
        self.size = size
    
    def __str__(self):
        return f" ----Bike object----\n     brand:{self.brand} \n     size:{self.size} "

    def __repr__(self):
        return f"Bike({self.brand!r},{self.size!r})"

object_bike = Bike("Fixie","Medium")
repr(object_bike)

>> "Bike('Fixie','Medium')"
{% endhighlight %}
Note: we will discuss why we used "!r" in the f-string in the "details on implimentation" section

Now here we use `repr(object)` and we get a different output. This is aimed towards a developer and this provides much formal definition. Infact we can use the output of repr to recreate the object.
Let's look at the example below to see it in action.

{% highlight python %}

class Bike:
    def __init__(self,brand,size):
        self.brand = brand
        self.size = size
    
    def __str__(self):
        return f" ----Bike object----\n     brand:{self.brand} \n     size:{self.size} "

    def __repr__(self):
        return f"Bike({self.brand!r},{self.size!r})"

object_bike = Bike("Fixie","Medium")
repr(object_bike)

>> "Bike('Fixie','Medium')"

new_object = eval(repr(object_bike))
new_object.brand
>> "Fixie"
new_object.size
>> "Medium"

{% endhighlight %}

One thing to add is that __repr__() need not be always a strict definition of function. We can have what ever output we want. But its a general convention to have `__repr__ `to have a formal object definition while `__str__`to be the understandable output to an object.


<h3> Details on implimentation </h3>

- When using a print() on an object. The object will first look for its `__str__` implimentation and if that is not there it will fallback to `__repr__` output.
- If we look more cloesly at the output of `__str__` and `__repr__` we notice that the output from `__repr__` will be in string format which is enclosed in "". This allows us to use the output from `__repr__` on functions like `eval()` which requires a string as an argument. This is why when we returned the f-string for `__repr__` we used `!r` this will automatically add escape sequence charecters output to make a proper string representation of the object.
- Its always advisable to impliment a `__repr__` on a class

These are some of the interesting observations I had while studying about `__str__` and `__repr__`. If I made some egregious mistakes do send me a mail
or put a pull request on my repo. Also do check out my other blogs as well links in About section :)

<h4>Reference</h4>
-  [PythonDocs](https://docs.python.org/3/library/stdtypes.html)
-  [stack overflow](https://stackoverflow.com/questions/7784148/understanding-repr-function-in-python)
-  [Dan Barders Amazing blog](https://dbader.org/blog/python-repr-vs-str)
-  UCD Python programing class
