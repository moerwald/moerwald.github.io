---
layout: post
title:  "The PowerShell split operator"
published : false
categories: 
    - PowerShell
---

When working with the `split` operator you normally use the simplest form of split, e.g.:

{% highlight powershell %}
{% raw %}
    >"1:2:3" -split ":"
    1
    2
    3
{% endraw %}
{% endhighlight %}

But split offers some additional features:

* Defining the number of array entries that shall be returned.
* Deactiving regex parsing of the split argument (which is the default behaviour)
* Use script block parameters to define more complex split predicates (helpful when a regex can't solve your split problem)

## Return a defined number of entries

Besides the regex pattern (used as predicate) you can also define how much entries shall be returned by split.

Example:

{% highlight powershell %}
{% raw %}
    >"1:2:3:4:5" -split ":",3
    1
    2
    3:4:5
{% endraw %}
{% endhighlight %}

We can see that three entries are returned, where third element contains the remaining string.

## Use script block parameters for complex operations

Let's say we want to split this list up in pairs:

{% highlight powershell %}
{% raw %}
    >"Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune"
{% endraw %}
{% endhighlight %}

Here we need a predicate that ensures

* the actual character is `,`, and
* that we perform the operation with every second `,`

For this requirement we can use a script block parameter:

{% highlight powershell %}
{% raw %}
    > $count = q(0)
    > "Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune" -split { $_ -eq "," -and ++$count[0] % 2 -eq 0 }                                                                       
    Mercury,Venus
    Earth,Mars
    Jupiter,Saturn
    Uranus,Neptune
{% endraw %}
{% endhighlight %}

We need the helper array `count` to check if the actuall `,` occurence is even or odd.

As can be seen split offers a log of options to perform complex operations.