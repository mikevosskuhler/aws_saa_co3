# DB types
## OLTP
high frequency of reads / writes multiple operations per second
## OLAP
Complex queries against big datasets
# RDS
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
### Aurora
- drop in binary replacement for MySQL and MariaDB
- two versions --> MySQL compatible and PostgreSQL compatible
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
- can handle DB failover for your app, in case an instance dies
- allows IAM auth
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