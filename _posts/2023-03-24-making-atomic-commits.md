---
title: Making atomic commits
date: 2023-03-24T00:00:00+00:00
layout: post
permalink: /making-atomic-commits/
categories:
  - git
tags:
  - git
  - software-development
---

## What is an atomic commit
An atomic commit about is about a single, small focused change. Other names for atomic commits are small commits, focused commits, self contained commits or deliberate commits. If you're trying to figure out if the commit you're working on is to big, it probably is.

## Why atomic commits

### 1. Helps plan your work
With atomic commits you are forced to split up your large task into smaller pieces. This is helpful because you'll have to think in detail about what you're going to do. This will help with cognitive load and you wont be afraid about getting interrupted and forget your current state. Small commits also helps with the feeling of making progress.

### 2. Easier to review
Atomic commits is a win both for you and the reviewer. Going through a code review with atomic commits is much easier. Each commit is about a single change and all small commits together tells a story. When every change is small and focused it's easier to explain your change with a good commit message. For the reviewer each small commit can be handled one at a time making it easier to find bugs, typos and things to improve.

### 3. Easy to revert, cherry-pick and drop
A small commit is simple to revert or drop because it just one small focused change. If you find something to refactor that is not related to the task your working on it should be cherry-picked to another branch. When rebasing your branch again git will automatically see the change already applied and remove it from your branch. 

### 4. Easier merge conflicts
When handling merge conflicts small commits will be a lot simpler.

### 5. Good for debugging with git bisect
Using `git bisect` it will help you to find the specific change which introduced a bug. If the commit is small the change introduced will be easy to spot.

### 6. Good documentation
Small focused commits are great for documentation. They explain the what, the why and at the time the change was made. Its' documnetation that doesn't lie.


## Useful tools

### Interactive rebase
Use `git rebase --interactive` early and often to revise your history and get the latest changes from upstream branches.

### Amend
`git commit --amend` for quickly fixing up the latest commit with extra files or change the commit message.

### Partially stage files
If multiple changes is made to a file which should be in separate commits use `git add --patch` which will let you add specific lines from a file.

### Commit verbose
When writing the commit message it's useful to see the changes directly in the editor. This can be done with `git commit --verbose` or setting it as a default value with `git config --global commit.verbose true`.


## Good reading materials
- [Make Atomic Git Commits](https://www.aleksandrhovhannisyan.com/blog/atomic-git-commits)
- [How focused commits make you a better coder](https://tekin.co.uk/2021/01/how-atomic-commits-make-you-a-better-coder)
- [A Branch in Time (a story about revision histories)](https://tekin.co.uk/2019/02/a-talk-about-revision-histories)
- [Simplify writing code with deliberate commits](https://www.youtube.com/watch?v=mE8DZUfhdm4)