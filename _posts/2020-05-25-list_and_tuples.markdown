---
layout: post
title:  "list and tuple"
date:   2020-05-25 14:56:26 +0100
permalink: chapter1
author: "Sachin Soman"
comments: true
---
Python `tuple` and `list` are really interesting, if we don't keep a keen eye 
on the documentation, we may miss several important fine prints.
I will in this page not include the common stuff only those things I see as stuff that I may forget will be presented.

First lets initialize a `list`
{% highlight ruby %}
x = [1,2,3,4]

{% endhighlight %}

- list is mutable
- it is dynamic
- it is ordered
- have any type of objects as elements eg an int element and some user defined object in same list

<hr>
we can retrieve the last element in a list without knowing its length by negative index slicing
{% highlight ruby %}
last_element_x = x[-1]
Output:4
{% endhighlight %}

Lets have an another negative index slicing
{% highlight ruby %}
x = [1,2,3,4,5]
last_element_x = x[-5:-1]
Output:[1, 2, 3, 4]
{% endhighlight %}

We can slice in steps
{% highlight ruby %}
x = [1,2,3,4,5]
slice_in_steps_x = x[len(x):0:-2]
Output:[6, 4, 2]
{% endhighlight %}
So we can do same for reversing the string
{% highlight ruby %}
x = [1,2,3,4,5]
reversed_x = x[::-1]
Output:[5, 4, 3, 2, 1]
{% endhighlight %}

Other things in list include `mutability` , `nested lists` and all the `functions` associated with list object which I can use docs to access and 
nothing new there
<hr>
<br>
<h1>Tuples</h1>
Tuples are really interesting and I have some important observations which I would like to share including how `kwargs` in functions work.

Tuple has a lot of similarity with a list as in data can be accessed, sliced and of course, like data in list it is ordered; The major difference between list and tuple
is that tuple is immutable and is much faster than a list. The access times will only matter in large data and in most trivial situations the access time shouldn't matter much.


Tuple defined by use of parenthesis
{% highlight ruby %}
x = (1,2,3,)
type(x)
Output:tuple
{% endhighlight %}

However look at this example
{% highlight ruby %}
x = (1)
type(x)
Output:int
{% endhighlight %}
Here we expected to get type tuple but python interprets it as a simple int. So what happens here is that remember we use `()` in other stuff like
in `if` condition etc. So python thinks that the `()` used here is for something like `if(1)` and thinks that `x = (1)` is just an `int`.Instead, if
we define x as shown below.

{% highlight ruby %}
x = (1,)
type(x)
Output:tuple
{% endhighlight %}

Here we added a comma after which the interpreter has identified the variable to be of type tuple. One thing to note is that this quirk occurs only when we
define <strong>a single items</strong>

<h3> Tuple Unpacking </h3>
When we define a tuple with multiple items its as if the object is packed and we can subsequently assign different items in the tuple to different
variables this is an important concept to remember as we dive into some interesting stuff now.
![image]({{site.github.url}}/assets/images/tuple_packing_1.png)*Ignore my crude drawings*

Lets try to to unpack the tuple `x = [1,2,3]` to variables a, b, c:
{% highlight ruby %}
x = (1,2,3)
type(x)
Output:tuple
a,b,c = x
print(a)
Output:1
print(b)
Output:2
print(c)
Output:3
{% endhighlight %}
![image]({{site.github.url}}/assets/images/tuple_packing_2.png)*Unpacking in action*

So here we can see that each item in tuple gets assigned to each variable. 
An interesting thing to note is that 
{% highlight ruby %}
x = (1,2,3)
a,b,c = x
is same as 
(a,b,c) = x
{% endhighlight %}

In such situations python doesn't care for the `()` parentheses. This is an important observation when we discuss 
stuff later in the section. 
With this new found observation lets look at one more example
{% highlight ruby %}
x = 1,2,3,4
type(x)
Output:tuple
{% endhighlight %}

So here we assigned a tuple without the need for a `parantheses`. With that we might be able recognize the the famous example
everyone gives to show how easy python is. `swapping elements without a temporary variable`

{% highlight ruby %}
x1 , x2 = 1 , 2
x1 , x2 = x2 , x1
print(x1)
Output: 2
print(x2)
Output: 1
{% endhighlight %}
This is because we used tuples without parentheses and tuple unpacking. I know `MIND == BLOWN!!`
<hr>


<h3>*args</h3>
Now I need to show one more thing  before I conclude this blog. Infact it all come down to this<br>
*insert drumrolls...*

Remember that in python there are functions where we can enter as many paramenters as we want like `max(1,2,3,4,5,6,7)` and get a result.
How does the function know how many arguments were passed?
Before we go into details here some examples we need to see
{% highlight ruby %}
x = 1,2,3,4,5
type(x)
Output:tuple
a,b,c = x
Output: ValueError: too many values to unpack
{% endhighlight %}
Here we see that when I try to unpack tuple x to variables a,b,c since I only have 3 variables and for unpacking I have 5 values we get a `ValueError`
in such cases we can do the following
{% highlight ruby %}
x = 1,2,3,4,5
type(x)
Output:tuple
a,b,*c = x
print(a)
Output: 1
print(b)
Output: 2
print(c)
Output: [3,4,5]
{% endhighlight %}

Here we see that when I used `*` on variable c the tuple elements gets  added to c as a list and I can now iterate through it. Pretty neat!.
This is known as unpacking of tuple.
lets look at some more examples

{% highlight ruby %}
x = 1,2,3,4,5
type(x)
Output:tuple
a,*b,c = x
print(a)
Output: 1
print(b)
Output: [2,3,4]
print(c)
Output: 5
{% endhighlight %}

This is self explanatory the interpreter does its best to assign values by dividing them.

{% highlight ruby %}
x = 1,2,3,4,5
type(x)
Output:tuple
*a,*b,c = x
Output: SyntaxError: two starred expressions in assignment
{% endhighlight %}
Here the interpretor doesn't know how to split data so it throws a `SyntaxError`

Now lets look at an example code
{% highlight ruby %}
def test_function(*args):
    for x in args:
        print(x," ")
        
test_function(1,2,3,4,5)
Output: 1 2 3 4 5
{% endhighlight %}

So what happened here. Like our previous examples, the arguments 1,2,3,4,5 get passed to *args which will be an unpacked tuple
and therefore we can iterate through the args like we would on a normal tuple or a list.
With this information, we can now make methods which can have a varying number of parameters. 

These are some of the interesting observations I had while studying about tuples and list. If I made some egregious mistakes do send me a mail
or put a pull request on my repo. Also do check out my other blogs as well links in About section :)

<h4>Reference</h4>
-  [RealPython](https://realpython.com)
-  [PythonDocs](https://docs.python.org/3/tutorial/datastructures.html)
-  UCD Python programing class     


