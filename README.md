# zkSync Era Node Setup Guide

This guide explains step-by-step how to set up a `zkSync Era` node on Ubuntu Linux.

## System Requirements

- CPU: A relatively modern CPU is recommended.
- RAM: 32 GB
- Storage: 300 GB, with the state growing about 1TB per month.
- Network: 100 Mbps connection (1 Gbps+ recommended)

### Server Rental

If you need a server, you can rent one from Hetzner. Additionally, if you sign up using this referral link, you will receive a 20 EUR credit.

[Hetzner](https://hetzner.cloud/?ref=fu2umOyLCWhh)

## Step 1: Install Visual Studio Code (On Local Computer)

1. **Download and Install Visual Studio Code:**
   [Visual Studio Code Download Page](https://code.visualstudio.com/)

   - Once downloaded, follow the installation wizard to install Visual Studio Code.

2. **Install the SSH Extension:**
   - Open Visual Studio Code.
   - Click on the `Extensions` icon in the left sidebar.
   - Type `Remote - SSH` in the search bar and install the extension developed by Microsoft.

## Step 2: Connect to the Server via SSH (From Local Computer)

1. **Connect to SSH in Visual Studio Code:**

   - Press `Ctrl+Shift+P` in Visual Studio Code and select `Remote-SSH: Connect to Host...`.
   - Enter your server address in the format `ssh root@ip_address`.
   - Enter your password or SSH key to complete the connection.

   **Note:** Replace `ip_address` with the actual IP address of your server.

2. **Verify the Connection:**

   - If the connection is successful, the name of the connected server will appear in the lower-left corner of the Visual Studio Code window.

3. **Open the Terminal:**
   - After connecting to the server in Visual Studio Code, you can open the terminal by clicking on `Terminal` > `New Terminal` in the top menu.
   - Alternatively, you can open the terminal by pressing `Ctrl+` (Control key and the backtick key, located below the ESC key).

## Step 3: Install Docker and Docker Compose (On Server)

After successfully connecting to the server, execute these commands on the server.

### Docker Installation

1. **Update System Packages:**

   ```sh
   sudo apt update
   ```

2. **Install Docker:**

   ```sh
   sudo apt install docker.io
   ```

3. **Verify Docker Service is Running:**

   ```sh
   sudo systemctl status docker
   ```

4. **Verify Docker Installation:**
   ```sh
   docker --version
   ```

### Docker Compose Installation

1. **Download Docker Compose:**

   ```sh
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. **Give Execute Permission to Docker Compose:**

   ```sh
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. **Verify Installation:**
   ```sh
   docker-compose --version
   ```

## Step 4: Start the zkSync Era Node (On Server)

1. **Install Git (if not already installed):**

   ```sh
   sudo apt install git
   ```

2. **Clone the Repository:**

   ```sh
   git clone https://github.com/matter-labs/zksync-era.git
   ```

3. **Navigate to the Cloned Directories:**

   ```sh
   cd zksync-era/docs/guides/external-node
   cd docker-compose-examples
   ```

4. **Start the Node Using Docker Compose:**

   ```sh
   docker-compose -f mainnet-external-node-docker-compose.yml up -d

   ```

5. **Verify Running Containers:**

   ```sh
   docker ps
   ```

   This command lists currently running Docker containers. You should see the containers related to the `zkSync Era` node.

6. **View and Follow the Last 100 Logs:**
   ```sh
   docker logs -f --tail 100 docker-compose-examples-external-node-1
   ```

Press `Ctrl+C` to exit the log view. This will not stop your node; it will continue to run in the background.

## Step 5: Access the Dashboard via Port Forwarding (On Local Computer)

After running Docker, you can access a dashboard on port 3000. To view this dashboard from your local computer, you need to set up port forwarding in Visual Studio Code.

1. **Set Up Port Forwarding:**

   - Press `Ctrl+Shift+P` to open the command palette.
   - Type `Forward a Port` and select the option.
   - Enter `3000` for the port number and press Enter.

   Alternatively:

   - Click on the `PORTS` section in the bottom right corner of Visual Studio Code.
   - Select `Forward Port...` from the menu.
   - Enter `3000` for the port number and press Enter.

2. **Access the Dashboard:**

   - Open your web browser and go to `http://localhost:3000/d/0/external-node`.
   - You should now have access to the dashboard.

This step allows you to easily view the dashboard of the application running on your remote server from your local computer.

## Additional Information

- **View Container Logs:**

  ```sh
  docker logs <container_id>
  ```

- **Stop the Node:**
  ```sh
  docker-compose -f mainnet-external-node-docker-compose.yml down
  ```
