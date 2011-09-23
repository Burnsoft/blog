---
layout: post
title: "Octopress Install With Screencast"
date: 2011-09-23 07:04
comments: true
categories: 
---

I've recently updated my companies website and had added a blog section as an afterthought using Wordpress. Github is something I use
regularly to view open source projects but had never used as originally intended, (I just download the zip file), as I'd always viewed the
git process as having too steep a learning curve.
Since I found out about Octopress last week, this has replaced my mediocre blog with something very elegant and also put me on the way to
understanding and including git into my daily development workflow.
 
The process was relatively simple, but being new to Git and Ruby I had a few problems along the way. I hope this post helps you if you also
choose to host your octopress on github.
 
(Update - 23rd September 2011 - The Octopress team have made this even easier since I did my install last week.
There is a custom rake script (is that the correct terminology?) to do the tricky git stuff for a github install.)

###Steps###

-Things to do before installing Octopress:

<!--more-->

1. [Install Git](http://git-scm.com/)
2. [Install RVM and latest Ruby](https://gist.github.com/1159539/)
3. Create your repo on Github
4. Set up local directory to hold your repo

###Install to Github subdirectory

1. git remote add octopress git://github.com/imathis/octopress.git
2. git pull octopress master
3. git checkout -b source 
4. git remote add origin git@github.com:YourUserName/YourRepoName.git
5. git push origin source
6. Setup RVM 
7. rvm rvmrc trust
8. rvm reload
9. rake setup_github_pages
10. rake install
11. Amend you root line in your config.yml file to your new repo name 
12. Rake Generate
13. Rake Deploy

###Screencast

The sound didn't transfer to youtube for some reason. If I get enough asks then i'll try again. Try me on Twitter

<iframe width="420" height="315" src="http://www.youtube.com/embed/57VoGcJMBWs" frameborder="0" allowfullscreen></iframe>

## Thing I wish I'd known earlier

When you do the commits from the command line Git will prompt for username and password. This gets annoying, so I found this work around on
stackoverflow. Create a .netrc file in your user directory (cd~/) that contains:
 
{% codeblock %}
machine github.com 
login yourgithublogin
password yourgithubpassword
{% endcodeblock %}
 
No more password prompts.
 
Avoid using Finder to navigate your local repo structure. It has a nasty habit of creating .DS_Store hidden files which .gitignore didn't seem to ignore

```find . -name *.DS_Store -type f -exec rm {} \;``` will get rid of them.
you can also just ```rm ".DS_Store"``` in the relevant directory

 

