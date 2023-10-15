---
title: Usage
description: Nullam urna elit, malesuada eget finibus ut, ac tortor
icon: material/chart-donut
status: new 
---

# Usage

### Data Preparation
1. Make sure you have downloaded Duke data.
    
2. Create the folder `WP1` and in it `test` and `train_val`
```bash
mkdir workspace/<workspace-name>/user/data-and-scratch/data/WP1
mkdir workspace/<workspace-name>/user/data-and-scratch/data/WP1/{test,train_val}
```
3. Search for your institution in the [Node list](#nodelist) and note the data series in the column "Data"
   
4. Copy the clinic table and slide table into WP1
```sh
cp workspace/<workspace-name>/user/data-and-scratch/data/{clinical_table,slide_table}.csv workspace/<workspace-name>/user/data-and-scratch/data/WP1
```
   
5. Copy the features from feature folder into `WP1/test` from 801 to 922
```sh
cp workspace/<workspace-name>/user/data-and-scratch/data/features_odelia_sub_imagenet/Breast_MRI_{801..922}_{right,left}.h5 workspace/<workspace-name>/user/data-and-scratch/data/WP1/test
```
   
6. Copy the features from feature folder with the order you noted into `WP1/train_val` from xxx to yyy
```sh
cp workspace/<workspace-name>/user/data-and-scratch/data/features_odelia_sub_imagenet/Breast_MRI_{<first_number>..<second_number>}_{right,left}.h5 workspace/<workspace-name>/user/data-and-scratch/data/WP1/train_val
```

### Running Swarm Learning Nodes
To run a Swarm Network node -> Swarm SWOP Node -> Swarm SWCI node. Please open a terminal for each of the nodes to run. Observe the following commands:
- To run a Swarm Network (or sentinel) node:
```sh
$ ./workspace/automate_scripts/launch_sl/run_sn.sh -s <sentinel_ip_address> -d <host_index>
```

- To run a Swarm SWOP node:
```sh
$ ./workspace/automate_scripts/launch_sl/run_swop.sh -w <workspace_name> -s <sentinel_ip_address>  -d <host_index>
```

- To run a Swarm SWCI node(SWCI node is used to generate training task runners, could be initiated by any host, but currently we suggest only the sentinel host is allowed to initiate):
```sh
$ ./workspace/automate_scripts/launch_sl/run_swci.sh -w <workspace_name> -s <sentinel_ip_address>  -d <host_index>
```

- To check the logs from training:
```sh
$ ./workspace/automate_scripts/launch_sl/check_latest_log.sh
```

- To stop the Swarm Learning nodes, --[node_type] is optional, if not specified, all the nodes will be stopped. Otherwise, could specify --sn, --swop for example.
```sh
$ ./workspace/swarm_learning_scripts/stop-swarm --[node_type]
```

- To view results, see logs under `workspace/<workspace_name>/user/data-and-scratch/scratch`

Please observe [Troubleshooting.md](Troubleshooting.md) section 10 for successful running logs of swop and sn nodes. The process will keep running and displaying logs so you will need to open a new terminal to run other commands.
* Typical run time can take 3 hours for experiments trained on the DUKE dataset with ResNet50-3D with three nodes involved.

