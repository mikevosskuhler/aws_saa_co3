# S3 cross region replication
- async
- done by creating a replication rule
- write from one bucket to secondary bucket
# S3 transfer accelerator
- adds a per GB cost for usage
- routes data through CloudFront edge locations to speed up transfer
- provides a tool to check if it would add speed advantages using a custom url: s3-accelerate-speedtest.cs-accelerate.amazonaws.com
# Cloudfront S3 origin
allows you to host items in S3 and accelerate them using cloudfront CDN
# RDS
You can optimize RDS instances using the following settings:
- Instance type
- Schemas, Indexes, and views
- options groups --> additional functionality that can be configured
- parameter groups --> behavior controls for the database
# Networking
## Available interfaces
- Intel 82599 Virtual Function (VF)
- Elastic Network Adapter (ENA)
- Elastic Fabric Adapter (EFA)
# OpsWorks
Opsworks supports:
- Chef
- Puppet
# Monitoring
cloudwatch dashboards can be setup to contain widgets based on the data avaiable in cloudwatch
# Elasticache
supports clusters with one or more nodes with the following engines:
- MemcacheD --> simple setup but only support BLOBs
- Redis --> supports complex datatypes like strings, lists, and sets
# Partitioning / Sharding
might be nessary for your databases if they cannot scale further vertically.
# Compression
can be incorporated in application code, also used by CloudFront to safe on bandwidth.

