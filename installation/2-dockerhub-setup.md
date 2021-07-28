
# Dockerhub Setup

If a code/configuration/styling/markdown change occurs in any of the 6 repositories, the repositories in question needs to be re-built. Here the term 're-built' refers to the process of creating a new ready-to-deploy version of the service (which we also call an image).

The images created in this manner are all stored in a central dockerhub repository

## Pre-requisites

To get started, a dockerhub account is needed where all the images can reside. To do so, go on to https://hub.docker.com/ and create a new account. (Example: A new dockerhub account by the name influenzanet).

## Set up the required repositories

Similar to the repositories created in Github, we will also be creating repositories to house the images of each service. This ends up becoming a catalog of different service images, along with the different versions available for each service.

Note: Although 6 repositories exist in Github, on Dockerhub we create 9 repositories since API-Gateway and Messaging-Service are further split into 2 and 3 images respectively.

### Creating the repositories

1. Sign into Dockerhub, cick on create repository.
2. Give a suitable name, use table below for reference.
3. Select visibility as "Public".
4. Under Build Settings, select Connect to Github. Select the orgnisation and respository name under Github. Use the table below for reference.
5. Click create and repeat the process for the rest of the images.

The table of reference for Image names and Github repositories follows below:

| Image Name       | Github Repository Name  |
| -------------- | ----------------:|
| participant-webapp     | participant-webapp |
| management-api-image  | api-gateway |
| participant-api-image  | api-gateway |
| messaging-service-image  | messaging-service |
| message-scheduler-image  | messaging-service |
| email-client-service-image | messaging-service |
| study-service-image | study-service |
| user-management-image | user-management-service |
| logging-service-image   | logging-service |


## Configuring Automated Builds

Most of the Github repositories comes with a Github Action associated with it. The github action, is a file containing code to build a docker image, and push this image to the dockerhub repository on every new commit.

The file (present in the folder .github/workflows) genrally contains the following:
```
name: Docker Image CI

on:
  workflow_dispatch:
    inputs:
      tags:
        description: 'Docker Tag override: [leave empty if not needed]'

jobs:

  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - run: git fetch --prune --unshallow --tags
    - name: Get current version
      id: version
      run: echo "REPO_VERSION=$(git describe --tags --abbrev=0)" >> $GITHUB_ENV
    - name: Docker image tag override
      if: ${{ github.event.inputs.tags && github.event.inputs.tags != '' }}
      run: echo "REPO_VERSION=${{github.event.inputs.tags}}" >> $GITHUB_ENV
    - name: docker login
      env:
        DOCKER_USER: ${{secrets.DOCKER_USER}}
        DOCKER_PASSWORD: ${{secrets.DOCKER_PASSWORD}}
      run: docker login -u $DOCKER_USER -p $DOCKER_PASSWORD
    - name: Build the Docker image
      run: docker build --build-arg ENV_FILE=env-sample.config . --file Dockerfile --tag ${{secrets.DOCKER_ORGANIZATION}}/${{secrets.DOCKER_REPO_NAME}}:$REPO_VERSION
    - name: Push Docker image
      run: docker push ${{secrets.DOCKER_ORGANIZATION}}/${{secrets.DOCKER_REPO_NAME}}:$REPO_VERSION
```

This can be modified to detect commits or pull requests on different braches as needed. 

### Docker secrets

Automated builds make use of secrets to store Dockerhub authentication information. This is used by Github actions to login to docker hub repository and push the newly built image to the hosted repository.

To configure secrets for a Github repository navigate to the settings of the repo (ex: https://github.com/influenzanet/user-management-service/settings), and click on secrets. Here you can create new secrets for your repository.

NOTE: You need to be signed as the owner of the repository to add these secrets.

The different secrets needed for each of the Github Repositories are listed below:

 - **Participant-webapp**:
	 - DOCKER_USER : Username for the account on dockerhub
	 - DOCKER_PASSWORD: Password for the account on dockerhub
	 - DOCKER_ORGANIZATION: Name of the organisation on dockerhub under which the repositories exist. (Ex: influenzanet)
	 - DOCKER_REPO_NAME: The actual name of the image repository to push to on dockerhub. (Ex:participant-webapp)
 - **Study-service**:
	 - Same fields as participant-webapp
 - **User-management-service**:
	 - Same fields as the participant-webapp
 - **Logging-service**:
	 - Same fields as the participant-webapp
 -  **API-Gateway**:
	 - DOCKER_USER
	 - DOCKER_PASSWORD
	 - DOCKER_ORGANIZATION
	 - DOCKER_MANAGEMENT_API_REPO_NAME: Name of the dockerhub repository for management-api. (ex: management-api-image)
	 - DOCKER_PARTICIPANT_API_REPO_NAME: Name of the dockerhub repository for participant-api. (ex: participant-api-image)
 - **Messaging-service**:
	 - DOCKER_USER
	 - DOCKER_PASSWORD
	 - DOCKER_ORGANIZATION
	 - DOCKER_REPO_MS: Name of the dockerhub repository for messaging-service (ex: messaging-service-image)
	 - DOCKER_REPO_MSC: Name of the dockerhub repository for messaging-scheduler (ex: messaging-scheduler-image)
	 - DOCKER_REPO_EC: Name of the dockerhub repository for email-client-service (ex: email-client-service-image)

Once the dockerhub repositories and the secrets have been configured. You will notice that each repository can trigger a build by navigating to the actions tab on github and by clicking of the run manual workflow option. An easy method of identifying the different images is by using the version which is identical to the last tagged release in the respective Github repository. You can also choose to override the version being tagged and provide it through a manual input on triggering the workflow.
