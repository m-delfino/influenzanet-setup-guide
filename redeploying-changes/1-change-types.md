# Types of Changes

Before proceeding, we cover the different types of changes where a simple, straightforward re-deployment is advisable: 

1. Markdown changes: Changing the text on any of the web-pages.

2. Image/Video changes: Change images, or video links in the Influenzanet web UI.

3. Bug fixes: Tested changes to any of the 6 repositories or 9 images that form a part of Influenzanet.

4. New features: Tested feature addition/changes to any components of the Influenzanet platform.

These are the scenarios where a re-deployment is not advised or is simply not needed:

1. Untested bug-fixes/features: Re-deployment goes live immediately, so this should be avoided.

2. Changes like new emails, surveys or studies do not need a re-deployment.

3. Configurations changes like changing ReCAPTCHA keys, mongo credentials, changing period between new weekly surveys, and changing user sign up limits do not need a re-deployment. General rule of thumb here is to refer to the deployment files in [cluster management](https://github.com/influenzanet/cluster-management). Any env variable defined in these files can be changed on the fly, without a re-deploying a new version.

**Next**: [Redeploying Changes](../redeploying-changes/2-build-and-redeploy.md)