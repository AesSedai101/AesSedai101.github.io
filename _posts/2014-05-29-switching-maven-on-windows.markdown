---
layout: post
title:  "Switching Maven versions easily on Windows "
date:   2014-05-29 11:48:00
categories: maven windows symlinks
---
What do you do if plugins on different branches of your Git repo require different versions of Maven? First you spend half an hour figuring out that it is the Maven version that is wrong. After that, you attempt to switch Maven versions manually and completely mess up your Maven installations. Finally you spend an hour fixing Maven and searching for an easier solution.

After some research, I decided to use Windows's symbolic links. How-to geek has a good <a href="http://www.howtogeek.com/howto/windows-vista/using-symlinks-in-windows-vista/">article</a> on symlinks and how to set them up. What we are going to use is a soft link to a directory. 

For this to work you'll need a directory structure similar to this:

	maveninstalls
	|- apache-maven-3.0.5
	|- apache-maven-3.1.1
	|- maven (the symlink)

To point to the correct version of maven, you'll create a soft link to a directory, calling it 'maven'. For example, to set your Maven version to 3.0.5, point the symlink to the Maven 3.0.5 directory:

	mklink /D maven apache-maven-3.0.5

For ease of use, I created a script that will delete the old link and create a new one based on the version code given as parameter to the script
<script src="https://gist.github.com/AesSedai101/4817ef02ddd9d3e52990.js"></script>

Finally, point all your environment variables to the directory <pre>&lt;full path&gt;/maveninstalls/maven</pre>. If you want to change the Maven version, you can now change only the symbolic link, instead of having to figure out every instance where the old version is referenced.