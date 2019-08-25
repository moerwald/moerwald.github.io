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

Sometimes you're in the situation where you've to revert a specific commit via git revert sha1. Sometimes you only have a ticket number (in the best case ) and need to find out the corresponding GIT SHA1.

* Option 1: Type git log and read the commit messages ...

* Option 2: Convert the git log output to a PowerShell object.

## How can option two be realized?

Well GIT log supports pretty-formats, which you can use to sculpture your stdout output to make it easier to parse. Below you see the one-liner using the advantage of this formating feature:

{% highlight powershell %}
{% raw %}
    (git log --format="%ai`t%H`t%an`t%ae`t%s" -n 100) | ConvertFrom-Csv -Delimiter "`t" -Header ("Date","CommitId","Author","Email","Subject")
{% endraw %}
{% endhighlight %}

