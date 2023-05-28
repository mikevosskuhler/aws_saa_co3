# IAM policies
consists of four components:
- Effect --> allow or deny
- Action / Operation --> actions defined by each AWS service that allows you to take a specified action on the resource. Can contain wildcards to match large number of actions
- Resource(s) --> the AWS resource(s) that the action can be taken agains. May use wildcards to specify large groups of resources
- Conditions --> restrictions imposed on the actions / when they can be made. EG, particular source IP, MFA enabled account, timeframe
## AWS managed policies
These are prepackaged policies offered by AWS. AWS maintains and updates these
## Customer managed
These policies are managed by the customer. They are standalone policies that can be attached to prinicipals in the account
## Inline policies
embedded into AWS Principal or Group
# Permission boundaries
lets you define a policy that specifies the maximum permission an AWS principal could have. Even if a wider policy is attached later on the permission will be limited to the boundary that was set. 
# Role
this is an AWS principal without password or access keys. It can be assumed by resources or users so they can take actions that were granted to the role
## Instance profile
allows an instance to use the STSLAssumeRole operation to assume a role that can then be used to interact with resources. 
- The access key / temporary creds are exposed to the instance on 169.254.169.254 with the specific role in the url
# service level protections
several services offer their own access controls
- S3
- KMS
- SNS
- SQS
# Guard Duty
analyzes VPC flow logs, CloudTrail management events and Route 53 DNS queries for malicious artifacts (IPs, domains, signatures)
SNS alerts for findings can be generated:
- Backdoor --> EC2 instance infected and involved in DDOS or spam behavior
- Behavior --> anomalous traffic
- Crypo --> mining
- Pentest --> interactions with kali pentoo or parrot instances
- Persistence --> anomalous IAM user behavior
- policy --> root credential usage
- recon --> recon behavior
- Resource Consumption --> anomalous resource usage by IAM user
- Stealth --> password policy weakened, CloudTrail log disabled, modified or deleted
- Trojan --> EC2 has potentially been infected with Trojan
- Unauthorized Access --> failed attempt to access your AWS account / resources (including SSH / RDP bruteforcing)
# Amazon Inspector
agent based service for ec2 using assesments on network filesystem and process activity. there are 5 rule packages available:
- CVE
- Center for Internet Security Benchmarks
- Security Best Practices --> linux only, eg root SSH, weak password policies, and filesystem permission misconfigs
- Runtime Behavior Analysis --> insecure protocols, unused open TCP ports and linx filesystem misconfigs
- Network Reachability 
# Amazon Detective
graph based visualization tool for cloudtrail, vpc flow, and guardduty events for incident investigations
# Security hub
Centralized dashboard to review security information including:
- inspector
- guardduty
- macie
- assesments based on the payment card industry data security statndard 
# amazon fraud detector
ml based tool to detect fraud. Includes fraud detection workflows can incorporate mulitple detections / metrics
# AWS Audit manager
controls and compliance reporting tool. Can map config settings to industry standards. supports custom reqs as well. 
# Network security 
- WAF --> LB, API gateway, CloudFront protection including signature and geolocation
- AWS shield --> ddos mitigation
	- standard --> layer 3 and 4
	- advanced --> layer 7
- AWS firewall manager --> enforce firewall rules across AWS Org
	- supported by: network firewalls, SGs, DNS firewalls, WAF, etc
# encryption
at rest data can be encrypted using aws or customer master key (CMK), this works for S3, EBS, EFS, RDS
## S3 
ecryption options:
- Server side encryption with S3 managed keys (SSE-S3)
- Server side encryption with KMS managed keys (SSE-KMS)
- Server side encryption with customer managed keys (SSE-C)
- client side encryption
encryption is managed on a per object basis, enabling bucket encryption only encrypts going forward
## EBS
encryption can be done when creating a volume or by creating a snapshot of an unecrypted AMI and then encrypting it
## EFS
encryption can be enabled when creating a filesystem
# Macie
service to automatically identify and classify sensitive data 
it can find trade secrets and PII, find open buckets, changes to bucket permissions, and find data based on customer defined criteria. 
findings are published to EventBridge and security hub