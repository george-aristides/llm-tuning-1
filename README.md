### Finetuning Llama3.1-8b With Your Own Data For Dummies 

## Intro

Huge shoutout to Unsloth and Ollama here for making this accessible.

# Unsloth

I used unsloth's pre-made finetuning notebook for Llama3.1-8b, link here:
https://github.com/unslothai/unsloth/blob/main/README.md

Unsloth does a lot of heavy lifting for you and makes these pre-made notebooks for finetuning certain models. You can grab it from the link, but for a first pass I recommend taking the version of the notebook I upload here, since I simplified it even further.

I modified it to fit my own needs, though I failed to notice that there was also a dedicated ollama notebook and I will consider this for my next iteration.

# Ollama 
I used Llama3.1-8b and I pulled it from Ollama, which long story short does a lot of heavy lifting for you like unsloth and manages LLM models you want to use.
For Ollama link here: 
https://github.com/ollama/ollama?tab=readme-ov-file

If you want to run this locally on your laptop, download the mac version. I am on mac, so I have no insight into anything related to windows as of now.

If you want to run this on AWS, considering the instance and AMI type you will need to install with the Linux manual installation instruction.

Once you have installed it on mac, you should be able to open the terminal application and run Ollama commands as instructed. 

# AWS (optional)
I run this on an AWS instance, though I have done it locally on my laptop as well. If you don't want to do this then just don't.

In order to do so, I created an EC2 instance and requested 4 vcpu cores on the account level in order to run a g4dn.xlarge type instance. This request took a few days to be reviewed and granted. 
The AMI I chose for this instance was Deep Learning OSS Nvidia Driver AMI GPU TensorFlow 2.16 (Amazon Linux 2) 20240817. 

Generally these specs are necessary if you want to reproduce this, the vcpu space is not given by default and 4 cores are required to run a g4 type instance, while something like a t2.micro is available with a default free plan, but is too small to store and run an LLM. To spare other details the AMI is pre-built for running Running on an AWS instance is not stricly necessary, since as I previously mentioned I can also run this locally and have done so, but it is nice to offload compute and storage space to AWS especially if your device is limited in those areas.

## Instructions

1. **Installing Llama3.1-8b**
Make sure you have properly installed Ollama on your machine. 

To install Llama3.1-8b locally, in your terminal run $ ollama pull llama3.1 

If you want to test this on its own, run $ ollama run llama3.1

You can now chat with it just like ChatGPT. To close it control + D should do.

2. **Running the Tuning Notebook**
Now you need to open the notebook in Google Colab. 












