---
title: Tensorflow "Command Speech" in my way. Windows platform CPU only
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

So now is the time, activate your tensorflow cpu mode, get python session and run the command. It should start training, soon you will see the convolution matrix. 
There is something that you should know though, you are running on cpu mode and training process will be drastically slow.
In the website was mentioned that it will take you few hours, for me with a brand new 5 core windows machine took 48 hours!!!
So don't bind your hope that you see the results soon because you won't!

##Customize the Training Classification##
You might wonder what if I want to run with less words (commands) or what if I want to add more custom commands.
Fortunately train.py comes with a handy parameter --wanted_words that you just add at the end of the command. There are even more samples in dataset like house, sheila .... Adding them in front of --wanted_words you can easily train your NN with them.
