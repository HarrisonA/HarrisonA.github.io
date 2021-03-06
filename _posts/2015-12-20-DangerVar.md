---
layout: post
title: Danger Variables.  Don't Blow Your Stack!
date: 2015-12-20 00:00:00

---

Whenever you are writing a recursive function, you run the risk of the recursion never ending and causing a stack overflow. One way to prevent this is to create a "danger" variable.  Essentially, the danger variable is a way to provide an exit from a function that is running in infinite recursion.
<br><br>

How it works:
<br><br>1) Create a global danger variable and set it equal to zero.
<br><br>2) Every time you enter the recursive function, increase the count of that variable and check if that count is greater than the amount recursive calls you expect.
<br>
<br>
3) If the danger count ever exceeds that number, exit the recursive function.
<br>
<br>A simple example:
{% highlight javascript %}

var someFunction = function (){
  var danger = 0;
  
  var recursiveFunc = function (){
    danger++;
    if (danger > 200) return;

    // write the body of the recursive function here
    
  } 

}
{% endhighlight %}

Note: This technique can also be applied to "while" loops and other forms of code that run the risk of infinitly looping.