# Installing Plex Media Server with Docker Compose on Ubuntu

## Prerequisites

Before installing Plex with Docker Compose, ensure your system meets the following prerequisites:

- **Operating System**: Ubuntu 18.04 or newer
- **Docker**: Installed and running (see instructions below)
- **Docker Compose**: Installed and configured
- **Storage**: Sufficient storage for your media files and Plex data
- **Network**: A stable internet connection

### Step 1: Install Docker

If Docker is not already installed on your system, follow these steps to install it:

1. **Update your system**:

    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

2. **Install Docker**:

    ```bash
    sudo apt install -y docker.io
    ```

3. **Start and enable Docker**:

    ```bash
    sudo systemctl start docker
    sudo systemctl enable docker
    ```

4. **Add your user to the Docker group** (optional but recommended):

    ```bash
    sudo usermod -aG docker $USER
    ```

   Log out and back in to apply the group membership.

### Step 2: Install Docker Compose

1. **Download Docker Compose**:

    ```bash
    sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -oP '"tag_name": "\K(.*)(?=")')/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```

2. **Make the Docker Compose binary executable**:

    ```bash
    sudo chmod +x /usr/local/bin/docker-compose
    ```

3. **Verify the installation**:

    ```bash
    docker-compose --version
    ```

### Step 3: Set Up Docker Compose for Plex

1. **Create a directory for Plex**:

    ```bash
    mkdir -p ~/plex
    cd ~/plex
    ```

2. **Create a `docker-compose.yml` file**:

    ```bash
    nano docker-compose.yml
    ```

    Add the following content to the file:

    ```yaml
    version: "3.8"
    services:
      plex:
        image: linuxserver/plex
        container_name: plex
        environment:
          - PUID=1000 # Update with your user ID
          - PGID=1000 # Update with your group ID
          - VERSION=docker
        volumes:
          - ./config:/config
          - /path/to/your/media:/media
        ports:
          - 32400:32400/tcp
          - 3005:3005/tcp
          - 8324:8324/tcp
          - 32469:32469/tcp
          - 1900:1900/udp
          - 32410:32410/udp
          - 32412:32412/udp
          - 32413:32413/udp
          - 32414:32414/udp
        restart: unless-stopped
    ```

    - **PUID and PGID**: These should match the user and group ID of the user that will own the Plex files. You can find your user ID and group ID with the `id` command.
    - **Volumes**: Mounts the directories on your host to the container. Replace `/path/to/your/media` with the path to your media library on your Ubuntu server.
    - **Ports**: Maps the container ports to your host. The default Plex port is `32400`.

### Step 4: Start Plex Media Server

1. **Deploy the container**:

    ```bash
    docker-compose up -d
    ```

   The `-d` flag runs the container in detached mode (in the background).

2. **Verify that Plex is running**:

    ```bash
    docker ps
    ```

   You should see the Plex container listed as running.

### Step 5: Access Plex Media Server

Once Plex is running, you can access it via your web browser at:

```plaintext
http://your_server_ip:32400/web
```

Replace `your_server_ip` with the IP address of your Ubuntu server.

### Step 6: Update the Plex Container

To update Plex when a new version is available:

1. **Pull the latest image**:

    ```bash
    docker-compose pull
    ```

2. **Recreate and restart the container**:

    ```bash
    docker-compose up -d
    ```

This will pull the latest image, recreate the container, and start it with the new version.

### Step 7: Stop and Remove Plex Container

If you need to stop Plex or remove the container:

1. **Stop the container**:

    ```bash
    docker-compose down
    ```

2. **Remove the container and its volumes**:

    ```bash
    docker-compose down --volumes
    ```

### Step 8: Configure Firewall (Optional)

If you are running a firewall on your server, you’ll need to allow the Plex ports:

```bash
sudo ufw allow 32400/tcp
sudo ufw allow 32469/tcp
sudo ufw allow 3005/tcp
sudo ufw allow 8324/tcp
sudo ufw allow 1900/udp
sudo ufw allow 32410:32414/udp
```

### Conclusion

You have successfully installed Plex Media Server using Docker Compose on Ubuntu. This setup allows for easier management and portability of Plex, especially when upgrading or migrating to a new server. 

For more advanced configurations, consider customizing the `docker-compose.yml` file according to your specific needs.
