---
layout: post
title: "Githug playing time"
description: ""
category: 
tags: [git, github]
---
{% include JB/setup %}


最近玩了一下[Githug](https://github.com/Gazler/githug)，在游戏中测试玩家对git的了解，非常有意思，我想能不google不查帮助通关的人肯定不多吧，反正我是一路hint过来了，这里把攻略流程记一下，方便以后自己查阅了，也希望可以帮到卡在哪里的玩家。

#### Initialized empty git repo in …

just cd to the target directory and use `git init`

#### There is a file in your folder called README, you should add it to your staging area

`git add -A`

#### The README file has been added to your staging area, now commit it.

`git commit -m "first commit"`

#### Set up your git name and email, this is important so that your commits can be identified

`git config user.name "fizz"`

`git config user.email "fizzwu@gmail.com"`

#### Clone the repository at https://github.com/Gazler/cloneme to 'my_cloned_repo'

`git clone https://github.com/Gazler/cloneme 'my_cloned_repo'`

#### The text editor 'vim' creates files ending in .swp (swap files) for all files that are currently open.  We don't want them creeping into the repository.  Make this repository ignore .swp files.

open `.gitigore`, add a line `*.swp`

#### A file has been removed from the working tree, however the file was not removed from the repository.  Find out what this file was and remove it.

`git status`

`git add -A`

`git commit -m "delete file"`

#### A file (deleteme.rb) has accidentally been added to your staging area, find out which file and remove it from the staging area.  *NOTE* Do not remove the file system, only from git.

`git rm --cached deleteme.rb`

#### We have a file called oldfile.txt. We want to rename it to newfile.txt and stage this change.

`git mv oldfile.txt newfile.txt`

#### The README file has been committed, but it looks like the file `forgotten_file.rb` was missing from the commit.  Add the file and amend your previous commit to include it.

`git add -A`

`git commit --amend -m "ammend commit"`

#### There are two files to be committed.  The goal was to add each file as a separate commit, however both were added by accident.  Unstage the file `to_commit_second` using the reset command (don't commit anything)

`git rm --cached to_commit_second.rb`

#### A file has been modified, but you don't want to keep the files.  Checkout the `config.rb` file from the last commit.

`git checkout config.rb`

#### The remote repositories have a url associated to them.  Please enter the url of remote_location

`git remote -v`

#### You need to pull changes from your origin repository.

`git pull origin master`

#### Add a remote repository called `origin` with the url `https://github.com/githug/githug`

`git remote add origin https://github.com/githug/githug`

#### There have been modifications to the `app.rb` file since your last commit.  Find out which line has changed.

`git diff app.rb`

#### Someone has put a password inside the file 'config.rb' find out who it was

`git blame config.rb`

#### You want to work on a piece of code that has the potential to break things, create the branch test_code

`git branch test_code`

#### Create and switch to a new branch called 'my_branch'.  You will need to create a branch like you did in the previous level

`git checkout -b my_branch`

#### You forgot to branch at the previous commit and made a commit on top of it. Create branch 'test_branch' at the commit before the last

`git branch test_branch HEAD~1`

HEAD~1 means roll back 1 commit

I got this answer at [here](http://stackoverflow.com/questions/1628563/git-move-recent-commit-to-a-new-branch)

#### We have a file in the branch 'feature'; Let's merge it to the master branch

`git merge feature`

#### Your new feature isn't worth the time and you're going to delete it. But it has one commit that fills in README file, and you want this commit to be on the master as well

`git checkout new_feature  #switch to the "new feature" branch`

`git log`

find the README commit hash ca32a6dac7b6f97975edbe19a4296c2ee7682f68

`git checkout master`

`git cherry-pick ca32a6dac7b6f97975edbe19a4296c2ee7682f68`

#### Correct the typo in the message of your first commit.

`git log`

then you got three commits : second commit, first commmit, initial commit

I need to change "first commmit" => "first commit"

`git rebase -i HEAD~2`

edit the text, change pick => reword

#### You have committed several times but would like all those changes to be one commit

`git log`

then you got 4 commits : update readme, update readme, update readme, init readme

`git rebase -i HEAD~3`

just delete the two below "update readme" line

#### Merge all commits from the long-feature-branch as a single commit.

`git merge --squash long-feature-branch `

`git commit -m "-"`

#### You have committed several times but in the wrong order. Please reorder your commits

`git log`

got 4 commits: second commit, third commit, first commit, initial commit

`git rebase -i HEAD~2`

pick cbf879e Third commit

pick c634e98 Second commit

switch two lines and save

#### You've made changes within a single file that belong to two different features, but neither of the changes are yet staged. Stage only the changes belonging to the first feature.

`git add -p feature.rb`

press e to edit

got three lines

this is the class of my feature

+This change belongs to the first feature

+This change belongs to the second feature

delete the last line

I googled answer [here](http://stackoverflow.com/questions/2372583/untangle-two-lines-with-git-add-p)

#### You have been working on a branch but got distracted by a major issue and forgot the name of it. Switch back to that branch

`git reflog`

`git checkout solve_world_hunger`

#### You have committed several times but want to undo the middle commit.

`git rebase -i HEAD~2`

remove the bad commit line








