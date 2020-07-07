+++
title = "Creating Ubuntu Image for Device"
date = 2020-06-26T18:21:37+05:30
description = ""
author = "Rahul Singh"
imageUrl="/images/rahul.png"
+++

In this blog, you’ll learn how to install ubuntu server **with password less SSH connection** without connecting your Raspberry Pi to router using ethernet cable, monitor, keyboard and mouse.

  **Components You Need:**
  
  To follow this tutorial, you need:
    
    - Raspberry Pi 2/3/4 board.

    - 16GB or more microSD card.

    - Power Adapter for Raspberry Pi.

    - A laptop or desktop computer for installing/flashing Ubuntu image on the SD card.


1) Download the Ubuntu image for your device in your **Downloads** folder using this link:
   
   ```
   https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.4&architecture=arm64+raspi3
   ```

2) Extract the image file using this command:
   
   ```
   gunzip -c ~/Downloads/ubuntu-18.04.4-preinstalled-server-arm64+raspi3.img.xz > ./raspi3.img
   ```

3) Mount the image file.
   
   ![ubuntu](/images/ubuntu-image.png)

4) Generate an ssh key for your new Pi.
   
   launch terminal and run this command **ssh-keygen**

   First, the tool asked where to save the file. SSH keys for user authentication are usually stored in the user's .ssh directory under the home directory.than press **return** ,Then it asks to enter a passphrase press **return** two times.

   ![SSH-Exmaple](/images/ssh-exmaple.png)

5) Edit the "user-data" file for  password less ssh connection.

   ```
    # Enable password authentication with the SSH daemon
    ssh_pwauth: false , for password less ssh connection give 'false' 

    # On first boot, set the (default) ubuntu user's password to "ubuntu" and
    # expire user passwords
    chpasswd:
     expire: false
    list:
    - ubuntu:Set your passwords for ubuntu user
    
    ssh_authorized_keys:
      - <ssh pub key 1>
    ```

    ```
    ## Add users and groups to the system, and import keys with the ssh-import-id
    users:
      - default
      - name: The user's login name
        gecos: The user name's real name
        sudo: ALL=(ALL) NOPASSWD:ALL
        ssh_authorized_keys:
          - <ssh pub key 1>
    ```

    Add this command in the last for rebooting the system:
    
    ```
    runcmd:
     - [ sudo, reboot ]
    ```

6) Edit the "network-config" file to setup Wifi . 

    ```  
    wifis:
      wlan0:
        dhcp4: true
        optional: true
        access-points:
          "abcd":
            password: "wxyz"
    ```

7) After editing comprees the ubuntu image file using this command:
    
   ```
    xz -z raspi3.img
    ```

8) Insert your microSD card and format it (macOS).
   
   Open a terminal window (Go to Application » Utilities, you will find the Terminal app there), then run the following command: **diskutil list**.Your removable drive must be DOS_FAT_32 formatted if not than go to disk utility and select your disk erase the data and change the format of disk to MS-DOS(FAT).

   ![SD](/images/sd-format.png)
   

9)  Unmount your microSD card with the following command:

   ```
    diskutil unmountDisk < drive address >
   ```

11) When successful, you should see a message similar to this one:
    
   ```
    **Unmount of all volumes on < drive address > was successful**
   ```

10)  You can now copy the image to the microSD card, using the following command:

   ```
    sudo sh -c 'gunzip -c ~/Downloads/raspi3.img.xz | sudo dd of=< drive address > bs=32m'
   ```

11) Once the microSD is ready then insert the SD card into the Raspberry Pi and plug it into the   power supply.Than wait for the device to boot.

12) SSH into the Raspberry Pi using this command.
    
    ```
    ssh -i ~/.ssh/custom_key_name user@server
    ```


Now,we have completed setting up the Raspberry Pi with Ubuntu Server **with password less SSH connection** init.
