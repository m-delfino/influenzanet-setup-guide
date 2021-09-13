# Rollback and error handling

## Scenario 1 - On redeploy, the new image fails to start

In case the workload fails to bring up a new instance of the new image. There are a couple of steps to be followed:

1. The older version is still in use, so end users still see the older version. This gives us some time to explore what the issue is.

2. In general navigate to the workload, and look for errors. Also look for errors in the logs section.

3. After navigating to workloads, scroll down to see the managed pods, click the pod that is failing (newly created one for the new image version). You may find additional details here, or in the logs sections of this pod.

4. Generally errors occur due to version mismatch (no such image exists in Dockerhub), invalid pull rights (this can happen if the dockerhub repo is configured as private instead of public). Additionally if the machines in the cluster do not have sufficient memory or CPU, this can also lead to a failure in creating the new pod.

5. Try resolving these issues, and wait for the errors to be resolved. 

6. In case the errors cannot be resolved, edit the yaml to revert to the older version of the image. Delete the failing pod and wait for the error to be resolved.

7. Delete both pods and wait for the older version to be re-created if the above does not work.

## Scenario 2: New version successfully deployed but has errors in runtime.

This is a scenario where errors that haven't been found during testing have shown up after a new version has been successfully deployed. In this case the resolution to be followed is as follows:

1. The older version is no longer running, so all participants are now exposed to the new image with the errors and bugs.

2. To revert to the older image, navigate to Workloads section of Kubernetes in GKE. 

3. Select the workload whose image version needs to be reverted.

4. Click edit and replace the image version to the older version and click save.

5. This should successfully bring up the older bug-free version.

## Scenario 3: Multiple fail-overs and configurations errors

This is the worst case scenario, where a fresh deployment of all services is needed. 

There are two versions of this available: 

1. Re-create only the influenzanet-services:

    1.1. Copy the contents of the PersistentVolume (mongo) and back them up else where. Navigate to storage in Kubernetes on GKE to perform this task.
    
    1.2. Once the data is backed up, connect to the cluster and open the terminal.
    
    1.3. cd into the cluster-management folder, and run the script ```sh  stop.sh```
    
    1.4. This removes all the services, secrets and configurations in the system.
    
    1.5. Re-create the configurations (if needed) and run ```sh start.sh ``` to reinstall the workloads.
    
    1.6. This process maintains the DB as before, so the data back up is only a precautionary measure

2. Completely re-install the system:

    1.1. Copy the contents of the PersistentVolume (mongo) and back them up else where if you didn't already. Navigate to storage in Kubernetes on GKE to perform this task.
    
    1.2. Navigate to clusters, and delete the cluster present.
    
    1.3. Re-do the process of requisitioning a cluster.
    
    1.4. Connect to the cluster and re-run the ```sh install_start.sh``` script.
    
    1.5. This will re-create not just the Influenzanet workloads, but also the certificate manager, nginx ingress controller and recreates all the persisitant volume claims.
    
    1.6. Navigate to storage and restore the backed up version to retain the database entries.
    
**Next**: [Maintenance Activity checklist](../maintenance/1-checklist.md)
