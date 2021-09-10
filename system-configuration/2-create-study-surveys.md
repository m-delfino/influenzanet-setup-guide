# Creating studies and surveys

Influenzanet is firstly a survey platform. This section explains the steps involved in creating a new study in the system and then adding surveys to the study.

## Pre-requisites

To create, and upload studies and surveys you need to configure two repositories on a local machine:

1. [Study-Manager-App](https://github.com/influenzanet/study-manager-app): Clone the repository in the link. Install the required dependencies by running ``` yarn start ```, and you can get started with designing the surveys.
2. [Admin-Scripts](https://github.com/influenzanet/admin-scripts): Clone the repository in the link. The readme here consists of explanations of how to upload new surveys and studies to a deployed version of influenzanet. Navigate to resources/config.yaml and enter the credentials of the admin user (configured in mongo configurations). Also, add the links for the management API and participant API. (Generally https://your-domain-goes-here/admin & https://your-domain-goes-here/api ) 

    ```yaml
    management_api_url: "https://[your-domain]/admin"
    participant_api_url: "https://[your-domain]/api"
    user_credentials:
    email: [admin_email]
    password: [admin_password]
    instanceId: belgium
    ```

## Creating a study

Studies contain surveys that are offered to participants of the study. To begin with, we first create a study by following the steps below:

1. Enter into the cloned [Admin-Scripts](https://github.com/influenzanet/admin-scripts) folder by running ``` cd study-manager-scripts ``` folder.
2. Navigate to resources/studies/props.yaml and give the new study a key in the studyKey attribute. (Ex: influenzanet-be).
3. In the resources/studies/study_def.yaml you can change the translations that show up describing the study as suited to you.
4. In the resources/studies/study_rules.yaml you can change configure how frequently the different surveys show up. But generally leave this file untouched.
5. Once done, navigate back to the root folder at study-manager-scripts and run the script ``` python run_create_study.py --study_def_path resources/study ```

This creates the study in the platform.

## Creating and adding surveys

To create a new survey make use of the [study-manager-app](https://github.com/influenzanet/study-manager-app) to code in your survey. A visual representation of what the survey will look like can be seen by running ```yarn start```. A detailed video describing this procedure can be found by contacting influenzanet@uhasselt.be. 

Once the questions have been coded in, you can download the surveys for intake and weekly questions by following these steps:

1. Run ```yarn start```

2. In the User Interface select for example the instance "belgium or bel" and select intake. Replace belgium with your instance here.

3. This loads the visual representation of the intake survey. Scroll to the bottom, enter the studykey "influenzanet-be" (or what you set previously while creating a study) and click download.

4. Repeat step 3 for weekly surveys.

This should give you 2 surveys that you then need to copy into the resources/surveys folder of the locally cloned [Admin-Scripts](https://github.com/influenzanet/admin-scripts) repository.

Once these files have been copied, run the following:
1. To upload the intake survey, ```python run_save_survey.py --study_key <your-study-key> --survey_json resources/study/<downloaded-intake-file-name>.json ```
2. To upload the weekly survey, ```python run_save_survey.py --study_key <your-study-key> --survey_json resources/study/<downloaded-weekly-file-name>.json ```


With these steps completed, a logged-in user should see the intake survey. Weekly surveys will show up once the intake has been filled, on the configured day of the week.

When adding new surveys (eg: vaccination surveys) the same steps must be followed.

To make updates to the intake or weekly questionnaire (or to any other survey), repeat the entire process and re-upload the surveys using the same scripts.

**Next**: [Setting up Emails](../system-configuration/3-email-setup.md)