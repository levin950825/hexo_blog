---
title: Automatic Deployment with Git to VPS 
date: 2017-03-20 17:51:36
tags: website dev
categories: Website Dev
comments:
toc: true
thumbnail:
banner:
---
How to set up automatic deployment with git to VPS for my html homepage-
<!-- more -->

## Server Setup
My workspace:

server live directory: `/var/www/peiyingchi.com/html`

server repository: `/var/repo/site.git`

## Creating Our Repository
Login to your VPS from command line and type the following:

```bash
cd /var
mkdir repo && cd repo
mkdir site.git && cd site.git
git init --bare
```
> `--bare` means that our folder will have no source files, just the version control.


## Hooks
Git repositories have a folder called 'hooks'. This folder contains some sample files for possible actions that you can hook and perform custom actions set by you.

[Git documentation](http://git-scm.com/book/en/Customizing-Git-Git-Hooks) define three possible server hooks: *'pre-receive'*, *'post-receive'* and *'update'*. *'Pre-receive'* is executed as soon as the server receives a *'push'*, *'update' *is similar but it executes once for each branch, and *'post-receive'* is executed when a *'push'* is completely finished and it's the one we are interested in.

So let's go to `hooks` folder:

```bash
cd hooks
```
Now, create the file `post-receive` by typing:

```bash
cat > post-receive
```
When you execute this command, you will have a blank line indicating that everything you type will be saved to this file. So let's type:

```sh
#!/bin/sh
git --work-tree=/var/www/peiyingchi.com/html --git-dir=/var/repo/site.git checkout -f
```
When you finish typing, press `control-d` to save. In order to execute the file, we need to set the proper permissions using:

```bash
chmod +x post-receive
```
You can see on the documentation that `git-dir` is the path to the repository. With `work-tree`, you can define a different path to where your files will actually be transferred to.

The `post-receive` file will be looked into every time a push is completed and it's saying that your files need to be in `/var/www/peiyingchi.com/html`

## Local Machine
The following will need to be done on our local machine (not server).

Here, I have already created a project git repo. 
Then we need to configure the remote path of our repository. Tell Git to add a remote called `live`:

```bash
cd /my/workspace
git remote add live ssh://user@mydomain.com/var/repo/site.git
```
Then, to push your work to your server, use the following command

```bash
git push live master
```
For me, I also have a GitHub repo for my files, it still works the same. This means that if I do `git push`, the local repo will be pushed to GitHub by default. The 2 remote repos (one at my cloud server, on at GitHub) do not have any connections. 

