---
layout: post
title: How To Migrate From Blogger
---

There is a very clear step from [here](http://import.jekyllrb.com/docs/blogger/)

* Install jekyll-import
```
sudo gem install jekyll-import
```

* Export Blogger content
- login to blogger.com
- click Settings -> Other -> Export Blog -> Download Blog

* Import using jekyll-import
go to the directory where your site clone is stored, run the following:
```ruby
ruby -rubygems -e 'require "jekyll-import";
JekyllImport::Importers::Blogger.run({
        "source"                => "/Users/swang/Downloads/blog-12-31-2014.xml",
        "no-blogger-info"       => false,
        "replace-internal-link" => false,
        })'
```
Note the "source" has to be absolute path

That's it!

#![_config.yml]({{ site.baseurl }}/images/config.png)

#The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
