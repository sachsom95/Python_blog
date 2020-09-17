---
layout: post
title: "__str__()  and  __repr__()"
date: 2020-09-08 14:56:26 +0100
permalink: chapter4
author: "Sachin Soman"
comments: true
---

Most of the time beginners and seasoned veterans of python try to debug their code using the trusty old `print()` statement,especially, to peek into contents of an object. This is particularly true when we are working in an interactive environment like a jupyter notebook. An example of this may be to see the contents of a list after an append operation.  Let's take a very simple example where we try to see what happens when we append elements to a list and try to see it using `print()`

{% highlight ruby %}
lst = list()
    for x in range(3):
    lst.append(x)
    print(lst)
>>[0]
>>[0, 1]
>>[0, 1, 2]
{% endhighlight %}

We see that we get a user understandable output for the `list` object when we print the list object. Now let's try to do something similar again. But this time we will try to print a user-defined object. We start by defining a simple class 'Bike'

{% highlight python %}
class Bike:
    def __init__(self,brand,size):
        self.brand = brand
        self.size = size

object_bike = Bike("Fixie","Medium")
print(object_bike)

>> <__main__.Bike object at 0x7fda39054e50>
{% endhighlight %}

Here when we defined our own object and tried a print statement. We get a rather esoteric output `<__main__.Bike object at 0x7fda39054e50>`. Now, this output is not useful to end-user or a developer.

This is where `__str__` and `__repr__` comes to the play. These are special functions that can be overridden to make our objects more understandable. Developers who use java have used something similar called `toString()` to achieve the same.

If we go to python documentation we get a cryptic explanation as shown below
![image]({{site.github.url}}/assets/images/chapter4_str_docs.png)_The python docs explaination_

`__str__()` and `__repr__()` allows us to make a string representation of the objects. Now the more confusing aspect is why have two different methods to achieve the same thing? well both the functions are aimed at providing string interpretation of the objects. But for different audiences. The `__str__` function is aimed at users who want an informal definition of the object which is understandable. While `__repr__` is used to give a more formal definition of the object. This will make much more sense when we implement them in our previous 'Bike' class.

{% highlight python %}
class Bike:
    def __init__(self,brand,size):
        self.brand = brand
        self.size = size
    
    def __str__(self):
        return f" ----Bike object----\n     brand:{self.brand} \n     size:{self.size} "

object_bike = Bike("Fixie","Medium")
print(object_bike)

>>  ---- Bike object ----
        brand: Fixie 
        size: Medium 
{% endhighlight %}


Here now when we use print() on the object instance we get a more useful output that explains what the object is about. Now let's look at `__repr__` implementation for this object. 


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
Note: we will discuss why we used "!r" in the f-string in the "details on implementation" section

Now here we use `repr(object)` and we get a different output. This is aimed towards a developer and this provides much formal definition like how the object needs to be initialized. In fact we can use the output of repr to recreate the object. Also notice that repr outputs will have quotes indicating that its a string. We will learn why repr gives a string output in the following example.
<!-- Let's look at the example below to see it in action. -->

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
# notice that the output is encased as a string
>> "Bike('Fixie','Medium')"

new_object = eval(repr(object_bike))
new_object.brand
>> "Fixie"
new_object.size
>> "Medium"

{% endhighlight %}

Here when we used `repr` a developer can understand how the object was created and the developer can use the `eval()` function to recreate the object. One thing to note is that if we use `__str__` to create the same output it will not create the string equivalent hence cannot be used with `eval`.

{% highligh python %}

class Bike:
    def __init__(self,brand,size):
        self.brand = brand
        self.size = size
    
    def __str__(self):
        return f"Bike({self.brand},{self.size})"

    def __repr__(self):
        return f"Bike({self.brand!r},{self.size!r})"

y = Bike("Fixie","Medium")
str(y)
# notice its not a string
>> Bike(Fixie,Medium)
repr(y)
# a string representation 
>> "Bike('Fixie','Medium')"

{% endhighligh %}

Now if we look closely with `repr` we used `!r` this is used with f-strings in python and it will wrap the output in a `''` indicating its a string for example if we didn't use `!r` in above example our output will be 

{% highligh python %}
repr(y)
>> "Bike(Fixie,Medium)"
{% endhighlight %}
instead we want

{% highligh python %}
repr(y)
>> "Bike('Fixie','Medium')"
{% endhighlight %}


> One thing to add is that __repr__() need not be always a strict definition of a function. We can have whatever output we want. But its a general convention to have `__repr__ `to have a formal object definition while `__str__`to be the understandable output to an object.



<h3> Details on implimentation </h3>

- When using a print() on an object. The object will first look for its `__str__` implementation and if that is not there it will fallback to `__repr__` output.
- If we look more closely at the output of `__str__` and `__repr__` we notice that the output from `__repr__` will be in string format which is enclosed in "". This allows us to use the output from `__repr__` on functions like `eval()` which requires a string as an argument. This is why when we returned the f-string for `__repr__` we used `!r` this will automatically add escape sequence characters output to make a proper string representation of the object.
- It is always advisable to implement a `__repr__` on a class
- Its not possible to give a formal definition for all the classes we make and it is perfectly acceptable as well.

These are some of the interesting observations I had while studying about `__str__` and `__repr__`. If I made some egregious mistakes do send me a mail
or put a pull request on my repo. Also, do check out my other blogs as well links in the About section :)

<h4>Reference</h4>
-  [PythonDocs](https://docs.python.org/3/library/stdtypes.html)
-  [stack overflow](https://stackoverflow.com/questions/7784148/understanding-repr-function-in-python)
-  [Dan Barders Amazing blog](https://dbader.org/blog/python-repr-vs-str)
-  UCD Python programing class
