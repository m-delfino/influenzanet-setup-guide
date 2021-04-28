# Build and Re-deploy changes.

Since automated builds have been configured in the [dockerhub-setup](https://github.com/influenzanet/infectieradar-setup-guide/blob/master/installation/2-dockerhub-setup.md), any changes pushed to the repositories will automatically create a built docker image. This image is also uploaded to the dockerhub repository. 

**NOTE**: Only the repositories where a change has occurred needs to be re-built. However in the case of API-gateway and messaging-service, these are built into 2 and 3 new images respectively since these are sub-divided as discussed in [dockerhub-setup](https://github.com/influenzanet/infectieradar-setup-guide/blob/master/installation/2-dockerhub-setup.md).

Since the build and deploy are automatically handled by Github and Dockerhub, the only remaining task is to link these newly created images to the installed Infectieradar platform running on Google Kubernetes Engine.

## Updating new image versions in GKE

Running the installation scripts in [gke-infectieradar-installation](https://github.com/influenzanet/infectieradar-setup-guide/blob/master/installation/3-install-infectieradar-gke.md) creates a deployment for each of the services of Infectieradar. These services are linked to a built image residing in Dockerhub.

To update this to the newly uploaded image, we can take an example of a change that occurs in the participant-webapp:
1. Once a code change is made, wait for Github to complete building and deploying the image to Dockerhub.
2. You can get the version number of this new build by signing into dockerhub, click on the repository (here the participant-webapp  repository), look for a recently uploaded image. Generally version numbers are marked according to the latest tags of their corresponding Github repositories.
3. Next sign into GKE, navigate to Kubernetes Engine, and click on workloads.
4. This contains a list of workloads (representing each deployment). Click on web-client in our example. Next click on edit. This should bring up a yaml file that describes the running version of web-client on GKE. The file should look like:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: "2021-03-12T14:33:45Z"
  generation: 42
  labels:
    app: web-client
  name: web-client
  namespace: belgium
  resourceVersion: "47793022"
  selfLink: /apis/apps/v1/namespaces/belgium/deployments/web-client
  uid: 94f3732f-38bc-4026-9724-bb2f0e5f53b9
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: web-client
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      annotations:
        linkerd.io/inject: disabled
      creationTimestamp: null
      labels:
        app: web-client
    spec:
      containers:
      - image: infectieradarbe/participant-webapp:v0.0.1
        imagePullPolicy: Always
        name: web-client
        ports:
        - containerPort: 3100
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2021-03-28T10:25:37Z"
    lastUpdateTime: "2021-03-28T10:25:37Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2021-03-12T14:33:45Z"
    lastUpdateTime: "2021-04-06T16:11:55Z"
    message: ReplicaSet "web-client-75db57ddd6" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 42
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
```
5. The only important line to edit here is the following ```- image: infectieradarbe/participant-webapp:v0.0.1```. This line describes the currently used image version from Dockerhub. Update this version to the newly built image version. (just replace the v0.0.1).
6. Once complete, click save.
7. This notifies Kubernetes to first try bringing up a new version of participant-webapp, and if successful then bring down the older version. The end-user doesn't notice the difference as long as the new version works as expected.

This process remains the same for changes to any of the other images (management-api, participant-api, study-service, etc). The image version needs to be changed in the relevant workload.

NOTE: There is no need to re-edit files of every workload, just the ones where a new image is created.
