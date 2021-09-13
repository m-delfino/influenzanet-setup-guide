# Error Handling & Issue Resolution Guide

General guideline for handling bug reports or errors discovered in the system

## Roles

1. Support team: All personnel handling the supprot email addresses linked with the platform.

2. System Admin: Accounts with access to Mailgun logs (if used) as well as GKE environment.

## Types of Issues: 

1. Errors reported via email to the support email

2. Errors detected through GKE logs, and Mailgun logs/analytics.


## Resolution Procedure:

In case the error was reported via email, first follow the procedure laid out below: 

[Responsibilty - Support team]

1. Inform your platform administrator of the error reported by email.

2. Write back to inform that this error is being looked into. - Perhaps prepare a template auto response for this 


Once an immediate response is drafted by the support team, the system admin takes the following steps: 

1. First look at Mailgun logs (if used) to assess the problem.

    - Log into mailgun, go to Sending -> Logs
    
    - Filter the logs according to the email id of the user facing the problem (if support request received)
    
    - Goal is to find which emails are failing or have been delivered too late.
    
    - Some example causes - > Registration email delivered too late, Login code delivered too late, or Login code delivery failed etc.
    
    - You can attempt to resend failed emails here, or also can view the content of the email to further help out the participant. (You can extract the code or the registration link directly and send to the participant)
    
    - Or if there is a different issue that requires mailgun support, immediate create a support request with Mailgun.

2. To investigate GKE, log in with Influenzanet admin account into google cloud console.

    - Look for Kubernetes in the menu on the top left corner and click on clusters
    
    - Workloads gives you a description of the services that are currently running -> How many pods, which image version is being used, current limits on resources=> how much ram or cpu can a pod take (upper limit), etc.
    
    - To investigate mongodb
        
        - Connect to mongodb by following the steps mentioned in [mongo-connect-configure](../system-configuration/1-mongodb-config.md)
        
        - Now you should be inside the mongo shell
        
        - List the databases by running ```show dbs```
        
        - The following databases are of importance (we use belgium as an example instance here):
        
            - `belgium_users` - > contains collections of users in the system
            
            - `begium_studyDB` -> contains collections for each study in the platform and it’s participants, surveys and surveyResponses.

        - To use a database enter ```use db-name```. For eg: ```use belgium_users```
        
        - To find user count run ```db.users.count()``` after selecting the belgium_users database for use.
        
        - Once a database is selected, you can see what collections are present by running: ```show collections```
        
        - To find a user run: ```db.users.find({“account.accountID”: “<EMAIL-ID>”})```. Here “user” is a collection present within belgium_users. This should allow you find potential problems in that user’s account.
        
        - Some examples are: 
        
            - If accountConfirmedAt field is at Number(0) then the account is not confirmed yet and needs to be confirmed.
            
            - If account is confirmed, user may have initiated a login token that is now expired. To check this look for the field “expiresAt” within “verificationCode” to determine if the token has expired.
            
            - To turn off 2-FA run: db.users.updateOne({"account.accountID": "<email-id>"}, {$set: {"authType": ""}})
            - To reset the 2FA run: db.users.updateOne({"account.accountID": "<email-id>"}, {$set: {"authType": "2FA"}})
        
        - To find other information wrt to studies run : ```use belgium_studyDB``` (optional show collections)
        
        - To find participants in the influenzanet-be study, run: ```db['influenzanet-be_participants'].count()```
        
        - To find total responses collected so far, run: ```db['influenzanet-be_surveyResponses'].count()```
        
        - Once done exit the mongo shell by typing: exit
        
        - Type exit once again to return to the GKE terminal
    
    - Investigate and check if resource limits are causing issues with any of the services.
    
    - Look for log entries showing failures in services like participant-api, user-management or study-management.
    
    - Look for errors showing up in the logs for mongo.

3. If error cause could not be determined or if errors requires more detailed code fixes, contact Coneno for support requests and bug fixes.

**Next**: [Scaling Resources](../maintenance/3-resource-scaling.md)