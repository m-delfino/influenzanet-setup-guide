# Scaling resources on GKE

This section deals with some scenarios where you may need to adjust resources available to the cluster on GKE. In addition it may be needed to increase or decrease number of replica's of each workload running on the cluster.

## Increasing or decreasing number of nodes

In the default deployment, there are 2 nodes (or machines) present in the Google Kubernetes Cluster. The cluster also contains configurations to automatically scale up or down hardware. This can be adjusted by performing the following steps:

- Log into GKE, and navigate to the clusters page.

- Click on the cluster being used, to open up the details of the cluster.

- Select the nodes tab to display the pools of nodes being used.

- Select the default-pool and click edit.

    - Here you can reconfigure what the current number of nodes in the system are.
    
    - You can set a minimum and maximum number of nodes that can be requested automatically based on demand.
    
    - Keep in mind, that new nodes are going to be of the same type of machine (eg: n1-standard-1) as previously picked while creating the cluster.
    
    - If you want to add a different machine type, add a new node-pool to list of machines.
    
    - While scaling down resources, be aware that default system services run by Google (such as Kubernetes system workloads) also require some amount of CPU and Memory.

## Limiting resources consumed by a workload

There may be some cases where one workloads consumes more resources than it needs to function. It may be advisable to set resource limits of each workload. This can be done by:

- Navigating to workloads and selecting the deployment to be limited (for eg: Mongo)

- Edit the yaml file and add the following section within the container tag:

    ```
    resources:
        requests:
            cpu: 30m
            memory: 150Mi
        limits:
            cpu: 150m
            memory: 300Mi
    ```

- You can reconfigure the values here to suit your needs.

## Scaling up replicas of a workload

This involves the creation of multiple replica's of the same pod in a deployment. This means that you may for example have two instances of the participant-webapp to split the load and resource consumption between the two.

Advantages of doing this is that you have improved availabilty to end users and better schedulability of a pod within the nodes requisitioned for the cluster.

To scale up or down replica sets:

- Navigate to the workloads page and select the workload you wish to scale.

- Select the actions dropdown and select scale.

- THis queries the number of instances you wish to create.

- Enter a suitable number, which can be more (up-scale) or less (down-scale) than what the deployment previously ran on.

**NOTE**: The system is designed to handle multiple instances of all workloads without causing data inconsistencies. The only bottleneck here is the mongodb which always has only a single instance. [Due to the GKE limitations on Many to One write capabilities on persistent disk]