# Roles
temporary identity that can be assumed by user or service
for types of trusted entities:
- AWS service
- Another AWS account
- Web identity (Amazon, Facebook, Google)
- SAML 2.0 federation
once assumed, AWS STS will provide a temporary security token
# Cognito
- managed user sign up and sign in for your apps
- lets you grant temporary access to users to services in your AWS account
- a pool of users is granted an IAM role which determines access levels
# AWS managed MS AD
AWS managed MS AD environment allowing you to control access to MS services (sharepoint etc)
- DCs run in two VPC AZs
## AD connector
connect to on prem MS AD
# AWS SSO
singled sign on for several popular services and SAML 2.0 support
# AWS KMS
create rotate and deleted keys, integrated logging with AWS cloudtrail
# AWS secrets manager
part of ssm and supports credential rotation
# AWS CloudHSM
AWS managed hardware security module
- keys are stored in dedicated, thirdparty validate HSMs under your exclusive control
- FIPS compliant
- PKCS, java JCE, MS CNG interface compatible
- high performance with VPC cryptographic acceleration
# AWS Resource Access Manager (RAM)
a way to share resources with multiple accounts either within or even outside of AWS org. Utilizes AWS RAM resource shares that can control the access
