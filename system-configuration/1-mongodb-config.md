# Connecting to MongoDB and initial configurations

## Pre-requisites

Ensure that mongo is running and installed through the installation script mentioned in [gke-influenzanet-installation](../installation/3-install-influenzanet-gke.md). 

This also assumes that you have created the required entries for Mongo usernames and passwords described in [cluster management](https://github.com/influenzanet/cluster-management)

## Connecting to Mongo

1. Connect to the Kubernetes cluster by logging in to the google cloud platform.

2. Once connected, we need to find the pod id where mongo is running. We use ```belgium``` namespace as an example, please replace it with your namespace. Do this by running ```kubectl get pods -n belgium```

3. This returns a list of active pods, copy the id of the one with mongo in its name. Usually something similar to ```mongo-77cf57567c-c8nkp```

4. Use this enter into the mongo pod by running the command ```kubectl exec -it <copied-mongo-id> --namespace=belgium -c mongo bash```

5. This should bring up a shell where you need to enter ```mongo -u <username> -p <password>```

6. That should bring you into the mongo shell where you can look up databases, collections, entries, etc.

**NOTE**: Only administrators of the cluster have access to this.

## Initial Mongo configurations

After connecting to MongoDB for the first time after installation a few entries need to be put in manually. These are as follows:

1. Enter ```show dbs``` to list all the databases in the system.
2. Select the global-Infos db by running ```use global-infos```.
3. Check the collections within global-infos by running ```show collections```
4. If the collections instances is not present, create it by running ```db.createCollection('instances')```.
5. Insert one row with your instance name (belgium in this case) by running ```db.instances.insert({"instanceID": "belgium"})```

Using the web application front-end, create a new user (who is to be promoted admin) by following the registration process into the website until you are asked to verify your email. Then switch back to the mongo shell and:

1. Switch to the user DB by running ```use belgium_users```
2. Find the user created by the web-app by running:

    ```javascript
    admin = db.users.findOne({"account.accountID": "<email-id>"})
    ```
    
3. Modify and update the user:
   
   ```javascript
   admin.account.accountConfirmedAt = NumberLong((new Date()).getTime());
   admin.roles = ["PARTICIPANT", "ADMIN"];
   admin.account.authType= "";
   db.users.replaceOne({"account.accountID": "<email-id>"}, admin);
   ```

This verifies and gives admin rights to the user in question, also disabling two factor authentication.

Once this is complete, your mongo is ready to use and you can proceed to the system configuration using the admin user credentials.

**Next**: [Creating a Study](../system-configuration/2-create-study-surveys.md)