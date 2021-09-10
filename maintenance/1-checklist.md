# Maintenance Activity Checklist


The following represents the list of activities to be perfomed over the course of the lifecycle of Influenzanet:

| Activity      | Priority  | Frequency |
| -------------- | :----------------:| ----------------:|
| Inspect support emails on the support account | High | As new emails come in / twice a week |
| Check mailgun logs if used as SMTP | High | Everyday during initial release, 2-3 times a week after |
| Check GKE resource usage [Cluster & per service] | Medium | Everyday during initial release, 2-3 times a week after |
| Increase or decrease resource allocation GKE | Medium | As needed |
| Periodially create data back-ups of the mongoDB | High | Monthly |
| Fetch user count, participant count & response count from DB | Medium | Weekly/Monthly |
| Routinely download participant responses | Medium | Weekly |
| Look for potential node upgrades by GKE | Low | Monthly |
| Communicate scheduled down-time when notified by GKE | Monthly | Monthly |
| Merge new versions from Upstream on Github | Low | Monthly |
| Re-deploy new versions if approved | Low | Monthly or more |

**Next**: [Issue Resolution](../maintenance/2-issue-resolution.md)