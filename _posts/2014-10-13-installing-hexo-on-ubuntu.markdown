---
layout: post
title:  "Installing Hexo on Ubuntu"
date:   2014-10-13 10:41:58
comments : true
categories: Blog
---
I have been searching for a simple and lightweight self hosted blogging platform that is suitable for programmers and I might have found the one! [Hexo](https://github.com/hexojs/hexo).

Here is a 10 minute installation to get it up and running. 

The only requirements are :

* [Node.js](http://nodejs.org/)
* [Git](http://git-scm.com/)


### 1. Install Node.js
The best way to install Node.js is installing with [nvm](https://github.com/creationix/nvm).

{% highlight bash %}
curl https://raw.githubusercontent.com/creationix/nvm/v0.17.2/install.sh | bash
{% endhighlight %}

The above script clones the `nvm` repository and also adds the source line to `~.bashrc` (or `~/.bash_profile` or  `~/.profile`)

Run the following to get a list of all the Node.js versions available and choose the version to install

{% highlight bash %}
nvm ls-remote
{% endhighlight %}

Install the version you have chosen by running the following:

{% highlight bash %}
nvm install 0.11.14
{% endhighlight %}

### 2. Install Git 
Ubuntu should have `git` already installed, but if it is not, run:

{% highlight bash %}
sudo apt-get install git-core
{% endhighlight %}

### 3. Install Hexo.
Use `npm` to install the `hexo` package globally (using the `-g` flag)

{% highlight bash %}
npm install -g hexo
{% endhighlight %}

### 4. Create a blog
Initialize a blog folder and install all dependencies.

{% highlight bash %}
hexo init blog
cd blog
npm install
{% endhighlight %}

### 5. Configure
Modify the options in `_config.yml` file which should be in the folder created by `hex init` command. The following configuration options are of interest.

{% highlight bash %}
title: [Blog Title]
subtitle: [Sub Title]
description:
author: [Name]
email:
language: [IETF format]

port: [port number]
server_ip: [ip address to bind]
logger: true
logger_format: default
{% endhighlight %}

### 6. Start Hexo server
Start the hexo server by running the following:

{% highlight bash %}
hexo server
{% endhighlight %}

This will start the server at the IP address and port specified in `_config.yml` file.

### 7. Installing a theme
The themes are installed under `themes` directory which is under the root directory (which is `blog` in this case)
To install a theme like `light` do the following:

{% highlight bash %}
git clone https://github.com/hexojs/hexo-theme-light.git themes/light
cd themes/light
git pull
{% endhighlight %}

Edit the `_config.yml` to include the name of the theme to `light` as follows:

{% highlight bash %}
theme: light
{% endhighlight %}

### 8.Use forever to start and stop the server
Install `forever` globally.

{% highlight bash %}
npm install -g forever
{% endhighlight %}

Install hexo in the `blog` folder.

{% highlight bash %}
npm install hexo --save
{% endhighlight %}

Create a `app.js` file under `blog` directory as follows:

{% highlight js %}
require('hexo').init({command: 'server'});
{% endhighlight %}

Now you can use `forever` to start and stop the server as follows

{% highlight bash %}
forever start app.js
forever stop app.js
forever restart app.js
{% endhighlight %}

### 9.Deploy to Github pages by editing the configuration file

{% highlight bash %}
deploy:
  type: github
  repo : git@github.com:[reponame] # usually username/username.github.io
  branch: master
{% endhighlight %}

Generate the site and deploy at the same time using:

{% highlight bash %}
hexo generate --deploy
{% endhighlight %}

For further documentation see the [Hexo Documentation](http://hexo.io/docs/).

_Pro tip - if you are monkeying about with different Hexo themes, make sure you delete the `db.json` file in the root folder before you generate/deploy. I’m not sure what that thing is, but some theme garbage can get jammed up in there and it won’t show up running hexo server but it will maddeningly appear when you `generate` the site._
