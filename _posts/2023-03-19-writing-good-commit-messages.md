---
title: Writing good commit messages
date: 2023-03-19T00:00:00+00:00
layout: post
permalink: /writing-good-commit-messages/
categories:
  - git
tags:
  - git
  - software-development
---

## Why doesn't everyone write good commit messages

This is what I think:

- Lack of motivation
- Lack of time
- The organization or coworkers doesn't value good git history hygiene
- Never learned to do it
- Doesn't see the value

## Writing good commit messages is important

- It forces you to really think about and understand your change which makes you a better developer
- It makes reviewing your code easier which improves the quality of the code
- It spreads knowledge. Someone may never know as much about this change that you do right now
- It is documentation that doesn't lie. A good investment for the future
- It makes your git history easier to search with `git log --grep` and `git log -S`
- It makes it easier to automatically generate a changelog
- It can save tremendous amount of time for anyone trying to understand a piece of code

## What should a commit message look like

```
# (50-character subject line)
#
# 72-character wrapped longer description. This should answer:
#
# * Why was this change necessary?
# * How does it address the problem?
# * Were there any alternative solutions that you thought about?
```

## Tips for writing a good commit message

- When writing the subject line say "If applied, this commit will...". This will help you get the correct grammatical mood which forms a command or a request
- Don't use `git commit -m` as it don't let you write more than one line. Without specifying `-m` git will open your text editor of choice for writing the message
- Use a helpful commit message template `git config --global commit.template ~/.gitmessage`. Like the one above.
- Use `git config --global commit.verbose true` option. It lets you see the diff which will help you when writing your commit message.
- Think about the developers in your team. What questions would they have and try to answer them
- If your commit is a bugfix write about the bug, when it happens and why.
- If it's a feature write about the process of implementing it
- If it's a refactor. Write why it improves code

## Good reading materials

- [A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [5 Useful Tips For A Better Commit Message](https://thoughtbot.com/blog/5-useful-tips-for-a-better-commit-message)
- [How to Write a Git Commit Message](https://cbea.ms/git-commit/)
- [Pro Git](https://git-scm.com/book/en/v2)
