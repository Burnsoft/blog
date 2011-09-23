---
layout: post
title: "Octopress from a 'Git/Ruby/Markdown' novice point of view"
date: 2011-09-19 23:10
comments: true
categories: [Github, octopress, ruby, markdown]
published: false
---
 
I've recently updated my companies website and had added a blog section as an afterthought using Wordpress. Github is something I use
regularly to view open source projects but had never used as originally intended, (I just download the zip file), as I'd always viewed the
git process as having too steep a learning curve.
Since I found out about Octopress last week, this has replaced my mediocre blog with something very elegant and also put me on the way to
understanding and including git into my daily development workflow.
 
The process was relatively simple, but being new to Git and Ruby I had a few problems along the way. I hope this post helps you if you also
choose to host your octopress on github.
 
##Steps##
 
1. Create Github Repository.
2. Install RVM and Ruby on your local machine. I'm using a Mac, linux distros will have a similiar process. Not sure about windows.
3. Create a directory the same name as your repo on your local machine.
4. Initialise the directory for use with Git
5. Add Octopress repo to your master/gh-pages branch
 
 
 
1. Create Github repository on github. I created a seperate blog repo for mine named 'blog'
 
Create a local copy of the octopress master: (I use terminal and create a directory on my desktop)
(the below git command will fail if you dont have [git installed](http://git-scm.com/))
<!--more-->
 
{% codeblock %}
mkdir blog 'or name of your github repo
cd blog
git init 'initialise this directory to be under git control
git remote add octopress git://github.com/imathis/octopress.git 'add octopress main branch to your repo
git pull octopress master
git remote add git@github.com:username/blog.git 'where blog is the name of your github repo
git push origin master
{% endcodeblock %}
 
 
Before running the ruby commands ensure you have the latest version of rvm and ruby 1.9.2 installed. I found [this gist](https://gist.github.com/1159539) invaluable
 
{% codeblock %}
rvm rvmrc trust
rvm reload
gem install bundler
gem install rake
bundle install
{% endcodeblock %}
 
install the default Octopress theme and push it on Source branch :
 
{% codeblock %}
rake install
git add .
git commit -m 'Installed Octopress Theme'
git push origin source
{% endcodeblock %}
 
Edit _config.yml and change default value into yours. After finishing, then we deploy.
 
{% codeblock %}
git clone git@github.com:username/username.github.com _deploy
rake config_deploy[gh-pages]
rake generate
rake deploy
{% endcodeblock %}
 
 
When you ```Rake Generate```
the re-generated site is created in the 'public/blog/' directory.
 
You can either load the index.html page directly from this page and manually generate when you make changes or use the
```rake preview``` command to run a local web server and have view live changes by pointing your browser to localhost:4000
 
 
 
When you do the commits from the command line Git will prompt for username and password. This gets annoying, so I found this work around on
stackoverflow. Create a .netrc file in your user directory (cd~/) that contains:
 
`machine github.com` 
`login yourgithublogin`   
`password yourgithubpassword`   
 
No more password prompts.
 
 
find . -name *.DS_Store -type f -exec rm {} \;
 
{% gist 1233473 %}
