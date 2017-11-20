---
layout: post
title:  "Building a Jekyll Github Pages Blog"
date:   2017-11-17 16:00:00 -0800
categories: jekyll
---

# To install Jekyll for Github Pages on Mac OS:

### Update Ruby to 2.4.1 or the latest stable version:

List the known ruby packages
```
rvm list known
```

if rvm is not found: [https://rvm.io/rvm/install](https://rvm.io/rvm/install)


Choose a package and install
```
rvm install ruby-2.4.1
```

Install Jekyll
```
gem install jekyll
```

Install Bundle
```
gem install bundler
```

Create a new Jekyll Project
```
jekyll new --force comadan.github.io # will create a new directory in comadan.github.io. --force will overwrite contents of existing directory.
```

Install Bundle
```
cd comadan.github.io
bundle install
bundle exec jekyll serve
```

Go to http://localhost:4000 to view your local site.

Create a new repo comadan.github.io on github and then push your jekyll folder to that directory to serve it on Github Pages.

### Drafting Posts

Draft posts can live in the `_drafts` directory. These posts can be viewed with `jekyll serve --drafts`

### Including Images

create a folder e.g. `./assets/` to hold images and other assets.

Images can be shown with:
```
![My Image]({{ "/assets/screenshot.jpg" | absolute_url }})
```

More info on the right Markdown syntax for using images can be found in [jekyll's documentation](https://jekyllrb.com/docs/posts/) or in this [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

### including Latex

First you need to add the \_layouts directory. If we're using the default minima theme, you can get to this with 

```open $(bundle show minima)```

Copy the \_layouts directory to the root directory (e.g. comadan.github.io)

you need to add the following javascript reference to load the mathjax library to render the latex
```
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
```

Now latex can be inserted into markdown, e.g.:

```$$x_{1,2} = {-b\pm\sqrt{b^2 - 4ac} \over 2a}.$$```

will render as: $$x_{1,2} = {-b\pm\sqrt{b^2 - 4ac} \over 2a}.$$
