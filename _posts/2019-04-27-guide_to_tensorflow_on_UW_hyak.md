---
layout: post
title: "Jupyter Notebook Tunneling On UW Hyak For Tensorflow"
comments: true
description: "Jupyter Notebook Tunneling On UW Hyak For Tensorflow"
keywords: "dummy content"
---

Via STF, UW students get free access to GPU resources on Hyak. This is a great alternative to using other cloud services. 

## Getting a GPU Compute Node

Request an interactive GPU node with the following:

'''
srun -p stf-int-gpu -A stf --nodes=1 --mem=120G  --time=1:00:00 --gres=gpu:P100:1 --pty /bin/bash
'''

You can modify the time and other parameters as needed.


## Setting up port forwarding for Jupyter notebook





