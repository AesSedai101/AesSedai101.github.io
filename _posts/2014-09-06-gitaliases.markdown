---
layout: post
title:  "Git aliases"
date:   2014-09-06 12:00:00
categories: git alias
---
Git is a very powerful tool, but it can be tricky to learn and some of the commands can be hard to remember. Aliases make this a bit simpler by allowing you to essentially define custom commands. Some aliases I use are listed below.

## Amend a commit

Just a shortcut to amend a commit.

    amend = commit --amend

## Show all files that are ignored by Git

This is useful to test that your `.gitignore` file is set up correctly.

    ignored = ls-files --others -i --exclude-standard

## No-prompt merge
	
In most cases, the merge tool will prompt before opening each file with merge conflicts. Fortunately, getting around this is quite simple.

    mt = mergetool --no-prompt

## Resolve all merge conflicts and continue rebase
	
When you are rebasing, you want to automatically continue rebasing after all merge conflicts have been resolved.

    mtr = !git mt && git rebase --continue

## Standup
	
Maybe, like me, you can not remember what you did two hours ago, let alone yesterday. Luckily, you can see all the work you did yesterday by creating an alias.

    standup = "!git lg --all --since yesterday --author ` git config --global --get user.email`"

## Search for a tag
	
The 'showtag' command seems useless at first, but used with other commands, it becomes very powerful. This will simply display the first tag matching the given regex.

    showtag = !sh -c 'git describe --tags --abbrev=0 --match=$1' - 

## Show unpushed commits
	
Calling this command just before you go home can save you some headaches. It will list all the commits that you have not yet pushed to the remote repo.

    unpushed = log --branches --not --remotes --oneline 

## Pretty logs
	
Some shortcuts for displaying history.

    lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit 

    ls = log --pretty=format:"%Cgreen%h\\ %C(yellow)%ad%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --date=short 

## Search inside a changeset
	
Searching for a commit where a specific variable's value was changed is a mission, hence the find command.

    find = !sh -c 'git log -G$1 --pretty=format:"%Cgreen%h\\%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate' - 

## Show changes since last build	

Git can also show you changes you made since the last build, assuming you tag each build. In this case, we start each build tag with 'Build'.

    changeset = "!git showtag 'Build*' && git ls --no-merges `git showtag 'Build*'`..HEAD" 

## List all aliases

Going a bit meta: a command to list all the aliases you defined:

    la = "!git config -l | grep alias | sort | cut -c 7-" 

## Update remote branches
	
Since branching is cheap in Git, developers will often create working branches and delete them once the changes has been merged. This means that the list of branches (listed using git branch -a) will often become cluttered with remote branches that no longer exist. A git alias to remove all deleted remote branch references from your local repo:

    update = remote prune origin

## List commits in chronological order
	
Often, you need to add release notes to a specific build. You can generate your release notes from git using

    changelist = log --pretty=format:"%s" -–reverse

This will list all commit messages with the earliest message first (chronological order).

## Generate release notes

Combining the changelist command with some of the commands mentioned in my previous post can help you generate release notes quickly.

    release = "!git showtag 'Release*' && git changelist --no-merges `git showtag 'Release*'`..HEAD" 
	
This command assumes that releases are tagged in such a way that the tag starts with the word 'Release'. It makes use of the `changelist` and `showtag` aliases to show all commits since the previous release in chornological order.
