---
title: Run command for every commit in your git branch
date: 2023-04-22T00:00:00+00:00
layout: post
permalink: /run-command-for-every-commit-in-your-git-branch/
categories:
  - git
tags:
  - git
  - software-development
---

When working on a branch and your are rewriting your history often it's easy to come to a state when a commit doesn't compile. It's not obvious because your latest commit does compile. But it's good hygiene for your application to compile at every commit. Fortunately an interative rebase can help you with this:

For example: `git rebase -i master --exec "clear && yarn build"` will run the command specified for each commit since master. In this example I have script in my `package.json` which builds the application. If the build fails git rebase will stop and ask me to fix the error and do a `git rebase --continue`.
