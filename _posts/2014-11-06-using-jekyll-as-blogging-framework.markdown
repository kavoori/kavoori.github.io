---
layout: post
title:  "Using Jekyll for Blogging"
date:   2014-11-06 21:50:00
comments : true
categories: Blog
---
After using [Hexo](https://github.com/hexojs/hexo) for a while to generate static content for my blog, I discovered [Jekyll](http://jekyllrb.com/), which I like a lot more than Hexo. It is much easier to use, has better documentation and it is actually the blogging platform that powers [GitHub Pages](https://pages.github.com/). This means it should be easy to deploy your site using Github for free! (even with a custom domain!)

Here is another 10 minute installation guide to get it up and running.

### Installation
The only requirements are :

* [Ruby](https://www.ruby-lang.org/en/)
* [RubyGems](http://rubygems.org/)
* [Node.js](http://nodejs.org/)


#### 1. Install Ruby using [Homebrew](http://brew.sh/) or the default package manager on your Linux OS.
(If you don't have Homebrew on your Mac OSX, you should)
{% highlight bash %}
brew install ruby
sudo apt-get install ruby
{% endhighlight %}

#### 2. Install RubyGems and upgrade to the latest version.
{% highlight bash %}
gem update --system   
{% endhighlight %}

#### 3. Install Node.js
The best way to install Node.js is installing with [nvm](https://github.com/creationix/nvm).

{% highlight bash %}
curl https://raw.githubusercontent.com/creationix/nvm/v0.17.2/install.sh | bash
{% endhighlight %}

The above script clones the `nvm` repository and also adds the source line to `~/.bashrc` (or `~/.bash_profile` or  `~/.profile`)

Run the following to get a list of all the Node.js versions available and choose the version to install.

{% highlight bash %}
nvm ls-remote
{% endhighlight %}

Install the version you have chosen by running the following:

{% highlight bash %}
nvm install 0.11.14
{% endhighlight %}

### Install Jekyll with RubyGems
The best way to install Jekyll is via RubyGems. Run the following on the terminal. This will add `jekyll` executable to your path.
{% highlight bash %}
gem install jekyll
{% endhighlight %}

Now you can start using Jekyll. See the [documentation](http://jekyllrb.com/docs/home/) if you run into any issues.

### Start using Jekyll
Create a new blog and start your blog on your `localhost`
{% highlight bash %}
jekyll new blog
cd blog
jekyll serve [--port PORT] [--watch]
{% endhighlight %}

This will start your blog on localhost on port `4000` (or at the `PORT` if you used the `--port` flag). Browse to `http://localhost:[PORT]` to see your new blog.The `--watch` flag will watch for changes and generate the content automatically without having to restart jekyll.

### Create a new post
Create a new post by creating a new file under `_posts` folder. The file name *should* follow the following format.
{% highlight bash %}
YEAR-MONTH-DAY-title.MARKUP
{% endhighlight %}
Here is an example
{% highlight bash %}
touch _posts/2014-11-06-my-first-post.MARKUP
{% endhighlight %}
Edit the above file and add your content using [markdown](http://daringfireball.net/projects/markdown/syntax) syntax.

### Build your site
The following will generate your site into the `_site` folder. Now you can FTP the contents of the `_site` folder your hosting provider. 
{% highlight bash %}
jekyll build
{% endhighlight %}

Here is the best feature of Jekyll.

### Host your blog for free at [GitHub Pages](https://pages.github.com/).

All you have to do is create a repository in your GitHub account with the name `githubusername.github.io` and push the entire contents of your `blog` (not just the `blog/_site` folder) to the repo. Give it a few minutes and now browse to `http://githubusername.github.io`. Your blog should be hosted there for free!

Here are the detailed steps.

1. Create a new github repo as : `username.github.io`
2. Initialize a new git repo under the Jekyll `blog` folder by running:
 `git init`
3. Add the remote origin:
`git remote add origin git@github.com:[username]/[username].github.io.git`
4. Add and commit:
`git add --all &&` 
`git commit -m"Initial commit`
5. Push to the origin/master:
`git push -u origin master`
