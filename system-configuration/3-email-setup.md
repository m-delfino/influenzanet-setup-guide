# Setting up Emails 

This section deals with the kinds of email templates needed and how to upload them to a running instance of Influenzanet.

Influenzanet uses the following email templates:

| Email Name       | Purpose  |
| -------------- | :----------------:|
| **registration**    | When a new account is created, this message will be sent to the used email address. This message should include link to email verification route including the token (see below). |
| **invitation**| this email is sent out when a user account was created by an admin. This message should include link to invite link route including the token (see below)|
| **verify-email**| if a user requests a new verification email, or in case the email address is changed in the application, this email will be sent out. Should also contain the specific link for email verification|
| **password-reset**| after triggering the password reset workflow, this email-template will be used. Should contain link with url toward the password reset link resolver of the web-client, including the appropriate token (see below)|
| **verification-code**| After login, the user will receive this message, that should include the 6 digit numeric code prominently|
| **password-changed**| after a successful password change, the user will receive an email about the event. This template needs to define the content of this email|
| **accound-id-changed**| when a user changes the email address in the application, this message will be sent to the *old* email address. Can be used to warn about potential inappropriate access, or tell the user to use the new email to login|
| **account-deleted**| as a last contact, this message will be sent out to confirm the successful account deletion|


**NOTE**: One additional type of email is the weekly reminder email. 

## Pre-requisites

Clone the private repository [admin-scripts](https://github.com/influenzanet/admin-scripts). The ```readme.md``` file inside the [study-manager-scripts](https://github.com/influenzanet/admin-scripts/tree/master/study-manager-scripts) folder contains explanations on how to upload new surveys and studies to a deployed version of influenzanet. 


## Creating E-mail templates

For each of the e-mail types, a template needs to be created. Additionally each template needs to have copies in different languages intended to be supported by the platform. An example of this can be found in the resources/email_templates folder of the [Study-Manager-Scripts](https://github.com/influenzanet/study-manager-scripts) repository. You can chose to use the same (with changes to the logo's and content) or you can create fresh templates.

Along with the content of each email, there are also certain variables that need to be configured. (For ex to insert the login code for 2FA in the verification code email: ```{{index . "verificationCode"}}```). 
Use the examples as a guide to understand what variables are needed, and also refer to the variables listed in the [email-template guide](https://github.com/influenzanet/messaging-service/blob/master/docs/email-templates.md). 

In addition, a weekly email template is also needed to send survey reminders to participants once every week. The example for this exists in the folder resources/auto_reminder_email (one for each language supported). 

## Uploading the templates to Influenzanet

After creating a fresh email template or updating (in case of changes) an existing one in the [admin-scripts](https://github.com/influenzanet/admin-scripts) repository, follow these steps:

1. **Uploading email-templates**: Ensure that all the sets of emails are place under a folder named with the language-code (eg: en). This folder should reside within resources/email_templates. An example is already available for the de-be, en, fr-be, and nl-be codes. Along with the emails there should also be a file called subjects.yaml containing the following:

    ```yaml
    account-deleted: "Your account has been successfully deleted"
    account-id-changed: "Your email address has been changed"
    password-changed: "Your password has been changed"
    password-reset: "Change your password"
    registration: "Registration influenzanet"
    invitation: "Invitation influenzanet"
    verification-code: "Verification code"
    verify-email: "Confirm email address"
    weekly: "Your weekly questionnaire is ready for you"
    ```

    1.1. Create a copy of this file under each language code, containing the translations for the email subject for each language. (the key remains the same, only the value is translated)
    
    1.2. Once this is done, to upload the email-templates run ``` python run_email_template_uploader.py ``` from the root of the repository.

2. **Uploading Weekly reminders**: Save each language version of the weekly reminder as language-code.html within the resources/auto_reminder_email folder. Additionally, also create a settings.yaml file here with the following content:

    ```yaml
    sendTo: "all-users"
    studyKey: "influenzanet-be"
    messageType: "weekly"
    nextTime: "2021-03-26-06:07:00"
    period: 86400
    defaultLanguage: "nl-be"
    translations:
    - lang: "nl-be"
        subject: "Uw wekelijkse vragenlijst staat klaar"
        templateFile: "nl-be.html"
    - lang: "en"
        subject: "Your weekly questionnaire is ready for you"
        templateFile: "en.html"
    - lang: "fr-be"
        subject: "Votre nouveau questionnaire hebdomadaire est prêt"
        templateFile: "fr-be.html"
    - lang: "de-be"
        subject: "Ihre wöchentliche Frageliste ist fertig"
        templateFile: "de-be.html"
    ```
    
    2.1. Configure the study-key here
    
    2.2. sendTo: "all-users" indicates that this mail needs to be sent to all users in the system.
    
    2.3. nextTime: Indicates the next time emails are sent after this new version of the weekly emails are uploaded. (Always keep this to a future date)
    
    2.4. Configure the Subject of the emails for each language as shown in the example above.
    
    2.5. To upload the weekly reminder run the following script:  ``` python run_create_auto_mail.py ```
   
**Next**: [When To Redeploy Changes](../redeploying-changes/1-change-types.md)