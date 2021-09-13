# Setting up GKE cluster and installing Influenzanet

This topic involves instructions on bringing up a cluster on [GKE](https://cloud.google.com/kubernetes-engine). This requisitions the hardware on which the services making up the Influenzanet platform will run. The second part of this walkthrough will focus on running the scripts needed to get Influenzanet services running on this cluster, using the docker images built in the previous configuration step [Dockerhub Setup](../installation/2-dockerhub-setup.md).

## Pre-requisites

Create a google account and use it to sign into GKE. Set up the required billing contacts and payment details.

Some knowledge of Kubernetes and ```kubectl``` is useful for debugging errors. 

## Creating a cluster

1. Sign into https://cloud.google.com.
2. Navigate to the console.
3. Under the navigation menu -> Click on Kubernetes engine
4. Select the create new cluster option.
5. Give the cluster a suitable name, select the number of nodes as 2.
6. Select the node instances to be n1-standard1 (these are sufficient to get the platform started)
7. Leave the rest of the configurations as default and click create cluster

This should spawn up a new cluster with 2 nodes for us to install Influenzanet on.

## Installing Influenzanet

Once on the Kubernetes -> clusters page:
1. Click on the 3 dots next to the newly created cluster.
2. Select connect -> Run in cloud shell.
3. To install the platform we make use of the git repository [cluster management](https://github.com/influenzanet/cluster-management)
4. Once connected run ```git clone https://github.com/influenzanet/cluster-management.git```
5. Enter into the cloned folder by running ```cd cluster-management```
6. This repository requires you to configure a ```values.yaml``` file to provide details about your deployment. Refer to the Readme.md in [cluster management](https://github.com/influenzanet/cluster-management) for detailed instructions
7. Once ready, run the script install_start.sh by running ```sh install_start.sh```

The install script sets up the load balancer, docker containers for each of the services that are part of the platform, creates a mongoDB instance and also sets up the nginx ingress to route requests to the web app, participant or management api services respectively.

**NOTE**: In case the script fails, you'll have to navigate the logs and see which deployments are failing. First refer to the [Rollback and Error handling section](../redeploying-changes/3-rollback-errors.md). If that doesn't help, either run the stop.sh script and then run the start.sh script again or go into cluster-management folder and re-create the deployment that is failing using the ```kubectl create``` command. The cheat sheet for the ```kubectl``` command can be found at [Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/).

**Next**: [MongoDB Setup](../system-configuration/1-mongodb-config.md)
