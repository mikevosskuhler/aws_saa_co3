# route53
## domain registration
- domain names can be transfered from exiting registrar
- you can point other registrars to AWS NS
## DNS management
- hosted zones can be private or public
- AWS creates a Start Of Authority (SOA) records and 4 name servers
## Availability monitoring
- health checks can be coupled with routing policy
- health checks regularly check if the resource is still responding
## Traffic managment
- routing policies can be used to manage traffic using DNS
### simple routing
- default option
- all request to same target (IP or Name)
### weighted
route to multiple targets using a configured ratio between them
### Latency routing
Route53 routes to targets with the least amount of latency from the clients location
### Failover routing
primary and secondary targets. Primary is used as long as its health checks pass
### Geolocation routing
allows for focussed content delivery by routing based on country state or continent based routing rules. if the region cannot be determined a default set will be used
### Multivalue Answer routing
offers high availability by allow for up to 8 resources combined with health checks
## Route53 traffic flow
visual tool to showcase combinations of policies and their resulting routing 
## Route53 resolver
allows integration with on prem workloads --> bi directional queries between on prem servers and AWS servers
# Cloudfront
AWS CDN 
different distribution types supported --> web distribution / Real Time Messaging Protocal
## origin types
|Category|description|
|----|----|
|Amazon S3 Bucket|any accesible bucket|
|AWS MediaPackage channel endpoint|video packaging and origination|
|AWS MediaStore container endpoint|Media optimized storage service|
|App LB|AWS ALB|
|Lambda| serverless|
|custom origin|any HTTP server (even on prem)|

