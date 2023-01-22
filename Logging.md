- cloudtrail --> detailed log of read / write actions against your AWS resources (including who, when, and what happened)
- Cloudwatch --> numeric performance metrics for AWS and non-AWS resources (including on-prem servers)
- AWS Config --> tracks how your resources are configured and changed over time
# Cloudtrail
logs both API (resource manipulation / creation) and non-API (eg signins) in the AWS account. This includes API, SDK, and GUI usage. 
## management events
actions against resources
### write-only events
events that (might) modify resources
### read-only events
interactions that can read but cannot change resources
## data events
high volume events (S3 read / writes) and lambda executions
- S3 data events are group into read and write events
## event history
history is kept by defaults for 90 days (management events)
## trails
- allow you to store for longer than 90 days
- allows capture of non default events (such as S3 object interactions)
- delivers the logs to a S3 bucket (JSON format)
- single region or all regions
- up to 15 minute delay
# Cloudwatch
collect, retrieve, and graph numeric perf metrics (AWS and on-prem)
## cloudwatch metrics
- organized into name spaces which look like `aws/<servicename>`
- region specific, logs are only visible in the region they are logged
- `dimensions` allow for grouping of metrics. A dimension can for example be the instance ID
### basic VS detailed logging
depending on the service the log level can be basic or detailed
- basic level logs every 5 mins
- detailed level logs every 1 min
## log time resolution
- AWS services have a resolution of 1 min
- custom logs can have resolution up to 1 sec --> high resolution metrics
	- timestamp can be up to 1 week before and 2 hours forward when writing
## metric lifecycle
metric cannot be remove, they are deleted automatically. logs are agregated when they age to limit storage cost.
- high resolution (sub 1 minute) logs are stored for 3 hours --> then moved to 1 minute resolution
- after 15 days --> logs agregated to 5 min resolution
- after 63 days --> logs agregated to 1 hour resolution
- after 15 months --> completely deleted
## graphing
the following stats can be graphed:
- Sum
- Minimum
- Maximum
- Average
- Sample count
data is graphed by defining a metric, statistic period and timerange. the period is the span and the timerange to start and end time
- period --> 1 sec to 30 days
- timerange --> 1 minute and 15 months
### metric math
basic artimetic can be used in addition to:
- AVG
- MAX
- MIN
- STDEV
- SUM
## cloudwatch logs
records log events from app or AWS resource
- events from the same source go into one *log stream*
- log streams can be organized into log groups
- there is no limit on the number of streams in one group
- retention between 1 day and 10 years
### metric filters
metric filters can extract metrics from log streams to create cloudwatch metrics
- metrics should be a number, but can also be the contain of a string match
- metric filters are applied on a group basis
- they are not retroactive
## cloudwatch agent
can capture both metrics and logs, including metrics that are not natively supported
### cloudtrail to cloudwatch integration
cloudtrail can send trail logs to a cloudwatch log stream, the event size is limited to 256 KB
## cloudwatch alarms
tracks single metric and alert of threshold is breached. Then triggers actions like emails, scaling, or a reboot.
- threshold options include --> static, anomaly detection, math expression
- alarm states --> ALARM, OK, INSUFFICIENT_DATA
