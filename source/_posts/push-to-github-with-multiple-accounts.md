---
title: Work with GitHub and Multiple Accounts on one computer
date: 2018-02-08 22:45:43
tags: git
categories: Others
comments:
toc: true
thumbnail:
banner:
---

On my company laptop, I want to be able to use my personal github account as well as the company one, and to push commits to repos using the proper account. How could I do that?
<!-- more -->

Thanks Eric for the [tutorial](https://code.tutsplus.com/tutorials/quick-tip-how-to-work-with-github-and-multiple-accounts--net-22574)

## Scenario:
I have 2 github accounts, one for my own and one for the company. 
On the company laptop, I have copied my own github ssh keys. But the new company github account has no ssh key yet.

Let's denote the 2 accounts as follows:

```java
puppy@gmail.com // personal
puppy@company.com // company 
```
### Step 1 - Create a New SSH Key for company account

```bash
ssh-keygen -t rsa -C "puppy@company.com"
```
Be careful that you don't over-write your existing key for your personal account. Instead, when prompted, save the file as `id_rsa_COMPANY`. In my case, I've saved the file to `~/.ssh/id_rsa_COMPANY`.

### Step 2 - Attach the New Key
Next, login to your second GitHub account, browse to "Account Overview," and attach the new key, within the "SSH Public Keys" section. To retrieve the value of the key that you just created, return to the Terminal, and type: `cat ~/.ssh/id_rsa_COMPANY.pub`. Copy the entire string that is displayed, and paste this into the GitHub textarea. Feel free to give it any title you wish

Next, because we saved our key with a unique name, we need to tell SSH about it. Within the Terminal, type: `ssh-add ~/.ssh/id_rsa_COMPANY`. If successful, you'll see a response of "Identity Added."


### Step 3 - Create a Config File
Now, it's time to specify when we wish to push to our personal account, and when we should instead push to our company account. To do so, let's create a config file.

open `~/.ssh/config`, if it doesn't exist, create it by `touch ~/.ssh/config`

Now, add the following to the config file:

```bash
#Default GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
  
Host github-COMPANY
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_COMPANY
```

### Step 4 - Try it Out
To push the repo to your own github account:

```bash
git remote add origin git@github:yingchi/fastai-notes.git
```

To push the repo to company github account:

```bash
git remote add origin git@github-COMPANY:COMPANY/testing.git
```

Clone from company private repo:

```bash
git clone git@github-COMPANY:COMPANY/lalala.git
```

### Setting your github account for a single repository
```bash
$ git config user.email "email@example.com"
```
Confirm that you have set the email address correctly in Git:

```bash
$ git config user.email
email@example.com
```







