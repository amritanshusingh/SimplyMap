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

Your frontend application should now be running.