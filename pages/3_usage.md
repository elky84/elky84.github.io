---
layout: page
title: Usage
comments: true
permalink: /usage/
---

### Admin tools
* site [configuration file](https://github.com/LiXizhi/lixizhi.github.io/blob/master/_config.yml)
* Jekyll + [lixizhi.duoshuo.com](http://lixizhi.duoshuo.com/admin/)
* Jekyll + [lixizhi.disqus.com](http://lixizhi.disqus.com/admin/)
* Jekyll theme templates: [jekyllthemes.org](http://jekyllthemes.org)
   * Theme used: [cool-concise](http://jekyllthemes.org/themes/cool-concise-high-end/)
* Jekyll official site: [jekyllrb.com](http://jekyllrb.com)
* YAML for human readable markdown: [yaml.org](http://www.yaml.org/)
* markdown reference: [kramdown](http://kramdown.gettalong.org/quickref.html)

### About comments
Add a variable called `comments` to the [YAML front matter](http://jekyllrb.com/docs/frontmatter/) and set its value to true. A sample might look like:

    ---
    layout: post
    comments: true
    # other options
    ---

### Sample markdowns
Click view source at the bottom of the page

* Embedding code
{% highlight lua %}
local function main()
	print("hello world everyone")
end
{% endhighlight %}

