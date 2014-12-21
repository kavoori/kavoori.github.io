---
layout: post
title:  "Oh My Zsh. OMG!"
date:   2014-12-20 15:00:00
comments : true
categories: Blog
---
Oh my God. How did I not find out about [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) till now? Here is a 10 minute installation guide to pimp out your shell. You are already on [zsh](http://www.zsh.org/), right?

### Installation
Use `curl` to install automatically in one step. Run the following from the home folder. This clones the oh-my-zsh [git](https://github.com/robbyrussell/oh-my-zsh) repo to the default location `~/.oh-my-zsh`.
{% highlight bash %}
curl -L http://install.ohmyz.sh | sh
{% endhighlight %}

If this is the first time installing using oh-my-zsh, the install script creates a `~/.zshrc` file and copies over some information (path and aliases) to it. If it did not, copy an existing template to your home folder.
{% highlight bash %}
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
{% endhighlight %}

It is time to change your shell to `zsh` if you haven't done it already
{% highlight bash %}
chsh -s /bin/zsh
{% endhighlight %}

Open a new terminal window/tab and your prompt should look like this. Now change to a git directory and see the prompt change automatically. 
![Initial Prompt](/assets/oh-my-zsh-initial-prompt.jpg)

### Customization
The above prompt is made possible by the default theme "robbyrussell" that is loaded at the start of each session. This can be customized a lot to make it look any way you want. 

Choose `ZSH_THEME="random"` in your `~/.zshrc` (usually at the top of the file) and a different theme is loaded each time a new terminal session is loaded. You can look at all the existing themes this way till you find the one you like.

To customize your prompt and not use an existing theme, create a file name ending in `.zsh-theme` under `~/.oh-my-zsh/custom/themes` and it will be sourced each time a new session is started. 

For example create a file `~/.oh-my-zsh/custom/themes/[name-of-theme].zsh-theme` then add an entry `ZSH_THEME="[name-of-theme]"` to `~/.zshrc` and comment out the default theme. The following are the contents of my personal theme (95% of which is taken from an existing theme called 'ys')

{% highlight bash %}
function box_name {
    [ -f ~/.box-name ] && cat ~/.box-name || echo $HOST
}

# Directory info.
local current_dir='${PWD/#$HOME/~}'

# Git info.
local git_info='$(git_prompt_info)'
ZSH_THEME_GIT_PROMPT_PREFIX=" %{$fg[white]%}on%{$reset_color%} git:%{$fg[cyan]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%}"
ZSH_THEME_GIT_PROMPT_DIRTY=" %{$fg[red]%}x"
ZSH_THEME_GIT_PROMPT_CLEAN=" %{$fg[green]%}o"

# Prompt format: \n # USER at MACHINE in DIRECTORY on git:BRANCH STATE [TIME] \n $ 
PROMPT="
%{$terminfo[bold]$fg[blue]%}#%{$reset_color%} \
%{$fg[cyan]%}%n \
%{$fg[white]%}at \
%{$fg[green]%}$(box_name) \
%{$fg[white]%}in \
%{$terminfo[bold]$fg[yellow]%}${current_dir}%{$reset_color%}\
${git_info} \
%{$fg[white]%}
%{$terminfo[bold]$fg[red]%}$ %{$reset_color%}"

if [[ "$USER" == "root" ]]; then
PROMPT="
%{$terminfo[bold]$fg[blue]%}#%{$reset_color%} \
%{$bg[yellow]%}%{$fg[cyan]%}%n%{$reset_color%} \
%{$fg[white]%}at \
%{$fg[green]%}$(box_name) \
%{$fg[white]%}in \
%{$terminfo[bold]$fg[yellow]%}${current_dir}%{$reset_color%}\
${git_info} \
%{$fg[white]%}[%*]
%{$terminfo[bold]$fg[red]%}$ %{$reset_color%}"
fi
{% endhighlight %}

This makes my prompt look like this:
![My Prompt](/assets/my-prompt.jpg)

### Plugins
There are number of [plugins](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins) available. I have added the following to my `~/.zshrc`. My favorite ones are `sublime`, `sudo` and `web-search` (`git` comes with the default installation)
{% highlight bash %}
plugins=(git git-flow github git-extras ruby node npm sublime theme sbt scala mvn ssh-agent sudo web-search zsh-syntax-highlighting)
{% endhighlight %}

I prefer to have all my aliases and functions in separate files so that they can be sourced by both `bash` and `zsh` (in case I decide to fall back to `bash`).
For aliases, I just source all my aliases at the end of my `~/.zshrc`.

{% highlight bash %}
source $HOME/.aliases
{% endhighlight %}
For functions however, I had to add a new file to `~/.oh-my-zsh/lib` folder. The name did not really matter, but it has to have an extenstion of `.zsh`. All files with that extension are sourced automatically. Once this file is added, I added this entry to `~/.oh-my-zsh/.gitignore` to keep the repo clean.

### Upgrade
By default you will be prompted to check for upgrades. If you would like `oh-my-zsh` to automatically upgrade itself without prompting you, set the following in your ~/.zshrc:

`DISABLE_UPDATE_PROMPT=true`

To disable upgrades entirely, set the following in your `~/.zshrc`:

`DISABLE_AUTO_UPDATE=true`

To upgrade directly from the command-line, just run `upgrade_oh_my_zsh`.


### Uninstalling
Run `uninstall_oh_my_zsh`, (which is conveniently in your path) from the command-line and it’ll remove itself and revert you to `bash` (or your previous `zsh` configuration). You might have to run `chsh` to change your shell back to `bash`.

