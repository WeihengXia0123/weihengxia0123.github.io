---
title: "Jupyterlab Tensorboard 500 Internal Server Error"
date: 2020-11-25T12:53:09+01:00
draft: false

categories:
- Machine Learning
tags:
- Jupyterlab
- Machine Learning
keywords:
- Jupyterlab
- Tensorboard 500 Error
---

Here's how to solve the error `Tensorboard Invalid response: 500 Internal Server Error in Jupyterlab.`
<!--more-->

## Pre-installation
``` zsh
❯ pip list | grep tensor
jupyter-tensorboard    0.2.0 
tensorboard            2.2.0
tensorboard-plugin-wit 1.7.0

❯ pip list | grep jupyter
jupyter-client         6.1.7    
jupyter-core           4.7.0    
jupyter-kernel-gateway 2.4.3    
jupyter-telemetry      0.1.0    
jupyter-tensorboard    0.2.0    
jupyterlab             2.2.9    
jupyterlab-pygments    0.1.2    
jupyterlab-server      1.2.0    
```

If you don't have the below dependencies, please install using pip or pip3, like:
`pip install jupyter-tensorboard`

## Error
Then when you launch jupyterlab and run the widget of tensorboard, you will get the follow error:

`Tensorboard Invalid response: 500 Internal Server Error in Jupyterlab.`
![500-Internal-Error](https://i.stack.imgur.com/cZAuM.png)
## Solve
I searched online for many materials, and finally this answer caught my attention, and solve my problem.

>[Error Reason]\
>This is likely due to some recent refactoring of Tensorflow. More specifically, the removal of TensorBoardWSGI. See https://github.com/tensorflow/tensorboard/pull/3643.\
>Currently, the easiest way to fix it will be to use a lower version of Tensorboard. The latest version where TensorBoardWSGI is available is 2.2.

Therefore, what we need to do is:
```zsh
> pip uninstall tensorboard
> pip install tensorboard==2.2
```
