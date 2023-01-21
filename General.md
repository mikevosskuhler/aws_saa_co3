# AWS organisations
- unified SSO integration accross accounts
- Apply IAM rules globally using service control policies (SCPs)
- A single account's cloudtrail can watch events from all accounts in the organization

# AWS control tower
automate setting up AWS accounts (including AWS organizations) and setup a landing zone for users. A landing zone is a well architected AWS organization with relevant accounts to get started building your environment. 

# AWS service catalog
Cloudformation template based catalog that makes specified resources available to selected users / groups

# AWS license manager
centralized license manager, including usage / compliance tracking. 
- on-prem and cloud
- free of charge

# AWS artifact
compliance related reports on demand, usefull for audits
- SOCs
- PCI DSS
- ISO 27001

# Support plans
| level | cost | coverage|
|-|-|-|
|basic|free|customer service for billing and account issues AND forum acceess|
|devloper| 29 euro / month or 3 percent of account charges|access for one person to Cloud Support Associate and limited guidance and "system impaired" response|
|Business|100 euro / 10 percent of monthly cost|faster guidance for "impaired systems", more users with access, personal guidance and troubleshooting, and the support API||
|Enterprise On-Ramp|the greater of 5.5k / 20 percent of account cost|fast response for business critical systems, proactive guidance / reviews based on your apps|
|Enterprise support|15k (at least)|direct access to AWS architects / technicall account manager / online labs|

