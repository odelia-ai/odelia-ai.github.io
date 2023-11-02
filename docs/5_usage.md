---
title: Usage
description: A step by step guide to starting and stopping Swarm Learning after setup.
icon: material/chart-donut
status: 
---

# Usage

!!! info "Please replace the `<placeholder>` with the corresponding value:"
    - **`<workspace_name>`**: Name of the folder in `/opt/hpe/swarm-learning-hpe/workspace`` where the configuration files and model code is stored. e.g.: `odelia-breast-mri`
    - **`<sentinel_ip>`**: IP address of the sentinel host. The sentinel host is the initiator of the network and the operator of the license server in our case. e.g.: `172.24.4.67``
    - **`<host_index>`**: Name of the institution or partner participating in the training e.g.: `TUD`

## Data Preparation

**Place the data in the folder before you start training!**

1. Create the folder structure for storing the data as follows:
    ```sh
    cd /opt/hpe/swarm-learning-hpe/workspace/<workspace_name>
    ```
    ```sh
    mkdir user && mkdir user/data-and-scratch && mkdir user/data-and-scratch/data && mkdir user/data-and-scratch/scratch && chmod 777 -R /opt/hpe
    ```
    ```sh
    cd /opt/hpe/swarm-learning-hpe
    ```

2. Copy your Data to `opt/hpe/swarm-learning-hpe/workspace/<workspace_name>/user/data-and-scratch/data`

## Running Swarm Learning Nodes

!!! warning
    Enter root mode:
    ```sh
    sudo su
    ```

### 1. Run a Swarm Network (or Sentinel) node:
!!! info "SN node"
    SN nodes form the blockchain network. The current version of Swarm Learning uses an open-source version of Ethereum as the underlying blockchain platform. The SN nodes interact with each other using this blockchain platform to maintain and track progress. The SN nodes use this state and progress information to co-ordinate the working of the other swarm learning components. **Sentinel Node** is a special SN node. The Sentinel node is responsible for initializing the blockchain network. This is the first node to start.
    >NOTE: Only metadata is written to the blockchain. The model itself is not stored in the blockchain.

```sh
./workspace/automate_scripts/launch_sl/run_sn.sh -s <sentinel_ip> -d <host_index>
```

### 2. Run a Swarm SWOP (Swarm Operator) node:
!!! info "SWOP"
    SWOP is an agent that can manage Swarm Learning operations. SWOP is responsible to execute tasks that are assigned to it. A SWOP node can execute only one task at a time. SWOP helps in executing tasks such as starting and stopping Swarm runs, building and upgrading ML containers, and sharing models for training.

```sh
./workspace/automate_scripts/launch_sl/run_swop.sh -w <workspace_name> -s <sentinel_ip>  -d <host_index>
```

### 3. Run a Swarm SWCI 
!!! info "SWCI"
    Swarm Learning Command Line Interface (SWCI) is the command interface tool to the Swarm Learning framework. It is used to view the status, control, and manage the Swarm Learning framework. SWCI manages the Swarm Learning framework using contexts and contracts.

!!! warning 
    SWCI node is used to generate training task runners, could be initiated by any host, but currently we suggest **ONLY THE SENTINEL HOST IS ALLOWED TO INITIATE**

```sh
./workspace/automate_scripts/launch_sl/run_swci.sh -w <workspace_name> -s <sentinel_ip>  -d <host_index>
```

## Results

!!! tip "Check the logs from training"
    ```sh
    ./workspace/automate_scripts/launch_sl/check_latest_log.sh
    ```

View results under `workspace/<workspace_name>/user/data-and-scratch/scratch`

## Stop Swarm Learning

Stop and remove all Swarm Learning containers and volumes that are no longer needed with: 
```sh
./workspace/swarm_learning_scripts/stop-swarm --[node_type]
```
!!! info
    --[node_type] is optional, if not specified, all the nodes will be stopped. Otherwise, specify --sn, --swop, --swci, --sl.

!!! tip "Manually Remove containers and volumes"
    1. List all Docker containers:
    ```sh
    docker ps -a
    ```
    2. Remove all containers listed with docker ps -a
    ```sh
    docker rm <container-id-1> <container-id-2> <container-id-n> # Remove all listed with docker ps -a
    ```
    3. List all Docker volumes:
    ```sh
    docker volume ls
    ```
    4. Remove all volumes **except sl-cli-lib**
    ```sh
    docker volume rm <volume-id>
    ```

!!! question "Bugs and Problems"
    Did you find a **bug** in the code or other **problems**? Then raise an issue in our Github repository: [https://github.com/KatherLab/swarm-learning-hpe/issues](https://github.com/KatherLab/swarm-learning-hpe/issues)

    In case of **problems or requests for improvement of the documentation**, please raise an issue at: [https://github.com/odelia-ai/odelia-ai.github.io/issues](https://github.com/odelia-ai/odelia-ai.github.io/issues)