## Setting up the Frontend

This section will guide you through the steps required to set up the frontend for SimplyMap.

### Prerequisites

- A modern web browser
- Basic knowledge of HTML, CSS, and JavaScript

### Steps

1. Build your application locally. In your application's root directory , run
   ```bash
   npm run build
   ```
   You will notice a new directory called `dist`

2. Copy this directory to your `~` directory on your EC2 instance. You can use FileZilla

3. Login to your Amazon AWS EC2 CloudSSH Console.
   ```bash
    # Download and install nginx
    sudo apt install nginx -y
   ```

4. Move the dist folder contents to Nginx’s web directory:
   ```bash
   sudo mv /home/ubuntu/dist/* /var/www/html/
   ```
5. Set Permissions
   ```bash
   sudo chmod -R 755 /var/www/html/
   sudo chown -R www-data:www-data /var/www/html/
   ```

   Start and enable Nginx
   ```bash
   sudo systemctl start nginx && sudo systemctl enable nginx
   ```

   Verify Nginx is running
   ```bash
   sudo systemctl status nginx
   ```

6. Vite apps often use client-side routing, so configure Nginx to redirect all requests to index.html.
   Edit the Nginx configuration :
   ```bash
   sudo nano /etc/nginx/sites-available/default
   ```

   Replace the server block with 
   ```bash
   server {
    listen 80;
    server_name _;
    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
   }
   ```
   Save (Ctrl+O, Enter, Ctrl+X) and test the config
   ```bash
   sudo nginx -t
   ```

   Reload Nginx
   ```bash
   sudo systemctl reload nginx
   ```

7. Configure EC2 Security Group:
   In the AWS Management Console, go to EC2 > Security Groups.
   Select the security group for your instance.
   Add an inbound rule if not present:
   Type: HTTP, Protocol: TCP, Port: 80, Source: Anywhere (0.0.0.0/0).
   Save changes.

8. Test your app 
   - Open a browser and visit http://your-ec2-public-ip.
   - If the app doesn’t load, check:
     - Nginx logs: `sudo tail -f /var/log/nginx/error.log`.
     - Files in `/var/www/html/`: `ls /var/www/html/`.
     - Permissions: Ensure `www-data` owns the files.
     - Base path: If your app is in a subdirectory, update `base` in `vite.config.js` (e.g., `base: '/subpath/'`), rebuild, and re-upload.

Your frontend application should now be running.