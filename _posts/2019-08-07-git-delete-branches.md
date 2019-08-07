---
layout: post
title:  "Delete GIT local and remote branches from command line"
categories: 
    - GIT 
---

I'm always struggeling deleting remote GIT branches from the command line (in my case {% highlight powershell %}PowerShell{% endhighlight %}).
Deleting the local branch is a no-brainer:

{% highlight powershell %}
{% raw %}
    git branch -d branchName
{% endraw %}
{% endhighlight %}

If you've to force deletion (which GIT will tell you via {% highlight powershell %}git status{% endhighlight %}) you can use -D parameter.

Deleting the corresponding remote branch is done via a {% highlight powershell %}git push{% endhighlight %} command. In detail:

{% highlight powershell %}git push --delete origin branchName{% endhighlight %}

Hope that helps.
