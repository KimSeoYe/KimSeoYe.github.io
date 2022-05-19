---
layout: post
title:  "[Study] GitHub Action"
date:   2022-05-19 09:00:00
author: KimSeoYe
categories: Study
tags:   Study
cover:  "/assets/headers/hgu1.jpeg"
---

# Github Action
* [Official Document](https://github.com/features/actions)

## Learn Github Action
* Workflows
  * https://github.com/actions/starter-workflows 
* Events
* Jobs
  * Each step is either a shell script that will be executed, or an action that will be run.
  * Since each step is executed on the same runner, you can share data from one step to another.
  * You can configure a job's dependencies with other jobs
    * default : no dependencies => run in parallel
* Actions
  * a custom application for the GitHub Actions platform that performs a complex but frequently repeated task.
* Runners
* [Workflow syntax : events](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onpushpull_requestpull_request_targetpathspaths-ignore)
  * Q. on “push” ? 
  * on : push : paths-ignore : - ‘docs/**’ 같은 configuration을 할 수 있는 것 같은데, 이런 걸 통해 source의 change가 일어났을 때만 적용할 수 있도록 만들어야 하는 게 아닐까
