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