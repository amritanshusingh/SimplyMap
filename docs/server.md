## Setting up Amazon AWS EC2 t2.micro instance
It is pretty straight forward. 
1. Sign-up for Amazon AWS Free tier. 
2. Log into Amazon AWS Console and go to EC2 Instances.
3. Create a fresh t2.micro instance. It is CLI based server.
4. Launch t2.micro instance. You'll be able to see its public ipv4 address.
5. There is an option to generate an ssh key. It comes with a .pem extension. Download it and keep it safe
6. Now that we have our server setup we can ssh into it. In our terminal we go :
```bash
ssh -i t2microKey.pem ubuntu@XX.XX.XX.XXX
```
XX.XX.XX.XXX is our t2 micro server's public ipv4. ubuntu is our username for the server.

## Installing required tools