---
layout: post
title: How To Migrate From Blogger
---

There is a very clear step from [here](http://import.jekyllrb.com/docs/blogger/)

###  Install jekyll-import

```
sudo gem install jekyll-import
```

### Export Blogger content

- login to blogger.com

- click Settings -> Other -> Export Blog -> Download Blog

### Import using jekyll-import

Go to the directory where your site clone is stored, run the following:

{% highlight ruby %}
ruby -rubygems -e 'require "jekyll-import";
JekyllImport::Importers::Blogger.run({
        "source"                => "/Users/swang/Downloads/blog-12-31-2014.xml",
        "no-blogger-info"       => false,
        "replace-internal-link" => false,
        })'
{% endhighlight %}

Note the "source" bit has to be absolute path

That's it!

