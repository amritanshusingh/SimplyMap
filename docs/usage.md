# Setting up MkDocs

In this section, we will see through the steps required to set up MkDocs locally and link GitHub Pages to our local setup so that whenever we push the code to our GitHub Repository, our documentation hosted on GitHub Pages automatically gets updated.

For the purpose of this setup guide we will assume our operating system to be Ubuntu 64 bits.

## Pre-requisites

Install Python and Pip (if not already installed):

``` bash title="bash"
sudo apt update
sudo apt install python3 python3-pip
```

In case of a fresh installation of ubuntu, you won't be having `git` installed
To Install Git Simply follow :

``` bash title="bash"
sudo apt install git
```

## Step 1 - Installing MkDocs and it's material theme

We can simply begin with creating a directory to start working 

``` bash title="bash"
mkdir ~/Desktop/MkDocs_Sandbox
cd ~/Desktop/MkDocs_Sandbox
```

Now since we are inside our sanbox directory, we can begin MkDocs installation. Its a good practice to create a python virtual environment as installation of MkDocs might thrown some errors if installing globally. Run the following command to create a virtual environment (we'll call it venv)

```bash title="bash"
python3 -m venv venv
```

This only needs to be done once. Next time while starting the work, we just need to activate the virtual environment and not create it once more. If we run `python3 -m venv venv` everytime we start the terminal, we will have to reinstall mkdocs and mkdocs-material.
By now our director structure should look like this 

```
~/Desktop/MkDocs_Sandbox
├── venv/
```

To activate the Python virtual environment, run the following command:

```bash title="bash"
source venv/bin/activate
```

Once activated, your terminal prompt will change to indicate that the virtual environment is active. You can now proceed with installing MkDocs and its dependencies. To read more about Python virtual environment head over [here](https://docs.python.org/3/library/venv.html)

Now, install MkDocs using pip within the active virtual environment:

```bash title="bash"
pip install mkdocs
```

Similarly, install the Material for MkDocs theme

```bash title="bash"
pip install mkdocs-material
```

## Step 2 - Creating MkDocs Project

Remember that we were in MkDocs_Sandbox directory? i.e ~/Desktop/MkDocs_Sandbox
run:

```bash title="bash"
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

```yaml title="yml"
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

```bash title="bash"
   cd docs
   echo "# Welcome to SimplyMap Documentation" > index.md
```

2. **Create `api.md`**:
   Similarly, create the `api.md` file to document the API reference.

```bash title="bash"
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


## Step 4 - Build Your MkDocs Site:

Build your MkDocs site while the virtual environment is still active:
```bash title="bash"
mkdocs build
```
This will create the `site` directory within your `simplymap-docs` project.

## Step 5 - Configure GitHub Pages Deployment within the Virtual Environment:

Install the gh-deploy plugin within the active virtual environment:
```bash title="bash"
pip install mkdocs-material-extensions  # If you are using Material for MkDocs
pip install mkdocs-git-revision-date-plugin # Optional
pip install mkdocs-deploy-gh-pages
```

## Step 6 - Initialize Git for Deployment

Before deploying to GitHub Pages, you need to initialize Git in your `simplymap-docs` project directory and link it to your GitHub repository. Follow these steps:

1. **Initialize Git**:
   Navigate to the root of your `simplymap-docs` project and initialize a Git repository.

```bash title="bash"
   cd ~/Desktop/MkDocs_Sandbox/simplymap-docs
   git init
```

2. **Add Files to the Repository**:
   Add all the files in your project to the Git repository.

```bash title="bash"
   git add .
```

3. **Commit the Changes**:
   Commit the added files with an appropriate commit message.

```bash title="bash"
   git commit -m "Initial commit for MkDocs project"
```

4. **Link to a GitHub Repository**:
   Add the remote URL of your GitHub repository. Here assuming that SSH URL of our git repository is : git@github.com:amritanshusingh/SimplyMap.git

```bash title="bash"
   git remote add origin git@github.com:amritanshusingh/SimplyMap.git
```

5. **Push the Changes**:
   Push the changes to the `main` branch of your GitHub repository. If your default branch is `master`, you can rename it to `main` using the following commands:

```bash title="bash"
   # Rename the default branch to main
   git branch -M main

   # Push the changes to the remote repository
   git push -u origin main
```

   This ensures that your repository uses `main` as the default branch, which is now the standard for most Git hosting platforms like GitHub.

## Step 6.1 - Generate SSH Keys for GitHub Authentication

If you encounter an authentication error when running `git push -u origin main`, you need to set up SSH keys for GitHub authentication. Follow these steps:

1. **Generate an SSH Key Pair**:
   Run the following command to generate a new SSH key pair. Replace `your_email@example.com` with the email address associated with your GitHub account.

   ```bash title="bash"
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

   When prompted, press `Enter` to save the key in the default location (`~/.ssh/id_ed25519`) and optionally set a passphrase for added security.

2. **Add the Public Key to GitHub**:
   Copy the contents of your public key to your clipboard:

   ```bash title="bash"
   cat ~/.ssh/id_ed25519.pub
   ```

   - Log in to your GitHub account.
   - Go to **Settings** > **SSH and GPG keys** > **New SSH key**.
   - Paste the public key into the "Key" field, give it a title, and click **Add SSH key**.

3. **Start the SSH Agent**:
   Start the SSH agent to manage your private key:

   ```bash title="bash"
   eval "$(ssh-agent -s)"
   ```

4. **Add the Private Key to the SSH Agent**:
   Add your private key to the SSH agent:

   ```bash title="bash"
   ssh-add ~/.ssh/id_ed25519
   ```

5. **Test the SSH Connection**:
   Verify that your SSH key is correctly configured by testing the connection to GitHub:

   ```bash title="bash"
   ssh -T git@github.com
   ```

   If successful, you will see a message like: `Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.`

6. **Retry the Git Push Command**:
   Now that your SSH key is configured, retry the `git push` command:

   ```bash title="bash"
   git push -u origin main
   ```

   This should successfully push your changes to the `main` branch of your GitHub repository.

Now your project is ready for deployment to GitHub Pages.


## Step 7 -  Deploy to GitHub Pages within the Virtual Environment:

Run the deployment command while your virtual environment is active:
```bash title="bash"
mkdocs gh-deploy
```
This will build your site (if necessary), create or update the gh-pages branch, and push it to your GitHub repository.

## Step 8 -  Enable GitHub Pages for Your Repository:

1. Go to your repository: https://github.com/amritanshusingh/SimplyMap  
2. Click on Settings.  
3. Go to the Pages section.  
4. Set the "Source" to Deploy from a branch.  
5. Select the gh-pages branch.  
6. Ensure the "Folder" is set to /(root).  
7. Click Save.

Your documentation site will be available at https://amritanshusingh.github.io/SimplyMap/ after a short while.

## Step 9 - Deactivating the Virtual Environment (When Done):

When you are finished working on your documentation for the time being, you can deactivate the virtual environment:
```bash title="bash"
deactivate
```
Your terminal prompt will return to its normal state.


Everytime you make any changes to documentation pages (like index.md or usage.md etc) you'll have to stage your changes, commit and push to origin `main` branch. This push needs to be followed by 

```bash title="bash"
mkdocs gh-deploy
```
command to automatically re-build GitHub page SimplyMap Documentation static site and incorporate the changes from main branch to gh-pages branch which by the way acts as the source branch for our GitHub Pages SimplyMap Documentation site.
