# influenzanet-setup-guide

This is a brief guide to the different repositories present within the scope of Influenzanet. It also acts as the WIKI to the major actions - installation, configurations, updates and maintenance; that can be performed with Influenzanet.

**Note**: We refer to each repository that forms a part of the Influenzanet platform with the term 'service'. Even though there is not a one to one correspondence between repositories and services, it helps make our explanation easier.

First we take a look at the different parts of Influenzanet. Here, we use the term repository to specify a "Github repository" unless stated otherwise.

## Platform services

These repositories form the core services of the Influenzanet platform.

| Service        | Repository           | Function  |
| -------------- | -------------------- | ----------------:|
| participant-api      | api-gateway | Default backend API Gateway for web participants |
| management-api      | api-gateway | Backend API Gateway for the researchers and admins of Influenzanet |
| study-service      | study-service | Backend service responsible for managing studies and surveys in the system |
| user-management-service      | user-management-service | Backed service for the management of users in the system |
| email-client-service      | messaging-service | Backend service responsible for sending emails out of the system |
| message-scheduler      | messaging-service | Backend service that triggers events to send out emails periodically|
| messaging-service      | messaging-service | Backend service that manages processing of all messages in the system |
| logging-service      | logging-service | Backend service responsible for logging into DB information from all services |
| participant-webapp      | participant-webapp  | React based front-end of the Influenzanet platform |

## Supplementary repositories

These repositories act as supporting applications and scripts to help set up, perform configurations and/or deploy the Influenzanet platform.

| Repository           | Function  |
| -------------------- | ----------------:|
| study-manager-app | Contains a React Application to help build surveys for researchers and export it as a JSON |
| admin-scripts | Scripts to create studies, upload surveys, upload email templates, and perform config changes on a running Influenzanet platform (*private repository*) |
| cluster-management | Scripts that help install Influenzanet on a GKE cluster, contains scripts to also stop all deployed services on a running cluster and restart them |

## Walkthrough Sections

The walkthrough sections are divided into three main categories:

1. [Installation and Set-up](#1-installation-and-set-up)
2. [Initial system configurations](#2-initial-system-configurations)
3. [How to make changes & re-deploy](#3-how-to-make-changes-and-re-deploy)
4. [Maintenance Activities](#4-maintenance-activities)

Each section consists of topics that are to be followed in the provided order. For sections like maintenance activities you can jump directly to the area of concern.

### 1. Installation and Set-up

In this section we explain the following tasks:

| Section Topic        | Instruction File  |
| -------------- | ----------------:|
| Starting from scratch - set up the required Github repositories    | [repository-creation](installation/1-repository-creation.md) |
| Set-up dockerhub and image builds   | [dockerhub-setup](installation/2-dockerhub-setup.md) |
| Prepare a Google Kubernetes Environment and Install Influenzanet    | [gke-influenzanet-installation](installation/3-install-influenzanet-gke.md) |

### 2. Initial System Configurations

This section is to be followed once the installation and set-up has been completed. The topics covered are as follows:

| Section Topic        | Instruction File  |
| -------------- | ----------------:|
| How to access Mongo DB and create the initial DB configurations    | [mongo-connect-configure](system-configuration/1-mongodb-config.md) |
| Creating & uploading Studies and Surveys for the Influenzanet platform    | [create-study-surveys](system-configuration/2-create-study-surveys.md) |
| How to create and upload Email Templates to be sent out by the platform    | [create-upload-emails](system-configuration/3-email-setup.md) |
| Setting up mailing service configurations| [mailing-configuration](system-configuration/5-mailing-config.md) |
| Configuring layouts and pages in the web-ui| [web-ui-configuration](system-configuration/4-web-config.md) |

### 3. How to make changes and re-deploy

Any configuration, content, design changes or bug-fixes will require the service where the change has occurred to be re-deployed for it show up in the live version of the platform. This sections covers all the topics involved in making such changes on the fly with a running version of the platform.

Topics covered here are:

| Section Topic        | Instruction File  |
| -------------- | ----------------:|
| Types of code changes  | [change-types](redeploying-changes/1-change-types.md) |
| Automated builds and re-deployment to Google cloud   | [build-and-redeploy](redeploying-changes/2-build-and-redeploy.md) |
| Roll-back in case of errors     | [rollback-errors](redeploying-changes/3-rollback-errors.md) |

### 4. Maintenance Activities

Activities to be scheduled over the life-cycle of the Influenzanet-platform:

| Section Topic        | Instruction File  |
| -------------- | ----------------:|
| Maintenance Activity checklist | [checklist](maintenance/1-checklist.md) |
| Issue resoution | [issue-resolution](maintenance/2-issue-resolution.md) |
| Scaling resource allocations to cope with loads | [scaling-resources](maintenance/3-resource-scaling.md) |
