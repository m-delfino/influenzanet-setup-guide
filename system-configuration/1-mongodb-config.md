# Connecting to MongoDB and intial configurations

## Pre-requisites

Ensure that mongo is running and installed through the installation script mentioned in [gke-infectieradar-installation](https://github.com/influenzanet/infectieradar-setup-guide/blob/main/installation/3-install-infectieradar-gke.md). 

Note: This also assumes that you have created the required entries for Mongo usernames and passwords in the guide in [Cluster Management](https://github.com/InfectieradarBE/cluster-management)

## Connecting to Mongo

1. Connect to the Kubernetes cluster by logging in to the google cloud platform.
2. Once connected, we need to find the pod id where mongo is running. Do this by running ```kubectl get pods -n belgium```
3. This returns a list of active pods, copy the id of the one with mongo in its name. Usually something similar to ```mongo-77cf57567c-c8nkp```
4. Use this enter into the mongo pod by running the command ``` kubectl exec -it <copied-mongo-id> --namespace=belgium -c mongo bash ```
5. This should bring up a shell where you need to enter ``` mongo -u <username> -p <password> ```
6. That should being you into the mongo shel where you can look up databases, collections, entries, etc.

Note: Only the system administrator has access to this.

## Initial Mongo configrations

With the web-app running create a new user (who is to be admin) by registering at the User Interface.

After connecting to mongoDB for the first time after installation a few entries need to be put in manually. These are as follows:

1. Enter ``` show dbs``` to list all the databases in the system.
2. Select the global-infos db by running ``` use global-infos ```.
3. Check the collections within global-infos by running ``` show collections ```
4. If the collections instances is not present, create it by running ``` db.createCollection('instances') ```.
5. Insert one row with your instance name (belgium in this case) by running ``` db.instances.insert({"instanceID": "belgium"})```
6. Switch to the user db by running ``` use belgium_users ```
7. Find and update the email used to initially create a new user at the web-app by running ```db.users.updateOne({"account.accountID": "<email-id>"}, {$set : {"roles": ["PARTICIPANT", "ADMIN"]} }) ```
8. this gives admin rights to the user in question.

Once this is complete, your mongo is ready for use.