Here's a detailed guide for installing Plex on Ubuntu:

### File: `plex/installation/ubuntu.md`

# Installing Plex Media Server on Ubuntu

## Prerequisites

Before installing Plex, ensure your system meets the following prerequisites:

- **Operating System**: Ubuntu 18.04 or newer
- **Processor**: Intel Core i3 or equivalent for transcoding (higher recommended)
- **Memory**: 2GB RAM minimum (4GB or more recommended)
- **Storage**: Sufficient storage for your media files
- **Network**: A stable internet connection

## Step 1: Update Your System

Start by updating your package list and upgrading your existing packages:

```bash
sudo apt update
sudo apt upgrade -y
```

## Step 2: Install Required Dependencies

Install any necessary dependencies:

```bash
sudo apt install -y curl apt-transport-https
```

## Step 3: Add the Plex Repository

Plex provides its own repository for easy installation and updates. Add the Plex repository to your system:

```bash
curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
```

## Step 4: Install Plex Media Server

Once the repository is added, update the package list and install Plex:

```bash
sudo apt update
sudo apt install -y plexmediaserver
```

## Step 5: Start and Enable Plex Media Server

After installation, start Plex and enable it to run on boot:

```bash
sudo systemctl start plexmediaserver
sudo systemctl enable plexmediaserver
```

## Step 6: Verify Plex Media Server Status

Check the status of the Plex Media Server to ensure it's running correctly:

```bash
sudo systemctl status plexmediaserver
```

You should see an output indicating that the service is active and running.

## Step 7: Access Plex Media Server

Plex Media Server can be accessed through your web browser. Use the following address:

```plaintext
http://your_server_ip:32400/web
```

Replace `your_server_ip` with the IP address of your Ubuntu server. This will take you to the Plex web interface, where you can complete the initial setup.

## Step 8: Configure Firewall (Optional)

If you are running a firewall on your Ubuntu server, you'll need to allow Plex through it:

```bash
sudo ufw allow 32400/tcp
```

This command opens the Plex Media Server port (32400) to allow access.

## Step 9: Set Up Plex Media Library

Once you access the Plex web interface, you'll be prompted to sign in or create an account. After signing in, follow the on-screen instructions to:

- **Name your server**: Give your Plex server a unique name.
- **Add your media library**: Point Plex to the directories where your media files are stored.

## Step 10: Updating Plex Media Server

Plex will automatically notify you of updates. To manually check for updates and upgrade Plex:

```bash
sudo apt update
sudo apt upgrade -y plexmediaserver
```

## Conclusion

You have successfully installed Plex Media Server on Ubuntu. You can now start organizing your media and streaming it to your devices. For advanced configurations, refer to the [Plex official documentation](https://support.plex.tv/articles/).

This guide should be placed in the `plex/installation/ubuntu.md` file within your repository.

---

## Troubleshooting:

Here are some additional sections you can add to the Plex installation guide on Ubuntu to make it more comprehensive:

### 1. Troubleshooting Installation Issues

#### Common Errors

- **Repository Not Found**: If you encounter a "Repository Not Found" error, double-check the repository URL and key. Ensure you have internet access on the server.
  
- **Plex Not Starting**: If Plex doesn’t start after installation, verify that the service is enabled and check the logs for errors:

  ```bash
  sudo journalctl -u plexmediaserver
  ```

- **Port Conflict**: If the Plex port (32400) is already in use, you can change the port by editing the Plex Media Server preferences:

  ```bash
  sudo nano /var/lib/plexmediaserver/Library/Application\ Support/Plex\ Media\ Server/Preferences.xml
  ```

  Find the `<Preferences>` tag and add or modify the `PlexOnlinePort` attribute:

  ```xml
  <Preferences PlexOnlinePort="32401" ... />
  ```

  Then restart Plex:

  ```bash
  sudo systemctl restart plexmediaserver
  ```

### 2. Securing Your Plex Server

#### Enable HTTPS (SSL/TLS)

To secure your Plex server with HTTPS:

1. **Use Plex’s Built-in SSL**: Plex provides its own SSL certificate for securing connections. Ensure that remote access is enabled in your Plex settings.

2. **Use a Custom SSL Certificate**: If you want to use your own SSL certificate, you can place your certificate files in the Plex configuration directory:

   ```bash
   sudo mkdir -p /var/lib/plexmediaserver/Library/Application\ Support/Plex\ Media\ Server/SSL
   sudo cp fullchain.pem /var/lib/plexmediaserver/Library/Application\ Support/Plex\ Media\ Server/SSL/
   sudo cp privkey.pem /var/lib/plexmediaserver/Library/Application\ Support/Plex\ Media\ Server/SSL/
   ```

   Then, modify the `Preferences.xml` file to use your custom certificate:

   ```xml
   <Preferences CustomCertificatePath="/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/SSL" ... />
   ```

   Restart Plex to apply the changes:

   ```bash
   sudo systemctl restart plexmediaserver
   ```

#### Configure Firewall Rules for Remote Access

To allow remote access securely:

- Only allow specific IP addresses if possible:

  ```bash
  sudo ufw allow from <your_ip> to any port 32400 proto tcp
  ```

- Use VPN: For more secure access, consider setting up a VPN and only allowing Plex traffic over the VPN interface.

### 3. Automating Backups of Plex Configuration

Regularly backing up your Plex configuration can save you time in case of data loss or corruption. You can automate this using a cron job:

1. **Create a Backup Script**:

   ```bash
   sudo nano /usr/local/bin/plex_backup.sh
   ```

   Add the following script content:

   ```bash
   #!/bin/bash
   tar -czvf /backup/plex_backup_$(date +%Y%m%d).tar.gz /var/lib/plexmediaserver/Library/Application\ Support/Plex\ Media\ Server/
   ```

   Make the script executable:

   ```bash
   sudo chmod +x /usr/local/bin/plex_backup.sh
   ```

2. **Schedule the Backup with Cron**:

   Edit your crontab:

   ```bash
   sudo crontab -e
   ```

   Add the following line to schedule a backup every week:

   ```bash
   0 2 * * 0 /usr/local/bin/plex_backup.sh
   ```

### 4. Uninstalling Plex Media Server

If you need to uninstall Plex Media Server:

```bash
sudo apt remove --purge plexmediaserver
```

Remove the Plex repository and clean up residual files:

```bash
sudo rm /etc/apt/sources.list.d/plexmediaserver.list
sudo apt autoremove -y
sudo apt clean
```

### 5. Migrating Plex to a New Server

To migrate Plex to a new server:

1. **Backup the Current Plex Configuration** (using the backup script mentioned above).
2. **Transfer the Backup** to the new server using `scp` or `rsync`:

   ```bash
   scp /backup/plex_backup.tar.gz user@new_server:/path/to/backup
   ```

3. **Install Plex on the New Server** (follow the installation steps).
4. **Restore the Backup** on the new server:

   ```bash
   sudo tar -xzvf /path/to/backup/plex_backup.tar.gz -C /var/lib/plexmediaserver/Library/Application\ Support/Plex\ Media\ Server/
   ```

5. **Start the Plex Server**:

   ```bash
   sudo systemctl start plexmediaserver
   ```

---

These additional sections can help make your Plex installation guide more robust and valuable to others who may be using your repository as a reference.
