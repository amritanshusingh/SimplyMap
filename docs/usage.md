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

To activate the Python virtual environment, run the following command:

```bash
source venv/bin/activate
```

Once activated, your terminal prompt will change to indicate that the virtual environment is active. You can now proceed with installing MkDocs and its dependencies. To read more about Python virtual environment head over [here](https://docs.python.org/3/library/venv.html)

Now, install MkDocs using pip within the active virtual environment:

```bash
pip install mkdocs
```

Similarly, install the Material for MkDocs theme

```bash
ppip install mkdocs-material
```

## Step 2 - Creating MkDocs Project

Remember that we were in MkDocs_Sandbox directory? i.e ~/Desktop/MkDocs_Sandbox
run:

```bash
mkdocs new simplymap-docs
cd simplymap-docs
```

By now our directory structure should look like this:

```
~/Desktop/MkDocs_Sandbox
├── venv/
├── simplymap-docs/
    ├── mkdocs.yml
    ├── docs/
        ├── index.md
        ├── usage.md
        ├── api.md
```

By now we have setup our barebones MkDocs Project by the name of simplymap-docs

## Step 3 - Configure MkDocs (mkdocs.yml):

Open the mkdocs.yml file in your project directory with a text editor and configure it as needed. Here's the same basic configuration using the Material theme:

```yaml
site_name: SimplyMap Documentation
theme:
  name: material
nav:
    - Home: index.md
    - Usage: usage.md
    - API Reference: api.md
```

Since the `nav` section in `mkdocs.yml` references `index.md` and `api.md`, we need to create these files in the `docs/` directory.

1. **Create `index.md`**:
   Navigate to the `docs/` directory and create the `index.md` file. This will serve as the homepage of your documentation.

   ```bash
   cd docs
   echo "# Welcome to SimplyMap Documentation" > index.md
   ```

2. **Create `api.md`**:
   Similarly, create the `api.md` file to document the API reference.

   ```bash
   echo "# API Reference" > api.md
   ```

After creating these files, your `docs/` directory should look like this:

```
~/Desktop/MkDocs_Sandbox/simplymap-docs/docs/
├── index.md
├── usage.md
├── api.md
```

Now, you can proceed to edit these files to include the content you want for your documentation.