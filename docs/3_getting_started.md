---
title: Getting started
description: Learn how to build a swarm learning system and start your decentralized machine learning consortium.
icon: material/semantic-web
status:
---

# Getting started

!!! tip "HPE Swarm Learning"
    This project uses HPE Swarm Learning. For general documentation see: [https://github.com/HewlettPackard/swarm-learning](https://github.com/HewlettPackard/swarm-learning)


## Prerequisites

HPE Swarm Learning can run on any hardware that supports executing container software (Docker).

### Hardware
- Any x86-64 hardware
- System memory of 32 GB or more
- Hard disk space of 200 GB or more
- Qualified with HPE Edgeline, Proliant DL380, and Apollo 6500

!!! note
    HPE Swarm Learning can be deployed with Nvidia GPUs (accelerator cards) and AMD GPUS (accelerator cards).

### Supported Operating Systems and Platforms
HPE recommends that you run each Swarm Network node, and Swarm Learning node on dedicated systems to get the best performance from the platform. The recommended requirements for each system are as follows:
  
!!! note
    The requirements of system running the user ML node is driven by the complexity of the ML algorithm. GPUs may also be needed.
  
#### Network
- A minimum of one or a maximum four open TCP/IP ports in each node. All swarm nodes must be able to access the ports of every other node. 
- Stable internet connectivity to download Swarm Learning package and Docker images.

!!! info "Expoded ports"
    Depending on the type of Swarm Learning components that are running on a host, some or all these ports must be opened to allow the Swarm Learning containers to communicate with each other:

    - A Swarm Network peer-to-peer port on the hosts running Swarm Network nodes. By default, port **30303** is used.

    - A Swarm Network API server port on the hosts running Swarm Network nodes. By default, port **30304** is used.

    - Swarm Learning file server port on the hosts running Swarm Learning nodes. By default, port **30305** is used.

    - A License Server API port on the host running the License Server. By default, port **5814** is used.

    - (Optional). An SWCI API server port that is used by the SWCI node to run a REST based API service. By default, port **30306** is used.
  
#### Operating Systems
- Linux - Qualified on Ubuntu 22.04 RHEL 8.5. and SLES 15.0
- For Swarm Web UI installer, any x86-64 hardware running Linux, Windows, or Mac.
  
#### Container Hosting Platform
- HPE Swarm Learning is qualified with Docker 20.10.5.
- Configure Docker to run as a non-root user.
- Configure network proxy settings for Docker.
- Configure Docker to use IPv4.
  
#### Machine Learning Framework
- Qualified with Keras (TensorFlow 2 backend) and PyTorch 1.5 based Machine Learning models implemented using Python3.
  
#### Multi System Cluster Requirements
- Synchronized time across all systems using NTP.

## System preparation

!!! info "Operating system"
    All the following instructions are only tested on **Ubuntu**.

1. Update the system with **`Software Updater`** or in via **`terminal`** with:
    ```sh
    sudo apt update
    ```
    ```sh
    sudo apt upgrade
    ```
   
2. Install the **Nvidia driver** via `Software Updater → Settings → Additonal Drivers → NVIDIA driver metapackage from nvidia-driver-525 (proprietary) -> Apply Changes`.
   
3. **Restart** the System.
   
4. Test the successful driver installation with:
    ```sh
    nvidia-smi
    ```
   
5. Install **SSH** with:
    ```sh
    sudo apt install openssh-client openssh-server
    ```
   
6. Install **Git** with:
    ```sh
    sudo apt install git
    ```
   
7. Install **Curl** with:
    ```sh
    sudo apt install curl
    ```
   
8. Install **Docker** with:
    ```sh
    curl -fsSL get.docker.com | sudo sh
    ```

## Creation of swarm user and Download of the repository

1. **Create a user** named `swarm` and add it to the sudoers group:
    ```sh
    sudo adduser swarm
    ```
    ```sh
    sudo usermod -aG sudo swarm
    ```

2. **Login** with `swarm`:
    ```sh
    su - swarm
    ```

3. Run the following commands to **download the repository**:
    ```sh
    cd / && sudo mkdir opt/hpe && cd opt/hpe && sudo chmod 777 -R /opt/hpe

    ```
    ```sh
    git clone https://github.com/KatherLab/swarm-learning-hpe.git && cd swarm-learning-hpe
    ```
    ```sh
    sudo chmod 777 -R /opt/hpe
    ```

## Installing the License Server

!!! warning
    The license server must be provided by **only one** network member.

1. Go to [MY HPE SOFTWARE CENTER](https://myenterpriselicense.hpe.com/cwp-ui/auth/login).

2.  If you have the HPE Passport account, enter the credentials and **Sign In**. If you do not have it, create the HPE Passport Account and **Sign In**.

3. After signing in, click **Software** from the left pane.
   
4. Locate Search and select **Product Info** from the Search Type dropdown, and search for **Swarm Learning**. Search results list the available Swarm Learning products.
   
5. **"HPE Swarm Learning Community edition"** is the evaluation version that you need to download. Click **Action** and select **Product Details** to view the Swarm Learning product details.

6. Click **Installation** tab to view the **APLS** download link. Click the link here to view the APLS software downloads. Scroll down and view search results.
   
7. Download **AutoPass License Server**. To download, click Action and select **Get downloads**.
   
8. Click Download to copy the **APLS software** (apls-xx.xx.xx.zip) to your system.

9.  **Extract zip** file in Downloads.
    
10. **Execute setup scrip**t:
```sh
cd Downloads/apls-9.12/UNIX
chmod a+x setup.bin
sudo ./setup.bin
```

!!! info
    When it is **successfully executed** the following appears:
    ```sh
    Pre-Installation Summary
    ------------------------

    Please Review the Following Before Continuing:

    Product Name:
        HPE AutoPass License Server

    Install Folder:
        /opt/HP/HP AutoPass License Server

    Link Folder:
        /usr/bin

    Java VM Installation Folder:
        /opt/HP/HP AutoPass License Server/jre

    Data Folder Directory
        /var/opt/HP/HP AutoPass License Server

    Product Version
        9.12.0.0

    Disk Space Information (for Installation Target): 
        Required:      304.12 MegaBytes
        Available: 420,910.62 MegaBytes
    ```
    ```sh
    Congratulations HPE AutoPass License Server 9.12.0.0 has been successfully 
    installed to:

    /opt/HP/HP AutoPass License Server

    HPE AutoPass License Server GUI can be accessed at :
    https://<Host/IP address>:5814/autopass

    HPE AutoPass License Server Service Usage:
    hpLicenseServer {start|stop|restart|status}
    ```

11. **visit:** https://localhost:5000
!!! info 
    username: admin

    password: password

!!! bug "If service not working "
    ```sh
    cd "/opt/HP/HP AutoPass License Server/HP AutoPass License Server/HP AutoPass License Server/conf"
    sudo nano server.xml
    ```

    Search with Strg + W for “5814” and replace it with “5000” and save.

    ```sh
    cd "/opt/HP/HP AutoPass License Server/HP AutoPass License Server/HP AutoPass License Server/bin"
    sudo cp hpLicenseServer  /etc/init.d/hpLicenseServer
    sudo chmod 755 /etc/init.d/hpLicenseServer
    cd /etc/init.d
    sudo update-rc.d hpLicenseServer defaults 97 03
    service hpLicenseServer start
    service hpLicenseServer status
    ```
    
12.  In the APLS web GUI, go to **`License Management -> Install License`** and **note down the lock code**.

!!! info
    Lock Code = Serial Number

13. Navigate to the [MY HPE SOFTWARE CENTER](https://myenterpriselicense.hpe.com/cwp-ui/auth/login) home page. After signing in with your HPE Passport credentials and perform the following actions:
    
    Click **Software** (left pane) -> Under **Search** Select "Product Info" -> enter the string "Swarm Learning".
    
    Under the search results, For the product "HPE-SWARM-CMT 1.1.0"-> Click on **Action** -> **Get License**

 
2. Enter the lock code (Serial Number) you got from the **Install Licenses** page in the HPE Serial Number field and click **Activate**.
3. Once you activate the licenses, you will see the **Download Files** page.
4. Select and download the **keys and all the listed software files** (7 files).
5. Install and manage the Swarm Learning license:
    1. Open the APLS management console.
    2. Select **License Management** -> **Install License**.
    3. Select **Choose** file to upload the license file that you downloaded and click **Next**.
    4. Select the required feature IDs and click **Install Licenses**.






6. Download APLS container and run it using the following procedures.

    1.  Login to the HPE docker registry using your HPE Passport email id and password `hpe_eval`.

        ``` 
        docker login hub.myenterpriselicense.hpe.com -u <HPE-PASSPORT-EMAIL-ID> -p hpe_eval
        ```

    2.  Enable Docker content trust.

        ```
        export DOCKER_CONTENT_TRUST=1
        ```

    3.  Pull the image with a tag.

        ```
        docker pull hub.myenterpriselicense.hpe.com/hpe_eval/autopass/apls:9.14
        ```

    4.  Configure Data persistence.

        In order to retain configurations and installed licenses across containers, HPE recommends you to create a volume to persist the /hpe directory. This directory contains the following details:

        |Image Directory                 |Subdirectories    |Description                                                                                                                                                                                                |
        |--------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
        |/hpe|AutoPass/LicenseServer/data|..data/conf       |License server configuration directory. Contains database, logs and configuration files required to persist setup across containers transactions such as restarts, deletion or upgrades to new image tags. |
        |                                |..data/log        |                                                                                                                                                                                                           |
        |                                | ..data/database  |
 
        HPE recommends you to create the volume using the docker volume create command and assign a volume name such as `apls-volume`, as follows:

        ```
        docker volume create apls-volume
        ```

    5.  Create and run the APLS container.

    To run the APLS Docker container, user can use `docker cli` using the following instructions:

    **Docker CLI**

    ```
    docker run -d \
    --name apls \
    -v apls-volume:/hpe \
    -p 5814:5814 \
    --restart unless-stopped \
    hub.myenterpriselicense.hpe.com/hpe_eval/autopass/apls:9.14
    ```

    **NOTE:** In case the APLS container does not work, then user can choose to install APLS software using the APLS installer. User can select the AutoPass License Server \(APLS\) Installer link under 'Additional Notes' and download the [APLS](https://myenterpriselicense.hpe.com/cwp-ui/free-software/APLS) software. To install the APLS software on a host machine \(Linux or Windows\), see *AutoPass License Server User Guide*, which is part of the downloaded APLS software.

7.  From a browser, access the APLS management console using the URL `https://<localhost>:5814` on the host machine where you installed the license server. 

   The default user name is admin, and the password is password.

<blockquote>
    NOTE: These instructions assume that the host IP of license server is <localhost\> and the external port is 5814. Host IP is the IP of the system where the license server is running. Modify these values to match the actual IP and external port on your system.

</blockquote>
    
   If the web browser cannot connect to the APLS management console, check your network proxy settings and firewall policies. Consider techniques like port forwarding to work around firewall policies. If necessary, work with your network administrator to diagnose and resolve connectivity problems.

6. In the APLS web GUI, go to **License Management** -\> **Install License** and note down the lock code. 
   
   ![Lock code](https://github.com/HewlettPackard/swarm-learning/raw/master/docs/Install/GUID-A37C5798-B8B7-4B93-B786-A2682797AB37-high.png)

7.  Go to the Software Notification Message Receipt page and click **Access Your Products**.

    You will be navigated to the [MY HPE SOFTWARE CENTER](https://myenterpriselicense.hpe.com/cwp-ui/auth/login) home page. After signing in with your HPE Passport credentials, you will see the **Activate** page.

8.  Activate the license:

    1.  Select the number of licenses to activate and click **Next**.

        **NOTE:** You can select the number of licenses to be installed on the host machines. For example, if you have 5 licenses, you can install 2 on Windows, and 3 on Linux machines.

    2.  Designate yourself or for another user for activation. Click **Next**.

    3.  Enter the lock code you got from the **Install Licenses** page in the HPE Serial Number field and click **Activate**.

9. Once you activate the licenses, you will see the **Download Files** page. Select the keys and the software and download them. 

10. Install and manage the Swarm Learning license:
    
   1. Open the APLS management console. 
   2. Select **License Management** -\> **Install License**. 
   3. Select **Choose** file to upload the license file that you downloaded and click **Next**. 
   4. Select the required feature IDs and click **Install Licenses**.
  
![APLS License Management](https://github.com/HewlettPackard/swarm-learning/blob/master/docs/Install/GUID-979617B4-8568-4BB7-A536-9B1E304A86CA-high.png?raw=true)

!!! question
    Did you find a **bug** in the code or other **problems**? Then raise an issue in our Github repository: [https://github.com/KatherLab/swarm-learning-hpe/issues](https://github.com/KatherLab/swarm-learning-hpe/issues)

    In case of **problems or requests for improvement of the documentation**, please raise an issue at: [https://github.com/odelia-ai/odelia-ai.github.io/issues](https://github.com/odelia-ai/odelia-ai.github.io/issues)




  


