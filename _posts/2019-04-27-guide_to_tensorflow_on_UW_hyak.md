---
layout: post
title: "Jupyter Notebook Tunneling On UW Hyak For Tensorflow"
comments: true
description: "Jupyter Notebook Tunneling On UW Hyak For Tensorflow"
keywords: "dummy content"
---

Via STF, UW students get free access to GPU resources on Hyak. This is a great alternative to using other cloud services. 

## Setting up your Python environment

Go ahead and set up your environment on a normal interactive node. It seems the GPU nodes have issues with internet connection.

1. Get an interactive node:

~~~~
srun -p build --mem=100G --time=2:00:00 --pty /bin/bash
~~~~

2. Load anaconda module

~~~~
module load anaconda3_5.3
~~~~

3. Create conda environment, install tensorflow-gpu + other necessary packages.

~~~~
conda create -n neural_nets python=3.6
~~~~
~~~~
source activate neural_nets
~~~~
~~~~
conda install tensorflow-gpu scikit-learn jupyter
~~~~


## Getting a GPU Compute Node

Request an interactive GPU node with the following:

~~~~
srun -p stf-int-gpu -A stf --nodes=1 --mem=120G  --time=1:00:00 --gres=gpu:P100:1 --pty /bin/bash
~~~~

You can modify the time and other parameters as needed.




## Setting up port forwarding for Jupyter notebook





