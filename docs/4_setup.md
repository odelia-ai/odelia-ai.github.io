---
title: Setup
description: Nullam urna elit, malesuada eget finibus ut, ac tortor
icon: material/laptop
status: 
---

# Setup

!!! info 
    https://github.com/HewlettPackard/swarm-learning

## Installlation of HPE Swarm Learning

**Please replace the `<placeholder>` with the corresponding value**


- `<sentinel_ip>` = `172.24.4.67` It's the IP assigned by VPN server for Sentinal.

- `<license_ip>` = `172.24.4.73` It's the IP assigned by VPN server for License Server.

- `<host_index>` =  `swarm1`, `swarm2` or `swarm3`. It's the the name of the host.

- `<workspace_name>` = `odelia-breast-mri` It's the name of the workspace, where all the code is stored.
  

**Please only proceed to the next step by observing "... is done successfully" from the log**

1. `Prerequisite`: Runs scripts that check for required software and opens exposed ports.
    ```sh
    sh workspace/automate_scripts/automate.sh -a
    ```

2. Copy  VPN configuration file from `Data` Drive `odelia_summer_school/vpn/swarmX.ovpn` to `opt/hpe/swarm-learning-hpe/assets/openvpn_configs/good_access`

2. `Server setup`: Runs scripts that sets up the swarm learning environment on a server.
    ```sh
    sh workspace/automate_scripts/automate.sh -b -s <sentinel_ip> -d <host_index>
    ```
    - The script will ask for the HPE credentials. Please use the following credentials:
        - E-Mail:    
        - Password: 
  
    - The script will also ask for the VPN credentials. Please use the following credentials:
        | IP-Address  | Host Index  | vpn-account  | password |
        |---|---|---| --- |
        |

    - The Ip-Address should now be shown with:
        ```sh
        hostname -I
        ```


3. `Final setup`: Runs scripts that finalize the setup of the swarm learning environment. Only <> is required. The [-n num_peers] and [-e num_epochs] flags are optional.
    ```sh
    sh workspace/automate_scripts/automate.sh -c -w <workspace_name> -s <sentinel_ip> -d <host_index> -l <license_ip> [-n num_peers] [-e num_epochs]
    ```

!!! question
    Did you find a **bug** in the code or other **problems**? Then raise an issue in our Github repository: [https://github.com/KatherLab/swarm-learning-hpe/issues](https://github.com/KatherLab/swarm-learning-hpe/issues)

    In case of **problems or requests for improvement of the documentation**, please raise an issue at: [https://github.com/odelia-ai/odelia-ai.github.io/issues](https://github.com/odelia-ai/odelia-ai.github.io/issues)