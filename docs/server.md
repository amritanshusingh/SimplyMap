## Setting up Amazon AWS EC2 t2.micro instance

It is pretty straight forward.  

1. Sign up for Amazon AWS Free Tier.  
2. Log into the Amazon AWS Console and go to EC2 Instances.  
3. Create a fresh t2.micro instance. It is a CLI-based server.  
4. Launch the t2.micro instance. You'll be able to see its public IPv4 address.  
5. There is an option to generate an SSH key. The key file will have a `.pem` extension. Download it and keep it safe.  
6. Now that we have our server set up, we can SSH into it. In our terminal, we run:

   ```bash
   ssh -i "t2microKey.pem" ubuntu@ec2-44-243-118-166.us-west-2.compute.amazonaws.com
   ```

   Replace `t2microKey.pem` with the path to your downloaded key file and `ec2-44-243-118-166.us-west-2.compute.amazonaws.com` is the server's public IPv4 domain name. `ubuntu` is the default username for the server.

### Security Note
Ensure that your `.pem` file has the correct permissions. You can set the appropriate permissions using the following command:

```bash
chmod 400 t2microKey.pem
```

## Installing required tools

Now that we have successfully SSH'ed into our remote server we can try out simple commands for example:
```bash title="bash"
$ ll
total 28
drwxr-x--- 4 ubuntu ubuntu 4096 Apr 10 11:47 ./
drwxr-xr-x 3 root   root   4096 Apr 10 11:40 ../
-rw-r--r-- 1 ubuntu ubuntu  220 Mar 31  2024 .bash_logout
-rw-r--r-- 1 ubuntu ubuntu 3771 Mar 31  2024 .bashrc
drwx------ 2 ubuntu ubuntu 4096 Apr 10 11:47 .cache/
-rw-r--r-- 1 ubuntu ubuntu  807 Mar 31  2024 .profile
drwx------ 2 ubuntu ubuntu 4096 Apr 10 11:40 .ssh/
```

We will be presented with the server's clean and default directory structure.