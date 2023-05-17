---
title: Cherry-pick a range of commits with git
date: 2023-05-17T00:00:00+00:00
layout: post
permalink: /cherry-pick-range-of-commits-with-git/
categories:
  - git
tags:
  - git
  - software-development
---

When working in a feature branch I often find code that needs fixing or refactoring which I would like to keep regardless of the outcome of the feature I'am working on. So I make my changes in atomic commits then I use git rebase interative to put the commits first in the feature branch I'm on. The reason for that is so that I can see that the changes apply cleanly without my other changes in my branch. 

When I have collected a bunch of these commits I either cherry-pick them directly in to the main branch or another feature branch. Because I put all the commits in the beginning and in line I can cherry-pick a range of commits. To do this use

`git cherry-pick <the_first_commit>^..<the_last_commit>`

Notice the `^` which will make the `the_first_commit` point to the parent commit. The effect of `<the_first_commit>^` will be that the changes of `the_first_commit` is included, otherwise it will only include the child of that commit.