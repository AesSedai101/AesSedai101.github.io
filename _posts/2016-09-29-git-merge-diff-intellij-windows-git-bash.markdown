---
layout: post
title:  "Using IntelliJ IDEA as merge/diff tool in Git bash on Windows"
date:   2016-09-29 12:00:00
categories: git intellij idea diff bash windows
location: github
---

IntelliJ IDEA has a really awesome merge tool, which works great if you use the merge tools built into the IDE. However, if you prefer the command line ([which you can use without leaving your IDE](http://riaancornelius.com/software-development/android-studio-set-custom-terminal/)), setting up the tools are a bit more complicated.

To begin with, if your IDE is installed in the default `Program Files (x86)`, you will need to create a [symlink](http://www.howtogeek.com/howto/windows-vista/using-symlinks-in-windows-vista/) to your IntelliJ IDEA directory. The path to this symlink *should not have any weird characters in it*.

	mlink /D C:\workspace\tools\idea "C:\Program Files (x86)\JetBrains\IntelliJ IDEA 2016.2"

The next step is to tell Git to use IDEA as your difftool, using the symlink. However, there is another issue: the IDEA startup script (`idea.bat`) is a Windows batch script, while the Git bash shell makes use of *nix bash scripts. Fortunately, it is possible to invoke `cmd.exe` [with arguments](http://ss64.com/nt/cmd.html). The specific command we are looking for is 

	cmd /C "<path to idea.bat> <any other arguments>"
	
At this point, we just need to figure out which arguments to pass to `idea.bat` and then add it to the `.gitconfig` file. Looking at the [Jetbrains documentation](https://www.jetbrains.com/help/idea/2016.2/running-intellij-idea-as-a-diff-or-merge-command-line-tool.html), using IDEA as a difftool seems pretty simple: just pass `diff` as the first argument, followed by two file names. It is more or less the same thing with merging: pass `merge` followed by the two conflicting files, the base file and the output file. However, how will this work with Git history?

The IDEA documentation gives a link to a [nice blog post](http://brian.pontarelli.com/2013/10/25/using-idea-for-git-merging-and-diffing/) explaining how to set this up for Mac, which we can use to figure out the Windows structure:

	# merge
	cmd = idea merge $(cd $(dirname "$LOCAL") && pwd)/$(basename "$LOCAL") $(cd $(dirname "$REMOTE") && pwd)/$(basename "$REMOTE") $(cd $(dirname "$BASE") && pwd)/$(basename "$BASE") $(cd $(dirname "$MERGED") && pwd)/$(basename "$MERGED")
	# diff
    cmd = idea diff $(cd $(dirname "$LOCAL") && pwd)/$(basename "$LOCAL") $(cd $(dirname "$REMOTE") && pwd)/$(basename "$REMOTE")
   
So now we have everything we need:

- A symlink to the IDEA progrom folder with no special characters
- A command to invoke Windows batch scripts from a bash shell
- The paramaters we need to pass `idea.bat` to make it diff and merge

The final part of the puzzle is figuring out which special characters to escape when setting up the Git commands. The result (added to `.gitconfig`):

	[mergetool "intellij"]
		cmd = cmd \"/C D:\\workspace\\tools\\symlink\\idea\\bin\\idea.bat merge $(cd $(dirname "$LOCAL") && pwd)/$(basename "$LOCAL") $(cd $(dirname "$REMOTE") && pwd)/$(basename "$REMOTE") $(cd $(dirname "$BASE") && pwd)/$(basename "$BASE") $(cd $(dirname "$MERGED") && pwd)/$(basename "$MERGED")\"
	[difftool "intellij"]
		cmd = cmd \"/C D:\\workspace\\tools\\symlink\\idea\\bin\\idea.bat diff $(cd $(dirname "$LOCAL") && pwd)/$(basename "$LOCAL") $(cd $(dirname "$REMOTE") && pwd)/$(basename "$REMOTE")\"

The final step is to tell Git to make use of the configured `intellij` commands:

	[merge]
		tool = intellij
	[diff]
        tool = intellij
        guitool = intellij

My full `.gitconfig` file is available [as a Gist](https://gist.github.com/AesSedai101/cb4d896b68ab55e3991b).