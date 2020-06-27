+++
title = "Creating Ubuntu Image for Device"
date = 2020-06-26T18:21:37+05:30
description = ""
author = "Rahul Singh"
imageUrl="images/rahul.png"
+++

1) Download the Ubuntu image for your device in your **Downloads** folder using this link:
   ```
   https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.4&architecture=arm64+raspi3
   ```

2) Extract the image file using this command:
   ```
    gunzip -c ~/Downloads/ubuntu-18.04.4-preinstalled-server-arm64+raspi3.img.xz > ./raspi3.img
    ```
3) Mount the image file.
   
4) Edit the "user-data" file.
    ```
    # Enable password authentication with the SSH daemon
    ssh_pwauth: false

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

5) Edit the "network-config" file. 
   ```  
    wifis:
      wlan0:
        dhcp4: true
        optional: true
        access-points:
          "abcd":
            password: "wxyz"
    ```

6) After editing comprees the ubuntu image using this command: 
   ```
    xz -z raspi3.img
    ```
7) Insert your microSD card.
   
8) Unmount your microSD card with the following command:
    ```
    diskutil unmountDisk < drive address >
    ```
9)  You can now copy the image to the microSD card, using the following command:
    ```
    sudo sh -c 'gunzip -c ~/Downloads/raspi3.img.xz | sudo dd of=< drive address > bs=32m'
    ```
