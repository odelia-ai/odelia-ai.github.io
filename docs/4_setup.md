---
title: Setup
description: Step-by-step installation guide for HPE Swarm Learning after meeting all requirements.
icon: material/laptop
status: 
---

# Setup

!!! info "Please replace the `<placeholder>` with the corresponding value:"
    - **`<workspace_name>`**: Name of the folder in `/opt/hpe/swarm-learning-hpe/workspace` where the configuration files and model code is stored. e.g.: `odelia-breast-mri`
    - **`<sentinel_ip>`**: IP address of the sentinel host. The sentinel host is the initiator of the network and the operator of the license server in our case. e.g.: `172.24.4.67`
    - **`<host_index>`**: Name of the institution or partner participating in the training e.g.: `TUD`
  
## Installlation of HPE Swarm Learning

**Please only proceed to the next step by observing "... is done successfully" from the log.**

!!! warning "/opt/hpe/swarm-learning-hpe"
    Please make sure you are in `/opt/hpe/swarm-learning-hpe` before you start the installation. You can get there with `cd /opt/hpe/swarm-learning-hpe `.

1. **Prerequisite**: Runs scripts that check for required software and opens exposed ports.
    ```sh
    sh workspace/automate_scripts/automate.sh -a
    ```

2. **Server setup**: Runs scripts that sets up the swarm learning environment on a server.
    ```sh
    sh workspace/automate_scripts/automate.sh -b -s <sentinel_ip> -d <host_index>
    ```

!!! info "Credentials"
    - The script will ask for the HPE credentials. Please use your [HPE Account](https://auth.hpe.com/hpe/cf/) credentials.
    - The script will also ask for the VPN credentials. We use GoodAccess VPN. ODELIA consortium members get their credentials through the contact persons at TU Dresden. External users are welcome to open an issue on Github if they have any questions.

3. **Final setup**: Runs scripts that finalize the setup of the swarm learning environment. 
    ```sh
    sh workspace/automate_scripts/automate.sh -c -w <workspace_name> -s <sentinel_ip> -d <host_index> -l <license_ip> [-n num_peers] [-e num_epochs]
    ```
!!! info "Optional flags"
    - **`-n num_peers`**: Number of peers to be added to the network. e.g.: `3`
    - **`-e num_epochs`**: Number of epochs to be trained. e.g.: `10`

!!! question "Bugs and Problems"
    Did you find a **bug** in the code or other **problems**? Then raise an issue in our Github repository: [https://github.com/KatherLab/swarm-learning-hpe/issues](https://github.com/KatherLab/swarm-learning-hpe/issues)

    In case of **problems or requests for improvement of the documentation**, please raise an issue at: [https://github.com/odelia-ai/odelia-ai.github.io/issues](https://github.com/odelia-ai/odelia-ai.github.io/issues)

## Upgrade the Swarm Learning Environment

1. If you already have the old version of Swarm learning installed. Run the following command to upgrade the Swarm Learning Environment from 1.x.x to 2.x.x
```sh
$ sh workspace/automate_scripts/server_setup/cleanup_old_sl.sh
```
Then proceed to 1. `Prerequisite` in the installation guide.
