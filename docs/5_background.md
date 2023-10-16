---
title: Background
description: Swarm learning is a decentralized machine learning solution that uses edge computing and blockchain technology to enable peer-to-peer collaboration. It allows multiple collaborators to share data insights without sharing the data itself, protecting data privacy and security while allowing all contributors to benefit from collective learnings.
icon: material/bookmark
status: 
---

# Background

!!! info "Swarm Learning"
    Swarm learning is a decentralized machine learning solution that uses edge computing and blockchain technology to enable peer-to-peer collaboration. For this project, we are using **HPE Swarm Learning**, which was developed by **Hewlett Packard Enterprise**.

!!! tip "Swarm Learning Course"

    You can find a **free Swarm Leaning Course** explained in a generally understandable way under [https://learn.software.hpe.com/swarm-learning-essentials](https://learn.software.hpe.com/swarm-learning-essentials). 

## General

![Swarm Learning](https://support.hpe.com/hpesc/public/api/document/sd00001420en_us/GUID-739C1F67-AEA4-4D40-A26E-6E33BBA393FD-high.png?v=4)



### What is Swarm Learning?

Swarm learning is a **decentralized machine learning solution** that uses **edge computing** and **blockchain technology** to enable **peer-to-peer collaboration**. It allows multiple collaborators to **share data insights without sharing the data itself**, protecting **data privacy and security** while allowing all contributors to benefit from **collective learnings**.

### Why is swarm learning important?

As more data is collected and processed at the intelligent edge, its true value can only be realized by sharing it and turning it into a collective understanding. However, **sharing data** like this introduces **security risks** and in some cases is prohibited by **government regulations**. Because swarm learning **shares insights** rather than **source data**, the learnings derived from **protected data** can be **safely shared** across locations and even **across organizations**.

Swarm learning can play a vital role in **improving the accuracy of artificial intelligence (AI) models**. When organizations only have access to their **own data**, their AI models will evolve based solely on information about those individuals with whom the organization has previously or is currently engaged, **creating bias** in the models. With swarm learning, an organization can combine its proprietary data with the learnings from other organizations, **increasing accuracy and reducing bias**.

### What are the benefits of swarm learning?

Today, the huge volumes of data generated and collected at different edge locations creates a **challenge** for a traditional, **centralized machine learning approach**. These algorithms need data to be in a consolidated location, but moving large amounts of data from **multiple sources** to a single location introduces **security risks** and **latency concerns**.
Swarm learning’s decentralized approach allows data to be applied to AI models closer to the source, with **only the learnings being moved**. **Blockchain technology** enables multiple edge locations, collectively called a **swarm**, to share insights with one another in a **trusted manner** and prevents bad actors from gaining **unauthorized access** to the swarm.
This **decentralized approach** allows models to generate answers**more quickly** and organizations to have greater opportunities for **shared learning**, even outside their own four walls. At the same time, the **privacy of source data is protected**, because data movement is limited. This also **reduces data sprawl**, as data does not need to be duplicated to a core or central location.
By training models and harnessing insights at the edge, organizations can make decisions more quickly, where they are most relevant, resulting in better outcomes. Dataset sizes available to models can increase, making them more reliable and less prone to biases. At the same time, data governance and privacy are preserved.

!!! tip "Explanation Video"

    You can find an explanation video here: [HPE Swarm Learning Product Introduction](https://www.youtube.com/watch?v=ORujFJ1lVVw)

[^1]

[^1]: https://www.hpe.com/us/en/what-is/swarm-learning.html

## Swarm Learning Architecture

![Swarm Learning Architecture](https://github.com/HewlettPackard/swarm-learning/raw/master/docs/User/GUID-899B556F-D33F-42D1-8D0D-37F191715709-high.png)

Swarm Learning is made up of various components known as nodes, such as **Swarm Learning (SL)** nodes, **Swarm Network (SN)** nodes, **Swarm Learning Command Interface (SWCI)** nodes, and **Swarm Operator (SWOP)** nodes. Each node of SL is modularized and runs in a separate container.

- #### Swarm Learning (SL) node
  run the core of Swarm Learning. An SL node works in collaboration with all the other SL nodes in the network. It regularly shares its learnings with the other nodes and incorporates their insights. SL nodes act as an interface between the user model application and other Swarm Learning components. SL nodes take care of distributing and merging model weights in a secured way.

- #### Swarm Network (SN) node
  form the blockchain network. The current version of Swarm Learning uses an open-source version of Ethereum as the underlying blockchain platform. The SN nodes interact with each other using this blockchain platform to maintain and track progress. The SN nodes use this state and progress information to coordinate the working of the other swarm learning components.
    
    **Sentinel Node**
    is a special SN node. The Sentinel node is responsible for initializing the blockchain network. This is the first node to start.

- #### Swarm Learning Command Interface (SWCI) node
  is the command interface tool to the Swarm Learning framework. It is used to monitor the Swarm Learning framework. SWCI nodes can connect to any of the SN nodes in a given Swarm Learning framework to manage the framework.

- #### Swarm Operator (SWOP) node 
  is an agent that can manage Swarm Learning operations. SWOP is responsible to execute tasks that are assigned to it. A SWOP node can execute only one task at a time. SWOP helps in executing tasks such as starting and stopping Swarm runs, building and upgrading ML containers, and sharing models for training.

- #### License Server
  installs and manages the license that is required to run the Swarm Learning framework. The licenses are managed by the **AutoPass License Server (APLS)** that runs on a separate node.

!!! Blockchain
    Only metadata is written to the blockchain. The model itself is not stored in the blockchain.

[^2]

[^2]: https://support.hpe.com/hpesc/public/docDisplay?docId=sd00001420en_us&page=GUID-5924CFDA-389E-40ED-94C1-543FEDDFE872.html

## Swarm Learning Component Interactions

![Swarm Learning Component Interactions](https://support.hpe.com/hpesc/public/api/document/sd00001420en_us/GUID-19A4D5CE-204E-45CF-BFE9-B25EEB689AA0-high.png?v=4)

|Callout|Description|
|-------|-----------|
|1 \(<strong>SN Peer-to-Peer Port</strong>\)<br>| This port is used by each SN node to share blockchain internal state information with the other SN nodes. The default value of this port is 30303.<br> |
|2 \(<strong>SN API Port</strong>\)<br>| This API server is used by the SL nodes to send and receive state information from the SN node that they are registered with. It is also used by SWCI and SWOP nodes to manage and view the status of the Swarm Learning framework. The default value of this port is 30304.<br> |
|3 \(<strong>SL File Server Port</strong>\)<br>| This port is used by each SL node to run a file server. This file server is used to share insights learned from training the model with the other SL nodes in the network. The default value of this port is 30305.<br> |
|4 \(<strong>License Server API Port</strong>\)<br>| This port is used by the License Server node to run a REST-based API server and a management interface. The API server is used by the SN, SL, SWOP, and SWCI nodes to connect to the License Server and acquire licenses. The management interface is used by Swarm Learning platform administrators to connect to the License Server from browsers and administer licenses. The default value of this port is 5814.<br> |
|5 \(<strong>SWCI API server port</strong>\)<br>|This port is used by the SWCI node to optionally run a REST-based API service. This SWCI API service can be used to control and manage the Swarm Learning framework from a program by using the library provided in the wheels package. The default value of this port is 30306.|
|6 \(<strong>SL_REQUEST_CHANNEL and SL_RESPONSE_CHANNEL</strong>\)<br>|These named pipes (FIFO) are used between each pair of ML and SL nodes for exchanging the model parameters.|

[^3]

[^3]: https://support.hpe.com/hpesc/public/docDisplay?docId=sd00001420en_us&page=GUID-CE2496F4-22BD-468B-AD40-011E3F113E6E.html

## Swarm Learning Concepts

This section provides information about key Swarm Learning concepts. The subsequent section describes working of a Swarm Learning node, how to Swarm enable an ML algorithm, and interactions with SL, contexts, training contract, Taskrunner contract, and task

**SWCI** is the command interface tool to the Swarm Learning framework. It is used to view the status, control, and manage the Swarm Learning framework. **SWCI** manages the Swarm Learning framework using contexts and contracts. Learn more [here](https://support.hpe.com/hpesc/public/docDisplay?docId=sd00001420en_us&page=GUID-4AB7AD75-8467-4F3F-9392-23C694131D4F.html).

An **SWCI** context is a string identifier. It identifies an **SWCI** command environment and has artifacts (API server IP, port, environment variables, and versions) that are used to execute **SWCI** commands. **SWCI** can have only one active context at any given time even if multiple contexts are created.

Swarm Learning training **contracts** are used to control the swarm learning training process. It is an instance of Ethereum smart contract. It is deployed into the blockchain and registered into Swarm Learning Network using the `CREATE CONTRACT` command.

!!! note "Smart Contract"
     When a contract is created, it is permanent and cannot be deleted.

**SWOP** is the central component of the Taskrunner framework. It is packaged as a **SWOP** container. Taskrunner framework is a decentralized task management framework. Learn more [here](https://support.hpe.com/hpesc/public/docDisplay?docId=sd00001420en_us&page=GUID-5F7569ED-23C9-4091-A229-D7AF18E11A2E.html).

A **Task** is a well-defined unit of work, that can be assigned to a Taskrunner. **Tasks** are instantiated according to the schema specified in the Task Schema YAML file.

A **Taskrunner** is an instance of Ethereum smart contract used to coordinate execution of task by SWOP nodes.

[^4]

[^4]: https://support.hpe.com/hpesc/public/docDisplay?docId=sd00001420en_us&page=GUID-01AF4513-24A0-49DA-B345-E28A054E87B8.html

### Working of a Swarm Learning Node

1. SL node starts by **acquiring a digital identity**, which can be either user-provided CERTS or SPIRE certificates.
2. SL node **acquires a license** to run.
3. SL node **registers itself** with an SN node.
4. SL node **starts a file server** and **announces to the SN node that it is ready to run the training program**.
5. **Waits** for the User ML component to start the ML model training.

**SL node** works in collaboration with all the other **SL nodes** in the network. It regularly shares its learnings with the other nodes and incorporates their insights. Users can control the periodicity of this sharing by defining a **Synchronization Frequency**(from now on referred to as, sync frequency.) This frequency specifies the number of training batches after which the nodes share their learnings.

!!! note "Sync Frequency"
    Specifying a large value reduces the rate of synchronization and a small value increases it. Frequent synchronization slows down the training process while infrequent ones may reduce the accuracy of the final model. Therefore, the sync frequency must be treated as a hyperparameter and chosen with some caution.

Swarm Learning can automatically control the **sync frequency**, this feature is called **Adaptive Synchronization Frequency**. This feature judges the training progress by monitoring the mean loss. A reduction in the mean loss infers that the training is progressing well. As a response, it increases the sync frequency and enables more batches to run before sharing the learnings. This makes the training run faster. In contrast, when the loss does not improve, **Adaptive Sync frequency** reduces the sync frequency and synchronizes the models more frequently.

At the end of every **sync frequency**, when it is time to share the learning from the individual model, one of the **SL nodes** is designated as **“leader”**. This **leader** node collects the individual models from each peer node and merges them into a single model by combining parameters of all the individuals.

**Merge Methods** are one of the core operations in Swarm Learning that ensures learning is shared across **SL nodes**. Swarm Learning provides three different merge options via Swarm callback. The merge options are **'mean'**, **'coordmedian'** and **'geomedian'**. User can pass one of these options via `mergeMethod` parameter in the Swarm callback. This is an optional parameter for the callback. These options will consider the weightages of individual **SL nodes** as well. All nodes that have participated in the sync round will receive aggregated model parameters for the next sync round.

The **'mean'** option aggregates intermediate model parameters using a weighted mean method (also known as weighted federative averaging). This method sums up all the intermediate model parameters in an iterative way and at the end of the merge method, it divides with sum of weightages. This is the default merge method in Swarm.

The **'coordmedian'** option finds the weighted coordinate median of the intermediate model parameters. These parameters from all the nodes that are participating in the given merge round will get transformed and transposed before calculating the coordinate median. This merge method finds the 50th percentile and tries to converge along with the model parameters over the sync cycles.

The **'geomedian'** option finds the weighted geometric median of the intermediate model parameters. It uses Weiszfeld's algorithm which is an iterative method to calculate the geometric median. It starts with model parameters average as an initial estimate and iterate to find a better estimate. This process is repeated until the difference between new estimate and old estimate reaches to a threshold value.

**Leader Failure Detection and Recovery (LFDR)** feature enables **SL nodes** to continue Swarm training during merging process when an **SL leader node** fails. A new **SL leader node** is selected to continue the merging process. If the failed **SL leader node** comes back after the new **SL leader node** is in action, the failed **SL leader node** is treated as a normal **SL node** and contributes its learning to the swarm global model.

Each peer **SL node** then uses this merged model to start the next training batch. This process is coordinated by the SN network. The models are exchanged using the **Swarm Learning file server**.

A Swarm Learning ML program can specify a **Minimum Number of Peers** that are required to perform the synchronization. If the actual number of peers is less than this threshold value, the platform blocks the synchronization process until the required number of peers becomes available and reaches the synchronization point.

[^5]

[^5]: https://support.hpe.com/hpesc/public/docDisplay?docId=sd00001420en_us&page=GUID-80EB950E-DB95-469F-A3DF-E14BBD005486.html

### Adapting an ML Algorithm for Swarm Learning

![Adapting an ML Algorithm for Swarm Learning](https://support.hpe.com/hpesc/public/api/document/sd00001420en_us/GUID-49AC64FD-8D13-4772-9E71-97334988E182-high.png?v=4)

you can transform any **Keras** (with TensorFlow 2 backend) or **PyTorch** based ML program that has been written using **Python3** into a Swarm Learning ML program by making a few simple changes to the model training code by including the **SwarmCallback API** object. See the examples included with the Swarm Learning package for a sample code.

Your ML program algorithm can be any parametrized supervised learning model. It can be fully trainable model or a partially trainable model when using transfer learning. Learn more [here](https://support.hpe.com/hpesc/public/docDisplay?docId=sd00001420en_us&page=GUID-2D0165E8-A087-41E9-A160-EBCD66F39CB4.html).

[^6]

[^6]: https://support.hpe.com/hpesc/public/docDisplay?docId=sd00001420en_us&page=GUID-06917027-B029-400D-B9B0-9D2D5A9F29DB.html

## Whitepaper

You can find the whitepaper here: [Swarm Learning: Turn your distributed data into competitive edge](https://www.hpe.com/psnow/doc/a50000344enw)


*All information on this page has been gathered from various sources provided by Hewlett Packard Enterprise.*










