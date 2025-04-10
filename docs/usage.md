# Setting up MkDocs

In this section, we will see through the steps required to set up MkDocs locally and link GitHub Pages to our local setup so that whenever we push the code to our GitHub Repository, our documentation hosted on GitHub Pages automatically gets updated.

For the purpose of this setup guide we will assume our operating system to be Ubuntu 64 bits.

## Pre-requisites

Install Python and Pip (if not already installed):

```bash
sudo apt update
sudo apt install python3 python3-pip
```

In case of a fresh installation of ubuntu, you won't be having `git` installed
To Install Git Simply follow :

```bash
sudo apt install git
```

## Step 1 - Installing MkDocs and it's material theme

We can simply begin with creating a directory to start working 

```bash
mkdir ~/Desktop/MkDocs_Sandbox
cd ~/Desktop/MkDocs_Sandbox
```

Now since we are inside our sanbox directory, we can begin MkDocs installation. Its a good practice to create a python virtual environment as installation of MkDocs might thrown some errors if installing globally. Run the following command to create a virtual environment (we'll call it venv)

```bash
python3 -m venv venv
```

By now our director structure should look like this 

```
~/Desktop/MkDocs_Sandbox
├── venv/
```