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
    The license server must be provided by **only one** network member (Sentinel Node).

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
cd Downloads/apls-X.x/UNIX
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

!!! bug "If the service is not working "
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
![Lock code](https://github.com/HewlettPackard/swarm-learning/raw/master/docs/Install/GUID-A37C5798-B8B7-4B93-B786-A2682797AB37-high.png)
13. Navigate to the [MY HPE SOFTWARE CENTER](https://myenterpriselicense.hpe.com/cwp-ui/auth/login) home page. After signing in with your HPE Passport credentials and perform the following actions:
    
    Click **Software** (left pane) -> Under **Search** Select "Product Info" -> enter the string "Swarm Learning".
    
    Under the search results, For the product "HPE-SWARM-CMT x.x.x"-> Click on **Action** -> **Get License**

 
13. Enter the lock code (Serial Number) you got from the **Install Licenses** page in the HPE Serial Number field and click **Activate**.
14. Once you activate the licenses, you will see the **Download Files** page.
15. Select and download the **keys and all the listed software files** (7 files).
16. Install and manage the Swarm Learning license:
    1. Open the APLS management console.
    2. Select **License Management** -> **Install License**.
    3. Select **Choose** file to upload the license file that you downloaded and click **Next**.
    4. Select the required feature IDs and click **Install Licenses**.
   
![APLS License Management](https://github.com/HewlettPackard/swarm-learning/blob/master/docs/Install/GUID-979617B4-8568-4BB7-A536-9B1E304A86CA-high.png?raw=true)

!!! question "Bugs and Problems"
    Did you find a **bug** in the code or other **problems**? Then raise an issue in our Github repository: [https://github.com/KatherLab/swarm-learning-hpe/issues](https://github.com/KatherLab/swarm-learning-hpe/issues)

    In case of **problems or requests for improvement of the documentation**, please raise an issue at: [https://github.com/odelia-ai/odelia-ai.github.io/issues](https://github.com/odelia-ai/odelia-ai.github.io/issues)




  


