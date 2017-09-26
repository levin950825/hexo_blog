---
title: Speed Up Oh-My-Zsh
date: 2017-08-17 22:45:43
tags: shell
categories: Others
comments:
toc: true
thumbnail:
banner:
---

### Intro
I'm not a computer science expert, so I always choose to bear with the small issues happing on my laptop. My personal as well as working laptops are all Mac. And I use [iTerm2](https://www.iterm2.com/) and [Oh-My-Zsh](http://ohmyz.sh/) to replace the default Terminal. For the zsh theme, I recommend `mrtazz`.

One of the costs of using Oh-my-zsh is that the startup times are slow. That’s what everyone says.

But recently, I am getting more and more frustrated by the slow startup times. By searching online, I found [this tutorial](https://bennycwong.github.io/post/speeding-up-oh-my-zsh/) really helpful. 

The following are the steps I took to improve my oh-my-zsh experience:

### Step 1: Benchmarking Current Performance

Let's measure how long it takes to start a new shell session:

```bash
$ /usr/bin/time zsh -i -c exit
5.46 real         3.58 user         1.37 sys   
```

5.5 seconds??? That is not good at all.

### Step 2: Check What Slows It Down

Now, please pay attention. Run the following command in your Terminal, and take note of the portion where the output pauses a bit. 

```bash
$ zsh -xv
```

For my case, I found the portion for setting `hadoop & hive` parameters, and also for setting up `nvm` tend to get stuck for a while.

### Step 3: Remove the Unnecessary Parts

As I don't need `hadoop` and `nvm` that often, I decided to prevent them from auto-loading during starting up zsh.

So I made the following changes in my `~/.zshrc` file:

* Comment out the Hadoop Hive exports commands

```bash
#hadoop & hive
#export HADOOP_HOME=$(brew --prefix hadoop) # /usr/local/Cellar/hadoop/2.7.3/
#export HIVE_HOME=$(brew --prefix hive)/libexec
#export HCAT_HOME=$(brew --prefix hive)/libexec/hcatalog
#export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
#export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```

* Comment out the nvm loading command. Instead, create an alias for it so that I can easily load it when I need it.

```bash
#nvm
export NVM_DIR="$HOME/.nvm"
#[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
alias loadnvm='[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"'
```

### The End
That's all~ Now let's quite the Terminal and reopen it. 
Test the time again:

```bash
$ /usr/bin/time zsh -i -c exit
0.48 real         0.25 user         0.15 sys
```

Yeah.






