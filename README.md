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