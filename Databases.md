

# ACID vs BASE
CAP Theorem --> Consistency, availability, Partition tolerance (you can only have two)
- ACID --> Atomic Consistent Isolated Durable
	- **Most RDS databases are ACID** --> mostly SQL
	- Atomic --> all part of a transaction are successful, or the whole transaction fails
	- Consistent --> The database moves from one valid state to another, it cannot lead to a state that is not compliant with the database rules
	- Isolated --> transactions are separated from each other
	- Durable --> once committed transactions are durable and cannot be lost
- BASE --> Basically available SoftState EventuallyConsistent 
	- mostly NoSQL --> **DynamoDB** --> DynamoDB transactions is ACID
	- Basically available --> highly available but not guaranteed consistent
	- Soft State --> doesnt enforce consistency, so the app has to take care of this
	- Eventually Consistent --> reads and writes will ultimately be consistent, but it may take time
# DB types
## OLTP
high frequency of reads / writes multiple operations per second
## OLAP
Complex queries against big datasets
# RDS
Database Server as a Service --> one RDS DB server can run multiple databases. 
- AWS manages the Server including updates, you do not get OS access. 
- Runs in VPC --> AWS private service
- each instance has dedicated storage using EBS --> aurora does this differently
## Networking
- Uses subnet groups --> list of subnets that can be used by a specific RDS DB instance(s)
	- primary and standby are put in random subnet in the SN group (but they will be in different AZs) --> you can also pick them yourself
- RDS instance can be deployed in public subnets with public IP address
## Resilience
- snapshots --> S3 (AWS managed, not customer visible) --> can be replicated accross region (has to be configured)
	- S3 --> regionally resilient
	- manual --> called snapshot, can only be done manually, or through a script, snaps the whole instance (all DBs).
		- initial snapshot take whole instance, after that its increments 
		- causes a short read / write interuption --> for single AZ, multiAZ uses secondary
		- Snapshots don't expire and persist after DB deletion
- automated backups --> once per day
	- similar to snapshots
	- every 5 minutes the transaction logs are written to S3
	- retention can be 0 to 35 days --> 0 means no backups
- restoring from a snapshot / backup --> backup is restored and transaction logs are replayed | can be time consuming
- RR --> separate from MultiAZs and need support from the app. 
	- max 5 per DB instance
	-  can be promoted to primary for fast recovery --> only works if data is not corrupted
	- read only until promoted
	- can be across regions
- MultiAZ --> across AZs (not regions), each instance has their own storage
	- Instance deployment --> uses standby instance 
		- Synchronous replication at storage level
		- standby cannot be reached (at failover the CNAME will change to secondary), 
		- the standby does incur cost (its running), 
		- backups are taken from the secondary
	- Cluster Deployment --> uses up to two read replica's, better for performance, 
		- only primary can write, 
		- cluster CNAME --> read / write endpoint, 
		- reader CNAME --> points to any reader instance (potentially including primary), 
		- uses Graviton hardware, 
		- replication is done using transaction logs
## Security
- ssl/tls in transit
- encrytion at rest 
	- EBS uses KMS --> default (this is handled by EBS / host, so transparent to the DB)
	- CMK --> Storage / Logs / Snapshots / replica all encrypted with same key --> cannot be removed once turned on
- IAM auth --> by default the DB native user management is used
	- AWS IAM auth is also supported --> AWS IAM manages access tokens / roles and rotation--> only authentication

## DB engines
### MySQL
- OLTP
- RDS offers the latest MySQL community edition
- storage engines --> MyISAM / InnoDB
- oracle owned
### MariaDB
- opensource alternative to MySQL
- storage engines --> XtraDB / InnoDB
### Oracle
supported versions:
- Oracle Database 21c
- Oracle Database 19C
- Oracle Database 12c Release 2
- Oracle Database 12c Release 1
### PostgreSQL
Oracle compatible open source
### Aurora (provisioned)
- separate product from RDS
- uses a cluster of read/write replicas 
- drop in binary replacement for MySQL and MariaDB
- two versions --> MySQL compatible and PostgreSQL compatible
- data is replicated at the storage layer --> storage is distributed over 3 AZs (all nodes can access all the data)
- up to 15 replica's --> can be added / removed as required --> storage is separated from instances
- storage uses high IOPS SSDs and you are billed based on the high watermark --> watermark cannot be reset
	- additional charge per IO request
- cluster and reader endpoint
	- cluster points to primary --> can write
	- reader points to replica's --> can only read
	- individual instances also have their own endpoint
- 100% of DB size is included for backups --> should be sufficient unless your DB changes a lot
- Backups work similar to RDS --> if you restore a new cluster is created
	- you can also backtrack using a in place rewinds --> backtrack to before corruption
	- fast clone --> only contains the difference in data between the original and the clone --> saves data size as well. 
### Aurora serverless
uses ACU (Aurora Capacity Units) 
- MIN and MAX ACU can be configured
- can go to 0 ACU
- billing per second
- same resilience as Aurora provisioned --> uses the same storage setup as Aurora Provisioned
- ACUs are shared hot instances between customers without local storage then they use the storage layer
### Aurora global databases
replicate DBs across regions from 1 primary region
- primary region --> 1 write and up to 15 read
- secondary region (max 5) --> up to 16 reads (1 sec replication time from primary region)
- to be used for cross region resilience 
- low latency reads
- replication at storage layer
### Aurora Multi-Master
multiple write nodes in the cluster
- app communicates directly with the instances (no LB)
- changes are proposed by the proposing node to the other nodes, once allowed it willl write to storage and send it to the other nodes for caching
- higher availability since there is no failover time
### Microsoft SQL server
versions ranging from 2012 SP4 GDR and up
available editions include Express, Web, Standard, or Enterprise
## Licensing
- open source license --> MariaDB / MySQL / PostgreSQL
- license included --> Microsoft SQL / Oracle Database Standard Edition Two (SE2)
- BYOL --> Oracle Enterprise Edition (EE) / Oracle Standard Edition Two (SE2)
## Option groups
Options groups let you apply additional management options to one or more instances in RDS, options include:
- S3 integration (Oracle)
- transparent data encrytion (TDE) (oracle / MS)
- audit plugin for user logons / queries (MySQL / MariaDB)
*does consume additional memory*
## instance classes
you can switch between the different classes
### Standard
up to:
- 512 GB mem
- 128 vCPU
- 40 GBps bandwidth
- 50000 Mbps disk throughput
### Memory optimized
up to:
- 3904 GB mem
- 128 vCPU
- 25 GBps bandwidth
- 14000 Mbps disk throughput
### Burstable instances
- 32 GB mem
- 8 vCPU
- 5 Gbps bandwidth
- 2048 Mbps disk throughput
## storage
- one IOPS is max 32 KB
disk classes:
- general SSD (GP2) --> 2000 MBps throughput / 16000 IOPS per volume
- Provisioned IOPS SSD (io1) --> 256000 IOPS per volume
- magnetic --> max 1 TB and 100 IOPS
## Read replicas
- up to 5 read replicas 
- 15 for aurora
- it is async, so there is a delay
- the read only endpoint connects you to only the read replicas.
## high availability
- multi AZ is available
- one primary instance in one AZ
- standby instance in different region
- failover in 2 minutes
- can be configured on creation or later
- instances need to be in the same region
### aurora
aurora has more features for HA
- single mast / multi master
	- single master --> synchrous replication across three AZs, replica will be promoted if master fails
	- multi master --> no primary, every instance can write to the DB
## backup / recover
- EBS volume snapshots
	- backed up in multiple zones
	- suspends IO --> pick a low traffic time
	- time to restore from backup can be several minutes
- RTO --> Recovery Time Objective, time to get the DB up and running
- RPO --> Recover point Objective, amount of time over which data may be lost
### automated backups
enabling automatic backups enables point in time recovery --> archives DB  change logs every 5 minutes
- retention can be between 1 and 35 days, default is 7 days
- 0 days retention disables backups
## upgrade cycles
- major upgrades may not be backwards compatible, so you have to apply manually
- you can specify a weekly 30 minute upgrade window
- the upgrade window cannot overlap with the backup window
## Amazon RDS Proxy
sits between app / consumer and the DB
- limits number of open connections
- detect and hold open connections to prevent timeout
- can handle DB failover for your app, in case an instance dies --> DB is hidden from app
- allows IAM auth
- uses long term connection pool --> not based on client app interactions
- auto scaling and highly available and fully manged 
- works with RDS and Aurora
- only accessible in VPC
- proxy endpoints being used by the app
# Redshift
- based on PostgreSQL
- supports Open Database Connectivity (ODBC) or Java Database Connectivity (JDBC) connectors
## Compute nodes
a cluster contains 1 or more nodes
- Dense Compute --> 326 TB storage
- Dense Storage --> 2 PB storage
- RA3 --> 16384 TB storage, faster than Dense storage
## Distribution styles
- EVEN --> distributed evenly across nodes
- KEY --> grouped by key
- ALL --> every table is distibuted to all nodes
## Redshift spectrum
query data on S3
# DB migration service
used a EC2 instance called a DMS which performs the replication
- you give it a source and dest endpoint with a source and target DB
	- one of the endpoints has to be on AWS
- schema conversion tool (SCT) --> migration between incompatible DB types
	- can also be used in conjunction with Snowball
# NoSQL DBs
- large volumes of transactions
- less storage efficient
- query by primary / secondary key
## Dynamo
- 10 GB partitions
### primary / secondary keys
the (composite) primary key should be unique across the table
- no larger than 2048 bytes
- also called hash key
- composite primary key == combination of primary key and sort key
	- sort key can be 1024 bytes tops
### atributes and items
items can be 400 kb at most
### scaling
- determined by RCU (read capacity units) and WCU (write capacity units)
- 1 RCU is one strongly consistent read per second (4 KB)
- 1 WCU is one write per second (1 KB)
- autoscaling is available --> define min / max RCU and WCU and desired utilize percentage
- reserved capacity is available --> at least 100 RCU / WCU 100000 max. period of 1 to 3 years
### reading data
- scan reads all data --> expensive
- query get only matching items --> using the partition key
### secondary indexes 
are like a copy of the table (only specified fields), there are two types, global and local
- global --> hash and partition key can be different from base table, create any time
- local --> different sort key but same partition key, can only be created when creating base table
### global tables
replicate data across regions, only one replica per region. strongly consistent reads not available
### Backups
unlimited number of backups, no RCU / WCU cost. can be restored to any region