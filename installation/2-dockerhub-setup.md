# Dockerhub Setup

If a code/configuration/styling/markdown change occurs in any of the 6 repositories hosting the services source code, the repository in question needs to be re-built. Basically, this has to happen every time content of any kind is changed in the front end or in the back end. Here the term 're-built' refers to the process of creating a new ready-to-deploy version of the service (ie: a docker image).

The docker images created in this manner are all stored in a central dockerhub repository.

The first time the Influenzanet platform is deployed for a specific country all the services images will have to be built in this way. 

## Pre-requisites

To get started, a dockerhub repository is needed where all the images can reside. To do so, go on to https://hub.docker.com/ and create a new account. (Example: Influweb-IT).

## Configuring docker image builds

Each of the GitHub repositories comes with a GitHub Action associated with it. The GitHub action is a file containing code to build a docker image and push this image to the previously created dockerhub repository.

The action file (present in the repository folder .github/workflows) generally contains the following:

```yaml
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

By default the build action must be run manually. This behaviour can be modified to automatically build and push the image on Dockerhub when commits or pull requests are detected on specific branches. 

### Docker secrets

Action files make use of **secrets** to store Dockerhub authentication information. They are used by GitHub actions to login into the Dockerhub repository and push the newly built image.

To configure secrets for a GitHub repository navigate to the settings of the repository  (ex: https://github.com/Influweb-IT/user-management-service/settings), and click on secrets. Here you can create new secrets for your repository.

**Organization secrets** will be visible to each repository in the organization. **Repository secrets** will be visible only to that particular repository.

The different secrets needed for each of the GitHub Repositories are listed below:

 - **participant-webapp**:
	 - DOCKER_USER : Username for the account on dockerhub (Organization secret)
	 - DOCKER_PASSWORD: Password for the account on dockerhub (Organization secret)
	 - DOCKER_ORGANIZATION: Name of the organization on dockerhub under which the repositories exist. (Ex: influenzanet) (Organization secret)
	 - DOCKER_REPO_NAME: The actual name of the image repository to push to on dockerhub. (Ex:participant-webapp) (Repository secret)
 - **study-service**:
	 - Same fields as participant-webapp
 - **user-management-service**:
	 - Same fields as the participant-webapp
 - **logging-service**:
	 - Same fields as the participant-webapp
 -  **api-gateway**:
	 - DOCKER_USER (Organization secret)
	 - DOCKER_PASSWORD (Organization secret)
	 - DOCKER_ORGANIZATION (Organization secret)
	 - DOCKER_MANAGEMENT_API_REPO_NAME: Name of the dockerhub image for management-api. (ex: management-api-image) (Repository secret)
	 - DOCKER_PARTICIPANT_API_REPO_NAME: Name of the dockerhub image for participant-api. (ex: participant-api-image) (Repository secret)
 - **messaging-service**:
	 - DOCKER_USER (Organization secret)
	 - DOCKER_PASSWORD (Organization secret)
	 - DOCKER_ORGANIZATION (Organization secret)
	 - DOCKER_REPO_MS: Name of the dockerhub image for messaging-service (ex: messaging-service-image) (Repository secret)
	 - DOCKER_REPO_MSC: Name of the dockerhub image for messaging-scheduler (ex: messaging-scheduler-image) (Repository secret)
	 - DOCKER_REPO_EC: Name of the dockerhub image for email-client-service (ex: email-client-service-image) (Repository secret)

Once the Dockerhub repositories and the secrets have been configured, you will notice that each repository can trigger a build by navigating to the actions tab on GitHub, selecting "Docker Image CI" and by clicking of the "run workflow" button. An easy method of identifying the different images is by using the version which is identical to the last tagged release in the respective GitHub repository. You can also choose to override the version being tagged and provide it through a manual input on triggering the workflow.

**Next**: [Setting up GKE](../installation/3-install-influenzanet-gke.md)
