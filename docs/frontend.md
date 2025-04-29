## Setting up the Frontend

This section will guide you through the steps required to set up the frontend for SimplyMap.

### Prerequisites

- A modern web browser
- Basic knowledge of HTML, CSS, and JavaScript

### Steps

1. Login to your Amazon AWS EC2 CloudSSH Console.
   ```bash
    # Download and install nvm:
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
    # in lieu of restarting the shell
    \. "$HOME/.nvm/nvm.sh"
    # Download and install Node.js:
    nvm install 22
    # Verify the Node.js version:
    node -v # Should print "v22.15.0".
    nvm current # Should print "v22.15.0".
    # Verify npm version:
    npm -v # Should print "10.9.2".
   ```

2. Now we have node and npm installed. The steps and versions may vary with time. Visit official nodejs wbesite for the latest steps.
    Now we will clone our React app into our t2.micro server. Run:
   ```bash
   git clone https://github.com/amritanshusingh/SimplyMapReact.git
   ```
   You will be able to see a new folder SimplyMapReact in your user directory on your server

3. Install dependencies:
   ```bash
   cd ~/SimplyMapReact
   npm install
   ```

4. Start the development server:
   ```bash
   npm start
   ```

Your frontend application should now be running locally.