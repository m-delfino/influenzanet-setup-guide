# Creating the repositories

To begin with, it is advisable to set up an institution account on GitHub ideally with the country of deployment in mind. For example: `Influenzanet-IT`.

##  Influenzanet

The major components and services of the Influenzanet platform are centrally hosted under the [influenzanet](https://github.com/influenzanet) collection of repositories. The relevant repositories that we need are indicated in two tables in the central [README.md](../README.md)* file. For the deployment of a specific country, we will be creating forks of these repositories and making changes to these forks. Periodically, we will also be creating pull requests to the central repositories in [influenzanet](https://github.com/influenzanet) to maintain synchronization and to regularly take in updates.

\* Look for the name listed under the column "Repository"

## Creating and managing repositories

### Setting up the repositories

For each of the relevant services listed in the table in the central [README.md](../README.md), navigate to the repository hosted at Influenzanet. Make sure you are logged into the institution account you created on GitHub. Fork each of the repositories in the table into your institutions personal GitHub account. Once completed you should have 9 forked repositories (namely: api-gateway, study-service, user-management-service, messaging-service, logging-service, participant-webapp, study-manager-app, admin-scripts, cluster-management).

**For example:** to add the participant API and management API, navigate to the [api-gateway](https://github.com/influenzanet/api-gateway) repository in Influenzanet, click on the fork icon to create a forked copy into your GitHub account.

### Manage Access 

Once your repositories are forked, you can provide access to individual developers or teams that are working to deploy the Influenzanet platform. This has to be done for each of the forked repositories. If the decision is made to make changes only through the main GitHub account you have created (eg: `Influweb-IT`) this step can be skipped. 

Permissions for access can be granted by navigating to the forked repository, click on the settings >> manage access >>invite a collaborator (this is visible only to the owner of the main GitHub account)

### Maintaining sync with Influenzanet

Throughout the course of your individual deployment of Influenzanet, there are going to be changes made to your forked repository that might need to be merged back into the main Influenzanet repository. To do so, generally create a pull request onto the Influenzanet repository from your fork using the official GitHub account. 

Similarly, there may also be new features or changes coming in from the main Influenzanet repository. Periodically merge these into your forked repository to avoid having to face issues in adopting in new features coming in from the Influenzanet repositories.

##  From forks to your local machine

Most of the major tasks in configuring Influenzanet for a particular deployment mainly involve the participant-webapp, study-manager-app repositories. It is useful to have a clone of these in your local machine (from the newly created forks) to continue work on setting up the platform. Individual developers are also free to clone all services as they see fit.

**Next**: [Dockerhub Setup](../installation/2-dockerhub-setup.md)