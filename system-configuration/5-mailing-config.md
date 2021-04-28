# Mailing Configuration

This sections walks through the configurations needed to connect an emailing service to the Infectieradar platform. Currently the platform only supports SMTP, so the mailing service chosen needs to behave as an SMTP relay gateway.

## Pre-requisites

1. Select an SMTP compatible Mailing Service, create an account there and retrieve the credentials needed to connect to this service. This includes the following:
    - Hostname of the mailing service
    - Port to connect to
    - Authentication information
        - User name or email to use
        - Password to connect with.
2. Some mailing services might also need additional configurations to be made with the dns provider. These include registering the domain to be used with Infectieradar, and creating additional records within the dns settings. All mailing services have their requirements and generally have guides to help configure these.

NOTE: This configuration can be skipped if the configuration changes are made to cluster-management as mentioned in the [readme](https://github.com/InfectieradarBE/cluster-management/blob/master/README.md) before running the install script.

### Configuration

After running the installation scripts, perform the following: 
1. Navigate to Kubernetes > Configuration
2. Select email-server-config, and click edit.
3. Ensure that the following fields are correctly updated for both "high-prio-smtp-servers.yaml" and "smtp-servers.yaml"
    - from: What users see as the from address when receiving Infectieradar mails.
    - sender: Email address of the sender(from can be a name as well)
    - replyTo: additional addresses to always send emails to (can be an empty array)
    - servers: consists of configurations to be set from the mailing service (obtained in pre-requisites): [Array type]
        - host: SMTP host
        - port: SMTP port
        - auth: contains the username and password credentials for the mailing service.
4. Save to update.