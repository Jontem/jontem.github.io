---
title: Push a commit without triggering a pipeline in gitlab
date: 2022-06-01T00:00:00+00:00
layout: post
permalink: /push-a-commit-without-triggering-a-pipeline-in-gitlab/
categories:
  - gitlab
tags:
  - git
  - gitlab
  - gitlab-ci
---

If for some reason you don't wan't to to trigger a ci pipeline when doing a push to a gitlab remote you could add `[ci skip]` or `[skip ci]` to the last commits message.

Another way is to use git push options `git push -o ci.skip` which is available since git 2.10.

From the docs: https://docs.gitlab.com/ee/ci/pipelines/#skip-a-pipeline

