### Finetuning Llama3.1-8b With Your Own Data For Dummies 

## Intro

Huge shoutout to Unsloth and Ollama here for making this accessible.

This is my first ever successful iteration of this project, so while it works its also not great. Expect better versions in the future, and once you can run this please take the time to improve on what has already been done here.

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

You can look up how to copy files to an AWS instance from your machine with scp.

## Instructions

1. **Installing Llama3.1-8b**
Make sure you have properly installed Ollama on your machine. 

To install Llama3.1-8b locally, in your terminal run $ ollama pull llama3.1 

If you want to test this on its own, run $ ollama run llama3.1

You can now chat with it just like ChatGPT. To close it control + D should do.

2. **Running the Tuning Notebook**
Now you need to open the notebook in Google Colab. 

Check the example I provided, but the key will be the "Data Prep" cell. If you have your own dataset, you will want to import that as a dataframe. Depending on where it is stored, you will have to either upload it to colab from your device, or if it is in Google drive you can "mount drive" and pull it directly from drive to colab. I may provide instruction on this later, but ChatGPT and some Google searching should suffice for figuring this out. 

In the example notebook I provided, the dataset I generated was extremely small and I was able to manually enter it in. It had two columns called "question" and "answer" and five rows...

I did this because it was easy enough and my focus was not on anything related to pulling in a dataset and I just didn't feel like dealing. I just wanted to jump straight to the training -  retrospectively not the best practice, but I don't feel like going back and fixing such a trivial detail to yield the same result. The next iteration will use a larger dataset and will not manually input it.

Once you have your dataset properly imported (or manually entered), modify the formatting prompts and alpaca_prompt to reflect the columns of the dataset. You can see I created a fake dataset with a series of questions and answers and reflected that. 

I would avoid on first pass changing anything other than these two lines

questions = examples["question"]

answers   = examples["answer"]

where you can put your own column header names into the quotation marks. If you find a way to apply this to a dataset with more than two columns, congratulations. I have not even attempted that so you are ahead of me, though if you can manipulate this in any way I can't imagine that would be too hard. 

In terms of the alpaca_prompt, that was initially an example from a professionally made refined version of llama, so aside from the name you should consider a few things about the prompt.

The question and answer format should be self explanatory, the idea is that if you ask it a question about the dataset it will read that and give you an answer. The context piece comes into play if it needs more information or doesn't understand your question enough to provide an answer, and it will prompt you to fill in more info, sometimes it will even ask you a specific question. 

If you want to just try it for the sake of it and do as little as possible, go replace my manually inputted questions and answers with your own.

Once you have made any changes you want, you can run the notebook. I suggest running cells one at a time, top down. This may sound elementary considering anyone with experience may know that you can just click the run all option, but since some of these operations require time and compute power that the free version of google colab might struggle with, I found it best to go one at a time and diagnose issues as they come up and also avoid using all of the compute power/time on colab and being timed out.

I ended up paying $10 for a month of colab pro to zip my files up and download them since I kept timing out here and running out of disk space on the free plan.

I suggest if you want to avoid this, try only zipping and downloading the .gguf file, since thats all you are really going to need. In order to do this, find the last few cells where one says 

!zip -r model_8bit.zip ./model_8bit/

and just tack on unsloth.Q8_0.gguf to the end, so it looks like this

!zip -r model_8bit.zip ./model_8bit/unsloth.Q8_0.gguf

Once this is zipped, download this to your machine. 

3. **Creating the Tuned Model**

This is the easiest part. You now have Ollama installed, Llama3.1-8b pulled, and you have downloaded the .gguf file.

Copy the modelfile I have provided as a template. Open it and replace the filepath to the .gguf with your own. If you just put the zip file on your desktop and open it there, the only thing to change should be your username.

The filepath refers to this line

FROM /Users/your-username/Desktop/model_8bit/unsloth.Q8_0.gguf

Then in terminal, run this command

$ ollama create model-name -f /path/to/Modelfile

Where you replace model-name with whatever you want to name it and the path should the path to this modelfile, not the .gguf. For example, if my modelfile is just on my desktop I would just use

$ /Users/your-username/Desktop/modelfile-name 

Where of course modelfile-name is whatever the file is called.

To see if this worked type 

$ ollama list 

to show all of the models you have, you new one that you named should show up

to run it use the command

$ ollama run model-name

and try asking it questions about the data you put in.


# Good Luck!

-----------------------------------

If you want to try importing, I included the dataset I used as a csv.

I also will add the .gguf later if you want to try running the model I made and seeing how it responds based on the dataset I trained it on. You'll see it works, but it's not great in some areas and definitely needs much further refinement. The issue is the file is too big to upload to github normally, so I'll take care of it soon.


