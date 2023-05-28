# Budgets
limit how much your resources can cost based on tags and get an alert if you are exceding the budget
# Cost explorer
a dashboard that lets you slice and dice your costs per instance, etc
# Cost and usage reports
writes cost reports to S3 to be queried with athena, quicksight, or redshift
# AWS Trusted Advisor
tracks your resources according to AWS best practices
- cost optimization
- preformance checks
- security checks
- fault tolerance
- service limits
# simple monthly calculator
tool for estimating costs based on service consumption

# reserved instances
allow you to reserve instances for a period of time and get a discount in return
# savings plan
more flexible in comparison to RI
- compute saving plan --> can be consumed in multiple compute service (EMR, ECS, EKS, Fargate, )
- EC2 instance plans --> region bound commitment
# EC2 Spot instances
auctioned off excess EC2 capacity
# autoscaling
can also support scaling down when traffic is low
# EBS lifecycle manager
allows you to create a rotaion policy that create, and also deletes snapshots at set intervals
