---
layout: post
title:  "Convert GIT log entries to PowerShell objects"
date:   2019-08-04 
categories: GIT PowerShell
---



Sometimes you’re in the situation where you’ve to revert a specific commit via git revert sha1. Sometimes you only have a ticket number (in the best case …) and need to find out the corresponding GIT SHA1.

Option 1: Type git log and read the commit messages ...

Option 2: Convert the git log output to a PowerShell object.

How can option two be realized? Well GIT log supports pretty-formats, which you can use to sculpture your stdout output to make it easier to parse. Below you see the one-liner using the advantage of this formating feature:

{% highlight powershell %}
{% raw %}
    (git log --format="%ai`t%H`t%an`t%ae`t%s" -n 100) | ConvertFrom-Csv -Delimiter "`t" -Header ("Date","CommitId","Author","Email","Subject")
{% endraw %}
{% endhighlight %}

*What's going on?*

{% highlight powershell %}
{% raw %}
    git log --format="%ai`t%H`t%an`t%ae`t%s" -n 100
{% endraw %}
{% endhighlight %}

Above GIT command produces the following output:


{% highlight powershell %}
{% raw %}
    2019-07-31 11:07:01 +0200       fd006e74a2c21c9a8ae806ceaed1bfbe3816b305        user  user@domain.com  Second commit message
    2019-07-31 09:09:15 +0200       6469f803601d16bf8b59ff6e333ff64cc9f531e0        user  user@domain.com  First commit message
{% endraw %}
{% endhighlight %}

Date, commit ID, user name, user email and commit message are separated by tabs. Separating this way makes it easy to convert the output to CSV, by piping it to theConvertTo-CSV cmdlet. With the help of the -Delimiter and the -Header parameters, it is easy to baptize every entry with a given name.

The last thing to do is to store the pipeline output in an array, like:

{% highlight powershell %}
{% raw %}
$commits = (git log --format="%ai`t%H`t%an`t%ae`t%s" -n 100) | ConvertFrom-Csv -Delimiter "`t" -Header ("Date","CommitId","Author","Email","Subject")
{% endraw %}
{% endhighlight %}

Each entry in `$commits` will have a Date, CommitId, Author, Email and Subject property, which makes it easy to filter for specific commit messages via

{% highlight powershell %}
{% raw %}
    $commits| Where-Object { $_.Subject -like "*added*" }
{% endraw %}
{% endhighlight %}
