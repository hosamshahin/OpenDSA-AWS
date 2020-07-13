
# OpenDSA-AWS

- [Table of Contents](#opendsa-aws)
  * [Introduction](#introduction)
  * [Prerequisites](#prerequisites)
  * [Solution Architecture](#solution-architecture)
  * [Provisinging Steps](#provisinging-steps)
  * [Post Provisinging Validation](#post-provisinging-validation)
  * [Stack Deletion](#stack-deletion)
  * [AWS Costs](#aws-costs)
  * [About Let's Encrypt](#about-let-s-encrypt)
  * [To-Do list](#to-do-list)
  * [Contribution](#contribution)

## Introduction
OpenDSA-AWS is a Cloud Formation template that automates OpenDSA infrastructure provisioning and application deployment on AWS.

## Prerequisites
To be able to deploy your instance of OpenDSA system you need to do the following:

* [Create and Activate a new AWS account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/#:~:text=Open%20the%20Amazon%20Web%20Services,Create%20a%20new%20AWS%20account)
* [Secure your AWS account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)
* [Register a domain using AWS Route53 service](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html)
* [Create a key pair using EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)

## Solution Architecture

After OpenDSA-AWS template execution completes it will create the following resources:

* [Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) (VPC)
* [Public Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)
* [Internet Gateway]([https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html))
* [Elastic IP Address](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) (EIP)
* [Elastic Cloud Compute](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) (EC2) Instance
* [Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
* [Route53](https://aws.amazon.com/route53/) alias record for OpenDSA domain name.
* OpenDSA Domain certificate generate by [Let's Encrypt](https://letsencrypt.org/) certificate authority.

For proper working you must provide the following resources:
* Hosted Zone configured in AWS Route53.

## Provisinging steps

* Log in to your AWS account.
| AWS Region | Short name | |
| -- | -- | -- |
| US East (Ohio) | us-east-2 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| US East (N. Virginia) | us-east-1 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| US West (Oregon) | us-west-2 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| US West (N. California) | us-west-1 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| Canada (Central) | ca-central-1 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ca-central-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| EU (Paris) | eu-west-3 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-3#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| EU (London) | eu-west-2 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| EU (Ireland) | eu-west-1 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| EU (Frankfurt) | eu-central-1 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| Asia Pacific (Seoul) | ap-northeast-2 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-2#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| Asia Pacific (Tokyo) | ap-northeast-1 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| Asia Pacific (Sydney) | ap-southeast-2 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| Asia Pacific (Singapore) | ap-southeast-1 | [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| Asia Pacific (Mumbai) | ap-south-1 |  [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-south-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
| South America (São Paulo) | sa-east-1 |  [![cloudformation-launch-button](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=sa-east-1#/stacks/new?stackName=opendsa&templateURL=https://opendsa.s3.amazonaws.com/opendsa-aws.yaml) |
* Fill in the template parameters
    - Email: Provide your institution email address to get important notifications regarding the infrastructure resources staus and certificate expiry date.
    - DomainName: The Domain you registered with AWS Route53 service
    - AdditionalDomainName: The same as the DomainName, you may need to change this parameter in case you hit let's encrypt certificates identical limit. Please see About Let's encrypt for more details.
    - HostedZoneId: The Route53 hosted zone Id used to route the traffic for the new domain
    - KeyName: The key pair name created earlier
    - DBName: OpenDSA database name (default opendsa)
    - DBUser: OpenDSA database user (default opendsa)
    - DBPassword: OpenDSA database password (default opendsa. Change to a strong password)
    - DBRootPassword: MySQL root password (default root. Change to even stronger password)
    - InstanceType: The EC2 instance type (default t2.small)
    - SSHLocation: The IP address range that can be used to SSH to the EC2 instances, we recommend to narrow that range to include only your computer public IP address.
    - VpcCIDR: VPC IP range (you don't need to change this value)

The provisioning process might take around 10 mins. Wait until the cloud formation stack status change to "CREATE_COMLETE".

## Post Provisinging validation

After the stack creation, you can navigate to `https://DomainName` to check that OpenDSA application is loading correctly. To start using your instance of OpenDSA to create Books and generate courses in Canvas LMS do the following:

1. Sign up to OpenDSA application using the same email you are using with your Canvas LMS instance.
2. Log in to OpenDSA using the admin user `admin@opendsa.org`, password `adminadmin`
3. Navigate to the admin area and open the `users` page, edit your user to make it admin.
4. Log in back to OpenDSA using your user and delete the temp admin user admin@opendsa.org.
5. In the admin area make sure you have the correct setup for Terms, Organizations, Courses, and LMS accesses.
6. Go to [instructor guides](https://opendsa-server.cs.vt.edu/home/guide) for detailed instructions on setting up an OpenDSA eTextbook instance for use within a Canvas course.

## Stack Deletion
**WARNING** deleting the CloudFormatin stack will delete all the resources listed above including the EC2 instance which has all the generated books and MySQL database which includes students' performance data. Deleting the stack will make all the links in the Canvas course invalid. However, you will still have the students scores already posted to Canvas grade book.
We recommend deleting the stack right after you are done with the semester to avoid paying for unused resources.


## AWS Costs
The creation of these AWS resources does not incur costs. However, you will incur the costs once you have the stack in CREATE_COMPLETE status and the EC2 instance is up and running.

### ToDos

- [ ] More about instance sizes, pricing, Savings Plans, and flexible pricing model.
- [ ] More about AWS support for [higher education program](https://aws.amazon.com/education/higher-ed/

## About Let's encrypt

[Let’s Encrypt](https://letsencrypt.org/) is a free, automated, and open certificate authority (CA), run for the public’s benefit. It is a service provided by the [Internet Security Research Group (ISRG)]([https://www.abetterinternet.org/](https://www.abetterinternet.org/)). OpenDSA template automates the certificate issuance using Let's Encrypt service. The service is designed to automatically renew your certificate every 90 days. It is important to provide your email in the template parameter to get important notification from Let's encrypt about certificate renewal and if you need to take manual action in case auto-renewal failed.

let's encrypt provides [rate limits](https://letsencrypt.org/docs/rate-limits/). The main limit is *Certificates per Registered Domain* (50 per week). The service also restricts certificate issuance to max 5 identical certs per week. Every time you spin up OpenDSA stack a certificate is created inside the EC2 instance and when you delete the stack the certificate got deleted with the instance. If you hit the hard limit of 5 identical certs, the template offers the `AdditionalDomainName` parameter to overcome this limit. So if your domain is `opendsa.net` and you hit the 5 certs identical limit, all you need to do is to change the `AdditionalDomainName` by adding a subdomain e.g. `odsa.opendsa.net` and a new certificate will be generated.

You can get a list of certificates issued for your registered domain by searching on [crt.sh](https://crt.sh/) , which uses the public [Certificate Transparency logs](https://www.certificate-transparency.org/).

## To-Do list
- [ ] Autoscale EC2 instance volume when the size hits a threshold.
- [ ] Building a fault-tolerant architecture by regularly taking backups and automate the restore in case of failure.
- [ ] Implement a high available solution by deploying multiple EC2 instances behind a load balancer and move the database out of the instance to the RDS service.
- [ ] Push clickstreams to a centralized store like S3 and feed the data to Caliper analytics.
- [ ] Enhance system notifications and alarms in some cases like unusual resources high usage.

## Contribution
If you find a bug in the template or you want to contribute to the project, please fork the repo, create a new branch, fix the bug or implement the improvement and submit a pull request.
