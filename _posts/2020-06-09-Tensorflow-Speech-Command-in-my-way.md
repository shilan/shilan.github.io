---
title: Tensorflow "Command Speech", in my way
author: Shilan Kalhor
date: 2020-06-09 15:34:00 +0800
categories: [Coding, Python, Machine_Learning, Tensorflow, Speech Processing]
tags: [Tensorflow, Command Speech, Machine Learning, Deep_Learning]
toc: true
visible: 1
---

There is a good [document](https://github.com/tensorflow/docs/blob/master/site/en/r1/tutorials/sequences/audio_recognition.md) in tensorflow github on how to train and test you neural network to make this work. However as a beginner with Tensorflow I had few bumps but I somehow managed to go through it. So I thought I will write down them here so that none of us forget it.

First of all, I wanted to make Tensorflow training to run on my windows machine which happened to have the Intel CPU which is not supported by Cuda.
That means you need to install Tensorflow with **CPU only.** Here is a good [document](https://medium.com/@teavanist/install-tensorflow-cpu-on-windows-10-4acbec6a71b7) on how to install Tensorflow CPU on windows 10.
Second when I installed Tensorflow on windows, I had no idea where it has installed itself. Surperisingly there was no indication to that during installation. 

The orginal Tensorflow [document](https://github.com/tensorflow/docs/blob/master/site/en/r1/tutorials/sequences/audio_recognition.md) says navigate to Tensorflow tree! but where is that?
On windows machine, if you use default setting and install using pip the tensorflow root folder should be installed somewhere like below:
`C:/users/[your-user-name]/Anaconda3/Lib/site-packages...` you should be able to find tensorflow folder here. :)
From command line navigate to this folder and only then try to run train command stated in the document.
`python tensorflow/examples/speech_commands/train.py`

Worth to mention some of the tensorflow versions are missin a dll file and you only realize that when you try to e.g run
a command like `python import tensorflow as tf` or when you try to run the train command.
For me 2.2 and 2.1 was like that. In that case try to downgrade to version 2.0 for example. That worked for me.

But wait that's not all! You install tensorflow and you realize the "examples" folder is either not in tensorflow folder or some part of it is missing.
To solve this issue I cloned (or you can just download the zip file) the entire tensorflow from github, unzip it or just navigate to example folder and copy paste it in your tensorflow tree root. 
There is a better alternative which is building from the tensorflow github source code. I haven't tried that though yet.
