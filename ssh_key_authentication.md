
# Setting Up SSH Key Authentication

This guide walks you through creating an SSH private-public key pair, configuring permissions, and setting up passwordless login.

## 1. Generating the SSH Key Pair

1. Open your terminal (Git Bash, WSL, or any Linux/Unix terminal).
2. Run the following command to generate a new SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
   - **Explanation**:
     - `-t rsa`: Specifies the type of key (RSA in this case).
     - `-b 4096`: Sets the key length to 4096 bits for enhanced security.
     - `-C "your_email@example.com"`: Adds a label for the key.

3. When prompted, press **Enter** to accept the default file location (`~/.ssh/id_rsa`).

4. Optionally, enter a **passphrase** for additional security, or press **Enter** for no passphrase.

## 2. Setting the Correct Permissions (Local Machine)

To ensure SSH works correctly, set appropriate permissions for the key files:

1. Set permissions for the private key:
   ```bash
   chmod 600 ~/.ssh/id_rsa
   ```
   - This makes the private key readable and writable **only** by your user.

2. Set permissions for the public key:
   ```bash
   chmod 644 ~/.ssh/id_rsa.pub
   ```
   - This makes the public key readable by anyone but only writable by you.

3. Ensure the `.ssh` directory itself has the correct permissions:
   ```bash
   chmod 700 ~/.ssh
   ```
   - This makes the directory accessible only by your user.

## 3. Copying the Public Key to the Remote Server

### Method 1: Using `ssh-copy-id` (if available)

1. Run the following command to copy your public key to the remote server:
   ```bash
   ssh-copy-id username@remote_host
   ```
   - Replace `username` with your username on the server and `remote_host` with the server’s IP or hostname.
   - This command automatically appends your public key (`~/.ssh/id_rsa.pub`) to the `~/.ssh/authorized_keys` file on the server.

### Method 2: Manually Copying the Key

If `ssh-copy-id` is not available:

1. Display the contents of your public key:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
   - Copy the entire output.

2. Connect to your remote server using SSH:
   ```bash
   ssh username@remote_host
   ```
   
3. Once logged in, create the `.ssh` directory if it doesn’t exist:
   ```bash
   mkdir -p ~/.ssh
   ```

4. Open the `authorized_keys` file:
   ```bash
   nano ~/.ssh/authorized_keys
   ```
   - Paste the public key you copied earlier into this file.
   - Save and exit (in `nano`, press `Ctrl + O` to save and `Ctrl + X` to exit).

5. Set the correct permissions on the server:
   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

## 4. Verifying the SSH Key Authentication

1. Log out of the remote server if you’re still connected.
2. Attempt to reconnect using SSH:
   ```bash
   ssh username@remote_host
   ```
   - If everything is configured correctly, you should not be prompted for a password.

## 5. Troubleshooting

- If passwordless login is not working:
  - Ensure the `.ssh` directory and `authorized_keys` file on the server have the correct permissions (`700` and `600`, respectively).
  - Verify that your public key was correctly pasted in the `authorized_keys` file.
  - Check the SSH server configuration (`/etc/ssh/sshd_config`) to ensure the following options are enabled:
    ```
    PubkeyAuthentication yes
    AuthorizedKeysFile .ssh/authorized_keys
    ```
  - Restart the SSH service if any changes are made:
    ```bash
    sudo systemctl restart ssh
    ```
