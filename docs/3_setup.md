---
title: Setup
description: Nullam urna elit, malesuada eget finibus ut, ac tortor
icon: material/laptop
status: new 
---

# Setup

!!! info 
    https://github.com/HewlettPackard/swarm-learning

### Prerequisites
#### Hardware recommendations
* 64 GB of RAM (32 GB is the absolute minimum)
* 16 CPU cores (8 is the absolute minimum)
* an NVIDIA GPU with 48 GB of RAM (24 is the  minimum)
* 8 TB of Storage (4 TB is the absolute minimum)
* We deliberately want to show that we can work with lightweight hardware like this. Here are three quotes for systems like this for less than 10k EUR (Lambda, Dell Precision, and Dell Alienware)
* Typical installation time can take 30 minutes to build up the necessary dependencies and another 1 hour to build up the environment for running demo experiments
  
#### Operating System
* Ubuntu 20.04 LTS
  * We have tested the Swarm Learning Environment on [Ubuntu 20.04 LTS, Ubuntu 22.04.2 LTS, Ubuntu 20.04.5 LTS] and they work fine. 
  *  Any experimental release of Ubuntu greater than LTS 20.04 MAY result in unsuccessful swop node running.
  * It also works on WSL2(Ubuntu 20.04.2 LTS) on Windows systems. WSL1 may have some issues with the docker service.

### Setting up the user and repository
1. Create a user named "swarm" and add it to the sudoers group.
Login with user "swarm".
```sh
$ sudo adduser swarm
$ sudo usermod -aG sudo swarm
$ sudo su - swarm
```
2. Run the following commands to set up the repository:

```sh
$ cd / && sudo mkdir opt/hpe && cd opt/hpe && sudo chmod 777 -R /opt/hpe
$ git clone https://github.com/KatherLab/swarm-learning-hpe.git && cd swarm-learning-hpe
```

3. Install cuda environment and nvidia drivers, as soon as you could see correct outputs of the following command you may proceed.
```sh
$ nvidia-smi
```
Please disable secure boot. On some systems, Secure Boot might prevent unsigned kernel modules (like NVIDIA's) from loading.
Check Loaded Kernel Modules:
- To see if the NVIDIA kernel module is loaded:
```sh
$ lsmod | grep nvidia
```
Review System Logs:
- Sometimes, system logs can provide insights into any issues with the GPU or driver:
```sh
$ dmesg | grep -i nvidia
```
Manually Load the NVIDIA Module:
- You can try manually loading the NVIDIA kernel module using the modprobe command:
```sh
$ sudo modprobe nvidia
```
Requirements and dependencies will be automatically installed by the script mentioned in the following section.

### Setting up the Swarm Learning Environment
**PLEASE REPLACE THE `<PLACEHOLDER>` WITH THE CORRESPONDING VALUE!**

`<sentinel_ip>` = `172.24.4.67` currently it's the IP assigned by VPN server for TUD host.

`<host_index>` = Your institute's name. For ODELIA project should be chosen from `TUD` `Ribera` `VHIO` `Radboud` `UKA` `Utrecht` `Mitera` `Cambridge` `Zurich`

`<workspace_name>` = The name of the workspace you want to work on. You can find available modules under `workspace/` folder. Each module corresonds to a radiology model. Currently we suggest to use `odelia-breast-mri` or `marogoto_mri` here.

**Please only proceed to the next step by observing "... is done successfully" from the log**

0. Optional: download preprocessed datasets. This will take a long time. Just run with either command, `get_dataset_gdown.sh` is recommended to run before you have done step 2, `get_dataset_scp.sh` is recommended to run after you have done step 2.
`get_dataset_gdown.sh` will download the dataset from Google Drive.
```sh
$ sh workspace/automate_scripts/sl_env_setup/get_dataset_gdown.sh
```
The [-s sentinel_ip] flag is only necessary for `get_dataset_scp.sh` The script will download the dataset from the sentinel node.
```sh
$ sh workspace/automate_scripts/sl_env_setup/get_dataset_scp.sh -s <sentinel_ip>
```
1. `Prerequisite`: Runs scripts that check for required software and open/exposed ports.
```sh
$ sh workspace/automate_scripts/automate.sh -a
```
2. `Server setup`: Runs scripts that set up the swarm learning environment on a server.
```sh
$ sh workspace/automate_scripts/automate.sh -b -s <sentinel_ip> -d <host_index>
```
3. `Final setup`: Runs scripts that finalize the setup of the swarm learning environment. Only <> is required. The [-n num_peers] and [-e num_epochs] flags are optional.
```sh
$ sh workspace/automate_scripts/automate.sh -c -w <workspace_name> -s <sentinel_ip> -d <host_index> [-n num_peers] [-e num_epochs]
```

Optional 5. Reconnect to VPN
In case your machine got restarted or lost the vpn connection. Here is the guide to reconnect: [VPN connect guide](https://support.goodaccess.com/configuration-guides/linux/linux-terminal)
The file.ovpn is the config file that TUD assigned to you.

If a problem is encountered, please observe this [README.md](workspace%2Fautomate_scripts%2FREADME.md) file for step-by-step setup. Specific instructions are given about how to run the commands.
All the processes are automated, so you can just run the above command and wait for the process to finish.

If any problem occurs, please first try to figure out which step is going wrong, try to google for solutions and find solution in [Troubleshooting.md](Troubleshooting.md). Then contact the maintainer of the Swarm Learning Environment and document the error in the Troubleshooting.md file.