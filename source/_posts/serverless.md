title: serverless
date: 2021-01-09 13:52:07
tags:
---
## Creating a serverless blog system

downloaded Node.js
updated git


installed Hexo 


{% codeblock %}
sudo npm install -g hexo-cli 
{% endcodeblock %}



Navigate to the directory where you want your site to reside, then create a new empty project 

{% codeblock %}
hexo init blog.mastertheclouds.com
{% endcodeblock %}
 - this clones a hexo-starter project from git

#h2 Here are my hexo command notes:

{% codeblock %}
hexo new <filename>
{% endcodeblock %}
  -  makes a new post/file in the _posts folder

{% codeblock %}
hexo new draft <filename>
{% endcodeblock %}
  - makes a draft post in the _posts/drafts folder

you can use command to see the drafts temporarily
{% codeblock %}
hexo server --draft
{% endcodeblock %}


when you are ready to publish, use 
{% codeblock %}
hexo publish <filename>
{% endcodeblock %}
  - moves to a the _posts folder

hexo new page <filename>
  - creates a dir with filename, used for an "About  us" type page

I went over to github and created a public repository for blog.mastertheclouds.com.

I then signed into the AWS console, and created a matching S3 bucket with versioning enabled, and encryption enabled using Server-side encryption Amazon S3 master-key (SSE-S3). I also enabled static website hosting.

This bucket will be private, and can only be accessed by going through a Cloudfront distribution.

arn:aws:s3:::blog.mastertheclouds.com

note: need:
	index.html and error.html
	buildspec.yaml

Setting up Cloudfront
using these settings: 
* Redirect HTTP to HTTPS
* using a cache policy
* * since this is primarily a development tool, using Managed-CachingDisabled
* using a Custom SSL certificate, set for *.mastertheclouds.com
* security policy is set to TLSv1.2_2019

Setting up Route 53
 I went to my hosted zone, and added an A record to point to the distribution
d21o54m1ge68qs.cloudfront.net



Creating a CodeBuild project
blog-mastertheclouds-com setup to point at the github repository.

repo is https://github.com/sjohnsonpbh/blog.mastertheclouds.com.git
checked Webhook - rebuild every time a change is pushed

use a Managed Image, Operaing system Ubuntu 
using a buildspec.yaml file

the build badge is:
https://codebuild.us-east-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiNCt3M2E4akw0SEJReGNrSEZGVFl6UzZENFo5aGRZYjMwWDFnZGsvM2p4ZWZYaVo3aU1xSGxZa0t3RmRoL29JbnI1dEJEbGxKa2RUWlljbk01ZVVXZlAwPSIsIml2UGFyYW1ldGVyU3BlYyI6ImE1OHJuK1ZQQUkyMzkwbFQiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master

First build failed, no buildspec.yaml file. Rookie mistake...
Cloning the repo in VSCode

setting up Git
git config --global user.name "Scott Johnson"
git config --global user.email sjohnsonpbh@yahoo.com

manually added the buildspec.yml file and re-tried the Codebuild
failed again,
I now have 31 tabs open

run this:
npm install -g npm

setup a test for a word on index.html ?

after installing Amplify CLI and signing into AWS, I created a new IAM user amplify-user, and then in the blog.mastertheclouds.com directory, I ran 

amplify init
![upload successful](/images/pasted-0.png)

amplify add hosting
￼
![upload successful](/images/pasted-1.png)￼

amplify publish -c

￼