# EC2
## AMIs
template with OS and app software for root volume. With four types:
- Quick start AMIs --> populair choises and supported 
- Marketplace AMIs --> official, production read, supported by the vendor
- Community --> 500+ 
- Private --> images created by yourself / your organization, can be imported using AWS VM import/export tool (utilizes S3)
A particular AMI is **only available in one region**
## Instance Types
|type|purpose|
|-|-|
|MacT2,T3,M6i,M6a,M5,M5a,M5n,M5zn,M4,A1|General purpose|
|T4g / M6g|General purpose Graviton2 (ARM)|
|C6,C7g,C6g,C6i,hpc6a,C5,C5a,C4|Coumpute Optimize --> high demand web servers / machine learning|
|R6g,R6i,R5,R5b,R5n,R4,X2gd,X2idn,X2iedn,X2iezn,X1e,X1,High Memory,Z1d|Memory optimized --> databases, caching, data analysis||
|P4,P3,P2,DL1,Trn1,G5,G3dn, G4ad,G3,F1,VT1|Accelerated Computing --> NVIDIA GPUs|
|lm4gn,ls4gen,l4i,l3en,D2,D3,D3en,H1|Storage Optimized --> EBS instance stores optimized for high IOPS|

## Tenancy
The way your EC2 instances are hosted on virtualization hosts
- shared tenancy --> host is shared between you and other AWS customers, default and cheapest option
- Dedicated tenancy / host --> in case you need a dedicated host because of regulation, more expensive

## Placement Groups
The way your instance are spread out across AWS infra. AWS by default does this for you, but you can pick a different model:
- Cluster Groups --> instance are launch in single AZ close to each other to limit latency
- Spread Groups --> spreads instances over seperate racks / AZs to increase redundancy
- Partition Groups --> groups some instances together while separating them from other partitions

## limits
can be increased by asking Amazon
- 5 VPC / region
- 5000 SSH key pairs

## Strorage
### EBS
- unlimited number per Instance
- only 1 instance per EBS store
- can be copied using snapshots --> can be used to create AMI
- AMI can be created while attached to instance (recommended to stop instance, but not required)
- very reliable (99.8%-99.9%) 
- EBS Data lifecycle manager --> automate snapshots and AMIs
- encryption using EBS managed key or KMS

EBS types:
- provisioned IOPS
	- IO1 --> 50 IOPS / GB (max 64000 IOPS)
	- IO2 --> 500 IOPS / GB
	- IO2 block express --> 256000 IOPS / volume
- general purpose SSD --> 3000 IOPS / Volume
- HDD
	- sc1 --> 0.015$ / GB-Month
	- sc2 --> 0.045$ / GB-Month --> 250MB/s per TB burst

### Instance stores
- physically attached to instance
- data is non persistent, reboot or system failure will remove the data

## connectivity
private IP ranges:
- 10.0.0.0./8
- 172.16.0.0/16
- 192.168.0.0/16

## security groups
SCs are host based firewall managed using the AWS management plain
- deny all by default
- statefull

## autoscaling
### launch config
- old --> not recommended anymore
- cannot be modified
- AMI, instance type, key pair, SG, instance profile, blcok devices, EBS optimization, tenancy, user data --> all part of launch config --> basically what you would configure when manually creating instance

### launch template
- same settings as launch config
- new --> recommended way of working
- versioned --> can be changed after creation

### autoscaling groups
When creating you an autoscaling group (group of EC2 instances) you need:
- launch config / template (previously created) --> required
- load balancer (previously created) --> optional
- health checks
	- default is EC2 health checks --> good for instance health, might miss app issues
- scaling policy --> optionally apply dynamic scaling
- scheduled actions --> optional, can be combined with dynamic policies
### Scaling policies
basic configs:
- minimum --> min amount of **healthy** instances
- maximum --> max amount of **healthy** instances
- desired capacity
dynamic scaling options --> based on thresholds for CPU, request / instance, avg bytes in, avg bytes out :
- simple scaling --> when threshold is exceeded the group will:
	- **ChangeInCapacity** --> add the exact number of instances to desired capacity
	- **ExactCapacity** --> set the desired capacity to specific number
	- **PercentChangeInCapacity** --> change desired capacity by `x` percent
	- uses cooldowns
- step scaling --> allows for smaller or larger steps based on how far the threshold is exceeded (using steps defined by you):
	- does not use cooldown periods
- Target tracking --> set a metric and a desired value, AWS deals with the rest

### scheduled actions
allow for scaling at specific days / dates / times
- has a start and end time
- cron schedule for intervals
- need to specify a min / max / desired capacity

## AWS systems manager
automating simple management task for cloud and on prem workloads
### actions
#### automation
take action agains AWS resources like cloudformation stacks or AMIs
#### run command
execute task / commands using the ssm agent
- requires the AmazonSSMManagedInstanceCore policy for the instance
#### session manager
interactive command line session using agent
- using TLS 1.2 to secure traffic
- logging available through cloudtrail

#### patch manager
uses patch baselines to patch. Patches are approved automatically after 7 days (auto-approval delay). 
- custom baselines can be made
- maintenance window can be define (or not if you want to patch always immediately)
- `AWS-RunPatchBaseline` document is executed to perform patching
#### state manager
handles software and configs on managed endpoints. Utilizes `AWS-GatherSoftwareInventory`
### Insights
#### built-in insights
available by default:
- config compliance --> comliance according to AWS config rules
- Cloud Trail events --> the most recent cloudtrail event for each resource
- personal health dashboard --> alerts for AWS issues, events resovled by AWS over last 24 hours
- Trusted advisor recommendations --> show perf, sec, fault tolerance recommendations and if you have exceeded 80% of a service limit
#### inventory
gathers info from endpoints using `AWS-GatherSoftwareInventory`every 30 minutes
#### complicance
patch and association status in comparison with the configured baseline

# Containers
## ECS
container service based on EC2
## ECS everywhere
ECS for on prem servers
- uses SSM agents in on prem instance
- AWS still charges money for the service
## EKS
kubernetes run in AWS, managed by AWS. 
- EKS anywhere available for on prem
- Amazon EKS Distro --> freely available package to run K8 on prem
## Fargate
serverless ECS (containers without managing / paying for always online servers). Pay as you go pricing.
## ECR
container registry on AWS
