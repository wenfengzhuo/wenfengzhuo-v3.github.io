---
layout: post
comments: true
categories: web-development
title: What I learned from building this blog website
---
### Why choose Jekyll
The answer is [markdown](https://daringfireball.net/projects/markdown/). There are many articles about why markdown is so popular with developers, so I won't list its advatanges here. [Jekyll](https://jekyllrb.com/) is both a website generator and also providing a web server that serves the website. It beautifully converts those plain text (markdown or liquid) to human-readable html pages. This is a great feature for developers especially those who are struglling with HTML/CSS stuff.  




### Github Pages vs AWS
A blog must be served in a host. [Github Pages](https://pages.github.com/) provides host for repository that either is for github projects or your personal blogs. Actually, Github Pages is powered by Jekyll, so technically, any Jekyll project could work well in Github pages (see -> [how to set up a Jekyll project in Github Pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)). Unfortuantely, Github Pages does not support most of beautify [Jekyll Themes](http://jekyllthemes.org/). Such themes are contributed by thounsand of developers who did a great job at making a beautiful Jekyll pages, and most importantly, they are free to use. This is why I decided to set up [AWS](https://aws.amazon.com). There are many cloud options, so why AWS? First, it is used by countless people; Second, it is probably one of the earliest public cloud infrastrature, so it is pretty mature; Third, it provides starters with a apealling 12-month's free trial.

> Edit: It turns out I was wrong because we can directly upload whole theme into our git repository. What Github Pages does not support is that it does not allow you to define a theme in _config.xml file. Anyway, using AWS gave me a great opportunity to learn how to use AWS to build website.

### AWS Network Problem
When everything was setup([how?](https://jekyllrb.com/docs/installation/)), I began to navigate the public adress to see whether my blog is ready. Unfortunately, the address cannot be accessed as the Chrome indicated.
I was wondering if the public address assigned by AWS is not public yet. This drove me to ping the address in my terminal. It turned out I cannot ping the address either.  
I dived into the AWS console to see what happened, but the address was a public IP. Then I doubted that is this related to firewall. I denied myself. If it is related to firewall, then at least we could ping the address although we could not access a specific port.
Is it related to some configurations? Then I noticed that there is a Network Security Group there in the configuration page, and it turned out be the evil of the issue. With a default setting, we could only ssh to aws instance because only 22 port is open for access, all other inbound and outbound traffic are turned off. See more details -> [Security Group](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)

### Automatically website updating
Finally, I want to talk about website updating. Github Pages provides a easy way to display your blog content. What you need to do is committing a change to the repository and the change will be automatically applied. While my blog is served in AWS, so how to automatically update the web content when I want to commit a new post?
The first tool I thought of is [cron job](http://www.unixgeeks.org/security/newbie/unix/cron-1.html). Using a cron job to periodically fetch the latest data from github repository, and that's it. The cron job is a very powerful tool in a linux system while it is so easy to configure. Here is my configuration:

```
  */1 *  *  *  * <User Name> cd <Blog Path> && git pull >> /var/log/blogcronb.log 2>&1
```
