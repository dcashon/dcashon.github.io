---
layout: post
title: "Jupyter Notebook Tunneling On UW Hyak For Tensorflow"
comments: true
description: "Jupyter Notebook Tunneling On UW Hyak For Tensorflow"
keywords: "dummy content"
---

Via STF, UW students get free access to GPU resources (NVIDIA Tesla P100s) on Hyak. This is a great alternative to using other cloud services. If you are a UW student and haven't used Hyak before, you can obtain an account at the following:

<http://students.washington.edu/hpcc/>

## Setting up your Python environment

On initial connection, forward the usual Jupyter Notebook local port to the login node - connect to Hyak using:

~~~~
ssh -L localhost:8888:localhost:8888 $USERNAME@mox.hyak.uw.edu
~~~~

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
source activate neural_nets
conda install tensorflow-gpu scikit-learn jupyter
~~~~

4. Once your environment is ready to go, exit the compute node and return to the login node.

~~~~
exit
~~~~

5. At this point, you should see @mox1 or @mox2 after your login name in terminal. Start screen to multiplex:

~~~~
screen
~~~~

Create another window with Ctrl-A c, then switch back to your main window with Crtl-A Crtl-A. In that window execute what follows.


## Getting a GPU Compute Node

Request an interactive GPU node:

~~~~
srun -p stf-int-gpu -A stf --nodes=1 --mem=120G  --time=1:00:00 --gres=gpu:P100:1 --pty /bin/bash
~~~~

You can modify the time and other parameters as needed.

Load in Anaconda and CUDA, then hop into your conda env from earlier:
~~~~
module load anaconda3_5.3 cuda/10.1.105_418.39
source activate neural_nets
~~~~

## Setting up port forwarding for Jupyter notebook
At this step, you should have 2 terminals open, one in the entry node @mox1 or @mox2 and another in your allocating GPU compute node, denoted by nXXXX. 

1. Start a jupyter notebook server in your GPU node:

~~~~
jupyter notebook --no-browser --port=8888
~~~~

2. Switch back to the login node (Ctrl-A Ctrl-A) then port forward (you have to enter the specific node that Hyak allocates to you, nXXXX):

~~~~
ssh -L localhost:8888:localhost:8888 nXXXX
~~~~

3. You should now be able to access the notebook via your local browser by navigating to localhost:8888. Copy and paste the login token and you are good to go. Check that tensorflow recognizes your GPU by executing the following in a Jupyter cell:

~~~~
import tensorflow as tf
tf.test.is_gpu_available()
~~~~

If returns true, you are ready to rumble. If you can't access the notebook via your local browser, ensure that you port forwarded to the login node upon initial connection to Hyak, and also that you port forwarded to the GPU node.

## Practicalities
When you create environments in conda, they seem to be placed in your home directory. On Hyak this has a 10GB quota - creating an environment with Tensorflow, jupyter, and scikit-learn already reaches a decent size due to all the dependencies. If you get File I/O errors you may be over your home directory quota - check using:

~~~~
mmlsquota --block-size G gscratch:home 
~~~~




### References
Helpful guide to Anaconda GPU stuff

<https://docs.anaconda.com/anaconda/user-guide/tasks/gpu-packages/https://docs.anaconda.com/anaconda/user-guide/tasks/gpu-packages/>

Hyak Wiki:

<https://wiki.cac.washington.edu/display/hyakusers/WIKI+for+Hyak+users>





