# AWS Lake Formation
data lake that can ingest data from many sources and uses AWS glue for ETL
- analytics can be done with:
	- Athena
	- QuickSight
	- RedShift Spectrum
	- Amazon EMR
	- AWS Glue --> Apache Spark based
# AWS tranfer family
tranfer data to and from S3 and EFS using these protocols:
- FTP
- SFTP
- FTPS
# Kinesis
process, store, and deliver **Streaming data**
## Kinesis video streams
ingest and index video at nearly unlimited scale
- producer consumer model
- also supports nonvideo data like audio subtitles GPS coordinates
- default retention 24 can be upped to 7 days
- consumers can read the data (eg SageMaker or Rekognition)
	- consumption can be realtime or later on
- HTTPS Live Streams (HLS) and Dynamic Adaptive Steaming Over HTTPS (DASH) are supported 
- Peer to Peer video conferencing supported using Web Real-Time Communication (WebRTC)
## Kinesis Data Streams
data pipeline for aggregation, buffering, and storage of streaming data
- logs can be ingested using the Amazon Kinesis Agent or Kinesis Producer Library (KPL)
- data streams are indexed using partition keys and sequence numbers
- multiple consumers are supported
- time between ingestion and consumption is usually less than 1 sec
- 1 shard supports 5 reads (2MB) per sec and 1000 records (1MB) written per second
## Data Firehose
Transform and send data to a select number of supported destinations (like Splunk HEC), sources for the firehose include Kinesis data streams. Also supports sending a copy of the data to S3.
## Data Streams vs Firehose
Data Streams are pulled by a consumer, while Firehose sends data to a destination