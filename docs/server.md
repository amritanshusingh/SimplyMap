## Setting up Amazon AWS EC2 t2.micro instance

It is pretty straightforward.  
1. Sign up for Amazon AWS Free Tier.  
2. Log into the Amazon AWS Console and go to EC2 Instances.  
3. Create a fresh t2.micro instance. It is a CLI-based server.  
4. Launch the t2.micro instance. You'll be able to see its public IPv4 address.  
5. There is an option to generate an SSH key. The key file will have a `.pem` extension. Download it and keep it safe.  
6. Now that we have our server set up, we can SSH into it. In our terminal, we run:

   ```bash
   ssh -i t2microKey.pem ubuntu@XX.XX.XX.XXX
   ```

   Replace `t2microKey.pem` with the path to your downloaded key file and `XX.XX.XX.XXX` with your server's public IPv4 address. `ubuntu` is the default username for the server.

### Security Note
Ensure that your `.pem` file has the correct permissions. You can set the appropriate permissions using the following command:

```bash
chmod 400 t2microKey.pem
```

## Installing required tools