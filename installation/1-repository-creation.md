
# Creating the repositories

  

To begin, it is advisable to set up an institution account on Github ideally with the country of deployment in mind. For example: InfectieradarBE, InfectieradarIT. 

##  Influenzanet

The major components and services of the Infectieradar platform are centrally hosted under the [Influenzanet](https://github.com/influenzanet) collection of respositories. The relevant respositories that we need are indicated in two tables in the central [Readme](https://github.com/influenzanet/infectieradar-setup-guide/blob/master/README.md)*. For the deployment of a specific country, we will be creating forks of these repositories and making changes to these forks. Periodically we will also be creating pull requests to the central repositories n Influenzanet to maintain synchronisation and to regularly take in updates.
 
\* Look for the name listed under the column "Repository"

## Creating and managing repositories

### Setting up the repositories

For each of the relavant services listed in the table in the central [Readme](https://github.com/influenzanet/infectieradar-setup-guide/README.md) navigate to the repository hosted at Influenzanet. Make sure you are logged into the institution account you created on Github. Fork each of the respositories in the table into your institutions personal github account. Once completed you should have 9 forked repositories (namely: api-gateway, study-service, user-management-service, messaging-service, logging-service, participant-webapp, study-manager-app, study-manager-scripts and cluster-management).

**For example:** To add the particpant API and management API, navigate to the [api-gateway](https://github.com/influenzanet/api-gateway) repository in Influenzanet, click on the fork icon to create a forked copy into your GitHub account.

### Manage Access 

Once your respositories are forked, you can provide access to individual developers or teams that are working to deploy the infectieradar platform. This has to be done for each of the forked repositories. If the decision is made to make changes only through the main Github account you have created (for ex: InfectieradarBE) this step can be skipped. 

Permissions for access can be granted by navigating to the forked repository, click on the settings >> manage access >>invite a collaborator. (this is visible only to the main Github account)

### Maintaining sync with Influenzanet

Throughout the course of your individual deployment of Infectieradar, there are going to be changes made to your forked repository that might need to be merged back into the main Influenzanet repository. To do so, generally create a pull request onto the Influenzanet repository from your fork using the official GitHub account. 

Do so periodically to avoid having to face issues in adopting in new features coming in from the Influenzanet repositories. 

##  From forks to your local machine

Most of the major tasks in configuring Infectieradar for a particular deployment mainly involve the participant-webapp, study-manager-app, study-manager-scripts, and cluster-management repositories. It is useful to have a clone of these in your local machine (from the newly created forks) to continue work on setting up the platform. Individual developers are also free to clone all services as they see fit.
