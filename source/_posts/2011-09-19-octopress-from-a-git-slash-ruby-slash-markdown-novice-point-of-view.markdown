---
layout: post
title: "Octopress From a Git/ruby/markdown Novice Point of View"
date: 2011-09-19 23:10
comments: true
categories: [Github, octopress, ruby, markdown]
---

-This is a work in progress, on site for testing purposes only
{% pullquote %}
I've recently updated my companies website and had added a blog section as an afterthought using Wordpress. Github was something I used 
regularly to view open source projects but had never used as originally intended. (I just download the zip file). {"I'd always viewed the 
git process as having too steep a learning curve."}
Since I found out about Octopress last week, this has replaced my mediocre blog with something very elegant and also put me on the way to 
understanding and including git into my daily development workflow.
{% endpullquote %}

Create Github repository on github. I created a seperate blog repo for mine named 'blog'

Create a local copy of the octopress master: (I use terminal and create a directory on my desktop)
(the below git command will fail if you dont have [git installed](http://git-scm.com/))

mkdir blog 'or name of your github repo
cd blog
git init 'initialise this directory to be under git control
git remote add octopress git://github.com/imathis/octopress.git 'add octopress main branch to your repo 
git pull octopress master 
git remote add git@github.com:username/blog.git 'where blog is the name of your github repo
git push origin master


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
