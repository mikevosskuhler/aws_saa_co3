- Availability is represented as a percentage of the time that the app is available
- multiple components that can fail will have to have their availability multiplied to get the overall availability
# Terminology

|term|description|
|--|--|
|high availability|designed for maximum uptime of the system --> is not operating through failure just that failure (and its impact) is minimized|
|fault tolerance|designed to deal with failures of one or multiple components and still keep on working as expected --> operating through failure|
|disaster recovery|policies, tools, and procedures to respond and recover from a disaster|

HA < FT < DR
High availability optimizes uptime, Fault tolerance operates even if components fail, and Disaster recovery helps recover from stuff like natural disasters.

# EC2 autoscaling
|Type|Description|recommended|modifiable|
|---|---|---|---|
|Launch template|defines the AMI, machine type, SSH keys, SG, Instance Profile, Block devices, tenancy, and user data|legacy|No|
|Launch config|same as Launch template, but can also be used to manually launch an instance|Yes, replaces Launch templates|Yes|
## autoscaling groups
uses a launch template or config to spin up hosts
required settings --> min and max capacity
optional --> desired capacity
## application load balancer
can point to the autoscaling group making it highly available --> is fully available
## health checks
by default autoscaling uses EC2 health check to determine the instance health. You can also configure application specific health checks using HTTP requests and status codes
## autoscaling types
available metrics for scaling:
- CPU
- request count
- Network Bytes in
- Network Bytes Out
|scaling type|description|
|---|---|
|Manual|uses the desired capacity that you can manually adjust to determine the number of instance|
|Simple Scaling| when metric threshold is broken: ChangeInCapacity (increase by specified amount), or ExactCapacity, PercentChangeCapacity **has a default cooldown period of 300 sec** | 
| Step Scaling| multiple steps that determine the number of instances to be added, can scale up quickly since there can be multiple steps, warm up time is available to prevent excesive scaling |
| Target Tracking | automatically scales to attain a specified target value for the metric |
| scheduled actions | scaling at regular intervals |
## S3 resilience
- versioning
- region replication --> sync items from bucket in 1 region to bucket in another region
## EFS
- replicates accross AZs
- backup indiviual files to S3
- AWS backup service to schedule incremental backups
## EBS
- replaces across AZs
- snapshots --> in S3, so replicated across AZs
- Amazon Data Lifecycle Manager to create a **Snapshot lifecyle policy** with 12 or 24 hour interval
## database Resilience
- RDS --> snapshots --> recovery takes some minutes
- multi-AZ RDS --> standby instance in other region (synchronous replication)
- Aurora --> up to 15 replica's across AZs
## Dynamo DB
- tables are stored across AZs
- global tables can be replicated across regions
- point-in-time recovery lets you take backups from 35 day to 5 minutes back
## Resilient Networks
- Direct Connect --> garantee fast connection to the application for the users
## SQS
service that allows for loose coupling between components in the application
- messages can be 256 kb in size
- default visibility timeout is 30 (can be 0 sec to 12 hours)
- default retention is 4 days (min 1 min, max 14 days)
- delay queues --> wait before message become visible after submission (default 0 max 15 min)
- Message Timer --> like a delay queue but for specific message
### opperation types
|name|description|
|-|-|
|SendMessage|Adding new message to the queue|
|ReceiveMessage|consumer grabs one or more messages from the queue (visibility timeout started)|
|DeleteMessage|Consumer deletes message from queue after succesfull processing|

### queue types
|type|desc|
|-|-|
|standard|can cause duplicates and messages might not be consumer in order. Max messages in flight 120000|
|FIFO|messages are consumed in order, and exactly once. 3000 messages per sec or 20000 messages in flight|
### Polling
- short --> faster but no garantee that you get everything (default)
- long --> slow but complete (can take up to 20 secs)
### dead letter queue
catch all for messages that cannot be processed
- MaxReceive --> after reaching this number of retries the message gets send to the dead letter queue
- has a retention period like every other queue
- the retention is based on the original creation time, not when it was send to the dead letter queue
