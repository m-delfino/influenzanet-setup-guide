
# Setting up GKE cluster and installing Infectieradar

This topic involves instructions on bringing up a cluster on GKE. This requisitions the hardware on which Infectieradar will run. The second part of this walkthrough focuses on running the scripts needed to get Infectieradar running on this cluster.

## Pre-requisites

Create a google account (Infectieradar) and use it to sign into GKE. Set up the required billing contacts and payment details.

Some knowledge of Kubernetes and Kubectl is useful for debugging errors. 

## Creating a cluster

1. Sign into https://cloud.google.com.
2. Navigate to the console.
3. Under the navigation menu -> Click on Kubernetes engine
4. Select the create new cluster option.
5. Give the cluster a suitable name, select the number of nodes as 2.
6. Select the node instances to be n1-standard1 (these are sufficient to get the platform started)
7. Add a persistant SSD of 50 gb
8. Leave the rest of the configurations as default and click create cluster

This should spawn up a new cluster with 2 nodes for us to install Infectieradar on.

## Installing Infectierdar

Once on the Kubenetes -> clusters page:
1. Click on the 3 dots next to the newly created cluster.
2. Select connect -> Run in cloud shell.
3. To install the platform we make use of the git repository [Cluster Management](https://github.com/InfectieradarBE/cluster-management)
4. Once connected run ``` git clone https://github.com/InfectieradarBE/cluster-management.git ```
5. Enter into the cloned folder by running ``` cd cluster-management ```
6. This repository contains a branch for each of the countries where the deployment is to take place. Refer to the readme in [Cluster Management](https://github.com/InfectieradarBE/cluster-management)
7. Switch to the relevant country branch by runnning ``` git checkout <country_name> ``` (ex: git checkout belgium)
8. Refer to section 2 of the readme in Cluster Management to perform the required configurations before installation.
9. Once ready, run the script install_start.sh by running ``` sh install_start.sh ```

Note: Sudo permissions may be required to run the install script.

The install script sets up the load balancer, docker containers for each of the services that are part of the platform, creates a mongoDB instance and also sets up the nginx ingress to route requests to the web app, participant or management api services respectively.

Note: In case scripts fail, navigate to see which deployments are failing. First refer to the [Rollback and Error handling section](https://github.com/influenzanet/infectieradar-setup-guide/blob/master/redeploying-changes/3-rollback-errors.md). If that doesn't help either run the start.sh script or go into cluster-management folder and re-create the deployment that is failing using the kubectl create command. The cheatlist of kubectl commands can be found at [Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/). 
