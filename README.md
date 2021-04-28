# infectieradar-getting-started
This is a brief guide to the different repositories present within the scope of Infectieradar. It also acts as the WIKI to the major actions - installation, configurations, updates and maintenance; that can be performed with Infectieradar.

**Note**: We refer to each repository that forms a part of Infectieradar with the term 'service'. While not all are services in the true sense of the word, it helps make our explanation easier.

First we take a look at the different parts of Infectieradar. Here, we use the term repository to specify a "Github repository" unless stated otherwise.

# Current Service Information
These repositories form the core services of the Infectieradar platform for Belgium

| Service        | Repository           | Function  |
| -------------- | -------------------- | ----------------:|
| participant-api      | api-gateway | Default backend API Gateway for web participants |
| management-api      | api-gateway | BAckend API Gateway for the researchers and admins of Infectieradar |
| study-service      | study-service | Backend service responsible for mananging studies and surveys in the system |
| user-management-service      | user-management-service | Backed service for the management of users in the system |
| email-client-service      | messaging-service | Backend service responsible for sending emails out of the system |
| message-scheduler      | messaging-service | Backend service that triggers events to send out emails periodically|
| messaging-service      | messaging-service | Backend service that manages processing of all messages in the system |
| logging-service      | logging-service | Backend service responsible for logging into DB information from all services |
| participant-webapp      | participant-webapp  | React based front-end of the Infectieradar platform |


# Supplementary Service Information
These repositories act as supporting services and scripts to help set up, perform configurations and/or deploy the Infectieradar platform.

| Service        | Repository           | Function  |
| -------------- | -------------------- | ----------------:|
| study-manager    | study-manager-app | Contains a React Application to help build surveys for researchers and export it as a JSON |
| study-manager-scripts    | study-manager-scripts | Scripts to create studies, upload surveys, upload email templates, and perform config changes on a running Infectieradar platform |
| cluster-management     | cluster-management | Scripts that help install Infectieradar on a GKE cluster, contains scripts to also stop all deployed services on a running cluster and restart them |


# Walkthrough Sections

The walkthrough sections are divided into three main categories:

1. Installation and Set-up
2. Initial system configurations
3. How to make changes & re-deploy
4. Maintenance Activities

Each section consists of topics that are to be followed in the provided order. For sections like maintenance activities you can jump directly to the area of concern.

## Installation and Set-up

In this section we explain the following tasks:

| Section Topic        | Instruction File  |
| -------------- | ----------------:|
| Starting from scratch - set up the required Github repositories    | [repository-creation](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/installation/1-repository-creation.md) |
| Set-up dockerhub and automated builds   | [dockerhub-setup](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/installation/2-dockerhub-setup.md) |
| Prepare a Google Kubernetes Environment and Install Infectieradar    | [gke-infectieradar-installation](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/installation/3-install-infectieradar-gke.md) |

## Initial System Configurations

This section is to be followed once the installation and set-up has been completed.

The topics covered are as follows:

| Section Topic        | Instruction File  |
| -------------- | ----------------:|
| How to access Mongo DB and create the initial DB configurations    | [mongo-connect-configure](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/system-configuration/1-mongodb-config.md) |
| Creating & uploading Studies and Surveys for the Infectieradar platform    | [create-study-surveys](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/system-configuration/2-create-study-surveys.md) |
| How to create and upload Email Templates to be sent out by the platform    | [create-upload-emails](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/system-configuration/3-email-setup.md) |
| Setting up mailing service configurations| [mailing-configuration](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/system-configuration/5-mailing-config.md) |
| Configuring layouts and pages in the web-ui| [web-ui-configuration](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/system-configuration/4-web-config.md) |

## How to make changes and re-deploy

Any configuration, content, design changes or bug-fixes will require the service where the change has occurred to be re-deployed for it show up in the live version of the platform. This sections covers all the topics involved in making such changes on the fly with a running version of the platform.

Topics covered here are:

| Section Topic        | Instruction File  |
| -------------- | ----------------:|
| Types of code changes  | [change-types](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/redeploying-changes/1-change-types.md) |
| Automated builds and re-deployment to Google cloud   | [build-and-redeploy](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/redeploying-changes/2-build-and-redeploy.md) |
| Roll-back in case of errors     | [rollback-errors](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/redeploying-changes/3-rollback-errors.md) |

## Maintenance Activities

Activites to be scheduled over the life-cycle of the Infectieradar-platform:

| Section Topic        | Instruction File  |
| -------------- | ----------------:|
| Maintenance Activity checklist | [checklist](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/maintenance/1-checklist.md) |
| Issue resoution | [issue-resolution](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/maintenance/2-issue-resolution.md) |
| Scaling resource allocations to cope with loads | [scaling-resources](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/maintenance/3-resource-scaling.md) |
