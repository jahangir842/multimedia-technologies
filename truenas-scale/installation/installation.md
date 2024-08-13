**TrueNAS Scale** is a powerful and scalable storage operating system based on Linux, designed for enterprise and home use. It provides a unified solution for file and block storage, and supports advanced features like data protection, replication, and high availability. Here's a detailed guide on how to install TrueNAS Scale:

### Prerequisites

1. **Hardware Requirements**:
   - A compatible server or workstation with sufficient CPU, RAM, and storage.
   - At least 8 GB of RAM (16 GB or more is recommended).
   - A bootable USB drive (minimum 16 GB).
   - Network connection.

2. **Download TrueNAS Scale**:
   - Download the latest TrueNAS Scale ISO from the [TrueNAS website](https://www.truenas.com/download-truenas-scale/).

### Installation Steps

#### Step 1: Create a Bootable USB Drive

1. **Download and Install Etcher**:
   - Etcher is a tool used to create bootable USB drives. Download it from [balena.io](https://www.balena.io/etcher/).

2. **Create the Bootable USB Drive**:
   - Insert your USB drive into your computer.
   - Open Etcher and select the TrueNAS Scale ISO file you downloaded.
   - Choose the USB drive as the target device.
   - Click “Flash!” to create the bootable USB drive.

#### Step 2: Boot from the USB Drive

1. **Insert the USB Drive** into the server or workstation where you want to install TrueNAS Scale.

2. **Access BIOS/UEFI Settings**:
   - Power on the machine and enter the BIOS/UEFI settings (typically by pressing a key like `F2`, `F12`, `Delete`, or `Esc` during boot).

3. **Set Boot Order**:
   - Set the USB drive as the first boot device in the boot order settings.

4. **Save and Exit**:
   - Save the changes and exit the BIOS/UEFI settings. The machine should boot from the USB drive.

#### Step 3: Install TrueNAS Scale

1. **Start the Installation**:
   - Once the system boots from the USB drive, you’ll see the TrueNAS Scale installer menu.
   - Select **"Install/Upgrade"**.

2. **Select the Installation Target**:
   - Choose the disk where TrueNAS Scale will be installed. This should be a dedicated disk, as all data on it will be erased.

3. **Confirm Installation**:
   - Review the disk selection and confirm that you want to install TrueNAS Scale. This will start the installation process.

4. **Wait for Installation to Complete**:
   - The installer will copy files to the selected disk and configure the system. This process typically takes a few minutes.

5. **Reboot the System**:
   - Once the installation is complete, you’ll be prompted to remove the USB drive and reboot the system.

#### Step 4: Initial Setup

1. **Access TrueNAS Scale Web Interface**:
   - After rebooting, TrueNAS Scale will start up and you’ll see an IP address on the screen.
   - Open a web browser on a computer connected to the same network and enter the IP address to access the TrueNAS Scale web interface.

2. **Login**:
   - The default username is `root` and the default password is the one you set during installation. If you haven't set a password, refer to the on-screen instructions for initial credentials.

3. **Configure Network and Other Settings**:
   - You will be prompted to configure network settings, create storage pools, and set up other system configurations.

4. **Set Up Storage Pools**:
   - In the web interface, navigate to **"Storage"** and create your storage pools according to your requirements. You can add and configure disks, create datasets, and set up shares (such as SMB, NFS, or iSCSI).

5. **Create Users and Permissions**:
   - Configure users and set permissions as needed for access control and security.

#### Step 5: Finalizing and Using TrueNAS Scale

1. **Install Updates**:
   - Check for any available updates in the web interface and apply them to ensure you have the latest features and security patches.

2. **Configure Additional Features**:
   - Explore additional features such as replication, snapshots, and plugins to enhance your TrueNAS Scale setup.

3. **Backup Configuration**:
   - It's a good practice to back up your TrueNAS Scale configuration regularly.

### Summary

Installing TrueNAS Scale involves creating a bootable USB drive with the installation image, booting the system from that USB drive, and following the installation prompts to set up TrueNAS Scale on your disk. After installation, you’ll configure the system through the web interface, set up storage, and configure other features based on your needs.