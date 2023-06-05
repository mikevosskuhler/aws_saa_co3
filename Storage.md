# S3 
object store on AWS
- items are stored with 2KB of meta data --> permission and the location of the file
- up to 100 buckets / per account by default
## prefixes / delimiters
prefixes allows users to organize files in a filesystem like structure. eg /folder1/objectname, with `folder1` being the prefix and `/` being the delimiter
## large objects
- single object can at most be 5 TB
- individual uploads can be 5 GB at most
	- multiplart upload recommended for objects over 100 MB
## S3 transfer Accelation
this service routes uploads through edge locations to speed up the upload
- AWS S3 transfer accelaration speed comparison tool availble to check if the accelaration will help
## Encryption
two kinds of encryption are available server and client side
### server side
- S3 managed keys (SSE-S3) --> server side fully managed by AWS using their enterprise standard keys (AES-256)
- KMS managed keys (KMS) --> server side fully managed by AWS (optionally with your own key in KMS). Adds an envelope keys and audit trailing for tracking
- Customer provided keys (SSE-C) --> the key is offered by the client when accessing / writing objects. The encryption process is then handled by AWS, but the key will be removed after that
### client side
this strategy can use KMS or a fully customer managed key. Each object is encrypted using a unique encryption key, this key is attached to the object (as metadata). before attaching the unique encrytion key it is encryted using a master key. This master key can be stored in KMS or by the customer. 
## logging
by default logging is diabled. To enable it you specify the source bucket (that you want to track) and a log destination bucket. Prefixes may be used to organize the logs and logs may come in with a slight delay
## durablility / availability
durability --> the percentage of data loss that can be expected over a year
availability --> percentage of time (in a year) that the data will be available
## eventual consistency
when changing objects it can take up to 2 seconds for the new version to be available. with new objects AWS S3 offers read-after-write consistency (data is available immediately), there is no risk of conflicts in this case. 
## S3 lifecycle
- bucket version will preserve every bucket object version indefinitely
- life cycle management allows for changing object storage tiers (object will need to stay for 30 days in a class). this also does not allow you to go from `S3 standard` to `Reduced Redundancy` directly
## Access control
- ACLs --> legacy from before IAM was available
- IAM --> user / group based control
- bucket policies --> bucket based control
### pre-signed URLs
provides temporary access to an object for a set amount of time (default 3600 seconds)
### S3 (glacier) select
provides the ability to filter structured data (csv) server side, thus reducing the amount of data received and the amount of compute required client side. 
## Glacier
used for long term storage
- encrypted by default
- objects are not named, but get a machine generated ID
- Objects are refered to as archives
- buckets are called vaults
Three types are available:
- Glacier flexible retrieval --> relatively fast retrieval, lowest retrieval cost
- Glacier instant retrieval --> fastest retrieval, most expensive
- Deep archive --> retrieval cost up to 12 hours, intermediate retrieval cost
# Elastic file system
NFS compatible file shares accesible using VPCs
# Amazon FSx
three flavors available:
- FSx for Lustre --> open source distributed file system for linux
- FSx for Windows --> SMB, NTFS, AD compatible
- FSx for OpenZFS
- FSx for NetApp ONTAP
# AWS storage gateway
compatible with legacy physical backup devices (like tape drives) and the way legacy apps integrate with that technology. Its run on S3 and EBS, but presents itself using conventional connectivity interfaces. 
# AWS Snow 
All snow devices are 265bit encrypted and HIPAA compliant
- snowcone --> 22 TB storage, 4 vCPUs, 4GB mem, RJ45, Wifi module
- Snowball Edge storage optimized --> 80 TB storage, 40 vCPUs, 80 GB mem
- Snowball Edge Computer Optimized --> 42 TB storage, 52 vCPUs, 208 GB mem
- Snowmobile --> 45 foot shipping container shipped to you DC
# AWS DataSync
tool for moving on-prem data to the cloud (over the internet)
- supports any AWS service
- supports transfer rates up to 10Gbps (if your bandwith allows it)
