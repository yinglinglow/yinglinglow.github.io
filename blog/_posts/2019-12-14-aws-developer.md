---
layout: post
title: "AWS Developer"
date: 2019-12-14
---

here I am trying to squirm my way through another exam...! lol

wish me luck

---

update: HAH guess who passed!!!!!

can't believe it seriously lol halfway through the exam i was legit like 'i'm going to fail this so bad its not even funny'

---

__IAM__

Use access keys (User) during development, not IAM roles.

AWS Security Token Service (STS) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users that you authenticate (federated users).

To assume a role, an application calls STS:AssumeRole API

to test out custom IAM policies:
1) get context keys
2) simulate custom policy

AWS CLI: aws iam get-context-keys-for-custom-policy and aws iam get-context-keys-for-principal-policy

AWS CLI: aws iam simulate-custom-policy and aws iam simulate-principal-policy

iam cannot simulate sign in and sign up


__Elasticache__

Elasticache if database is read-heavy and not prone to frequent changing. Memcache for simple caching, Redis for advanced. Memcache supports simple data types and multithreaded performance.

Redis supports Gaming leaderboards (computationally complex). Redis is for high availability.

Lazy loading: loads data into cache only when requested. When a node fails and is replaced by a new, empty node, your application continues to function, though with increased latency.

Write-through: adds data or updates data in the cache whenever data is written to the database. Node failure means data is missing until added or updated in database.

TTL: time to live (avoids stale data)

---

__EC2__

Query instance meta data to get private IP of the instance.

running ifconfig is for linux and ipconfig for windows.

use EC2 and elastic load balancer to develop and host RESTful api (serverless: lambda and api gateway)

M5: up to 25gbps and enhanced networking support with ENA.

When you launch a new EC2 instance, the EC2 service attempts to place the instance in such a way that all of your instances are spread out across underlying hardware to minimize correlated failures. You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload. Depending on the type of workload, you can create a placement group using one of the following placement strategies:

Cluster – packs instances close together inside an Availability Zone. This strategy enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication that is typical of HPC applications.
Partition – spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions. This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka.
Spread – strictly places a small group of instances across distinct underlying hardware to reduce correlated failures.
There is no charge for creating a placement group.

As users of the new Amazon EC2 C5 and M5 instance types are noticing, Amazon EBS volumes attached to C5 and M5 instances are exposed as NVMe devices.

placement groups dun support T size!

__S3__

Cross-origin resource sharing (CORS) defines a way for client web applications that are loaded in one domain to interact with resources in a different domain. 

Use case: use JavaScript on the webpages that are stored in this bucket to be able to make authenticated GET and PUT requests against the same bucket by using the Amazon S3 API endpoint for the bucket, website.s3.amazonaws.com. A browser would normally block JavaScript from allowing those requests, but with CORS you can configure your bucket to explicitly enable cross-origin requests from website.s3-website-us-east-1.amazonaws.com.

set condition in bucket to reject non-encrypted objects: header does not have x-amz-server-side-encryption.

server access logs delivered to same bucket may increase size of bucket.

Required configurations for website hosting:
- Enabling Website Hosting
- Configuring Index Document Support
- Permissions Required for Website Access

If you notice a significant increase in the number of HTTP 503-slow down responses received for Amazon S3 PUT or DELETE object requests to a bucket that has versioning enabled, you might have one or more objects in the bucket for which there are millions of versions. 

the Amazon S3 inventory tool generates a report that provides a flat file list of the objects in a bucket. 

---

__Lambda__

Default timeout is 3 seconds.

Avoid using recursion in Lambda.

AWS Lambda: Use an alias in notification configuration, so can just update the alias to point to another PUBLISHED version. Amazon Resource Name (ARN)

Use AWS X-ray to debug lambda (also elastic beanstalk, ec2, api gateway).

By default, the X-Ray SDK records the first request each second, and five percent of any additional requests. One request per second is the reservoir, which ensures that at least one trace is recorded each second as long the service is serving requests. Five percent is the rate at which additional requests beyond the reservoir size are sampled.

AWS Lambda automatically monitors Lambda functions on your behalf, reporting metrics through Amazon CloudWatch.

For an app to call publicly available AWS services, you can use Lambda to interact with required services and expose Lambda functions through API methods in API Gateway. 

Version Control with lambda: versions are immutable. If function is qualified, you can create an alias and use aliases to split traffic to different versions.

to access resources in a VPC, lambda needs subnet IDs and Security Group IDs.

AWS Step Functions: to coordinate execution of other lambda functions.

The following best practices for implementing AWS Step Functions workflows can help you optimize the performance of your implementations.
- Use Timeouts to Avoid Stuck Executions
- Use ARNs Instead of Passing Large Payloads (store in s3)

states can have multiple incoming transitions from other states.

Lambda@Edge is an extension of AWS Lambda, a compute service that lets you execute functions that customize the content that CloudFront delivers.

Use Kinesis with AWS lambda to preprocess the data.

Selectively include libraries that are required.

Lambda ALIAS with routing-config: can dictate what percentage of incoming traffic for each version.

enable access to write to xray (not the other way around!)

default memory is 128mb - for a java based function, this is too little.

Lambda authorizers are Lambda functions that control access to REST API methods using bearer token authentication as well as information described by headers, paths, query strings, stage variables, or context variables request parameters. Lambda authorizers are used to control who can invoke REST API methods. 

__API Gateway__

API Gateway: Stage variables are name-value pairs that you can define as configuration attributes associated with a deployment stage of a REST API. They act like environment variables.

With deployment stages in API Gateway, you can manage multiple release stages for each API, such as alpha, beta, and production. Using stage variables you can configure an API deployment stage to interact with different backend endpoints. 

must create deployment before users can test the api.

API Gateway has caching capabilities to improve performance. You can throttle (ie control flow to) API Gateway to prevent attacks. If you are using javascript with multiple domains, ensure you have enabled CORS. CORS is enforced by the client. To revert changes, just rollback to a previous stage of API Gateway. HTTP 429 is a Too Many Requests error.

API Gateway supports SOAP applications but only provides passthrough. API gateway does not transform or convert the responses.

Modify frontend: Method request and response

Modify backend: Integration request and response

if XML messages: work with Request and Response Data mapping

expose GET method to let users get their details.

__Cloudwatch__

Cloudwatch basic monitoring: every 5 minutes. Detailed: 1 minute. High-resolution: 10 seconds or 30 secs.

Install cloudwatch agent on instance and send logs to central server on cloudwatch. 

key cloudwatch metrics will not give detailed levels of logs to diagnose test issues.


__Serverless Application Model__

use cloudformation package to package deployment. place function code at root level of working directory along with yaml file.

AWS Serverless Application Model (AWS SAM) to build serverless applications.

AWS::Serverless::LayerVersion creates Lambda Layered function.

AWS::Serverless::Function is for lambda.

AWS::Serverless::API is for API gateway

AWS::Serverless::Application is used for apps in S3.

use sam package and sam deploy command, not cloudformation command (lambda package and lambda deploy)

---

__RDS__

Multi-AZ is used for high availability

__DynamoDB__

Local Secondary Index: must be created when creating table, same partition key as table, different sort key.

Global Secondary Index: created anytime, different partition key, different sort key.

1 capacity unit for 1 strongly consistent read of 4KB/ 2 eventually consistent reads of 4KB per second.
1 capacity unit for 1 write of 1KB per second

ProvisionedThroughputExceededException: exceeded your maximum allowed provisioned throughput for a table or for one or more global secondary indexes.
Your request rate is too high. The AWS SDKs for DynamoDB automatically retry requests that receive this exception. Your request is eventually successful, unless your retry queue is too large to finish. Reduce the frequency of requests using Error Retries and Exponential Backoff.

Exponential backoff: is to use progressively longer waits between retries for consecutive error responses.

BatchGetItem: returns the attributes of one or more items from one or more tables. You identify requested items by primary key. A single operation can retrieve up to 16 MB of data, which can contain as many as 100 items. 

In most cases a query is preferable to a scan operation. A scan is generally less efficient & more expensive.

Use high cardinality attributes for partition keys (very unique)

DAX or DynamoDB Accelerator is highly available, in-memory cache which is optimized for DynamoDB. DAX currently only offers write-through cache. ElastiCache is supported for use with DynamoDB, however it is not specifically optimized for use with DynamoDB. 

Using DynamoDB for session storage alleviates issues that occur with session handling in a distributed web application by moving sessions off of the local file system and into a shared location.

A query finds items based on the Primary Key, whereas a Scan returns the entire contents of a table.

When using Query, or Scan, DynamoDB returns all of the item attributes by default. To get just some, rather than all of the attributes, use a Projection Expression.

Use Time to Live (TTL) to define an expiry date and time for a data record in your table.

DynamoDB Streams: use to capture a time-ordered sequence of all the activity which occurs on your DynamoDB table – e.g. insert, update, delete. 

AWS managed CMK incurs charges (not AWS owned)

Offers fully managed encryption at rest.

Reduce page size to minimise impact of scan.

Amazon DynamoDB global tables allow for multi-region multi-master table without having to build own replication solution.

Use parallel scans or queries to improve performance.

Global secondary index does not support consistent read, only supports eventual read.

To read data from a table, you use operations such as GetItem, Query, or Scan. Amazon DynamoDB returns all the item attributes by default. To get only some, rather than all of the attributes, use a projection expression.

DAX maintains an item cache to store the results from GetItem and BatchGetItem operations. The items in the cache represent eventually consistent data from DynamoDB, and are stored by their primary key values.

For strongly consistent reads, everything is forwarded from DAX to dynamodb and no cache is stored.

query: You must specify the partition key name and value as an equality condition.

for large items: You can also store the item as an object in Amazon Simple Storage Service (Amazon S3) and store the Amazon S3 object identifier in your DynamoDB item.

By default, a Query operation does not return any data on how much read capacity it consumes. However, you can specify the ReturnConsumedCapacity parameter in a Query request to obtain this information. The following are the valid settings for ReturnConsumedCapacity:

NONE — No consumed capacity data is returned. (This is the default.)
TOTAL — The response includes the aggregate number of read capacity units consumed.
INDEXES — The response shows the aggregate number of read capacity units consumed, together with the consumed capacity for each table and index that was accessed.

5xx errors: service unavailable or network issues

4xx errors: missing parameters, exceed throughput, authentication failure

---

__SQS__

SQS: usually used for messaging across distributed components of an application. use this to decouple entire system.

Cannot change existing queue to FIFO. Must choose queue type when you create it.

FIFO (First-In-First-Out) queues are designed to enhance messaging between applications when the order of operations and events is critical, or where duplicates can't be tolerated.

Not visible in queue:
set visibility timeout before processing. default is 30 seconds.


Not visible to customers:
If you create a delay queue, any messages that you send to the queue remain invisible to consumers for the duration of the delay period.

If you send a message with a 45-second timer, the message isn't visible to consumers for its first 45 seconds in the queue. 

__KMS__

Use the aws kms encrypt command to encrypt files.

Use the aws kms enable-key-rotation command to configure key rotation. 

The re-encrypt API call encrypts data on the server side with a new customer master key (CMK) without exposing the plaintext of the data on the client side.

Envelope encryption is the practice of encrypting plaintext data with a data key, and then encrypting the data key under another key. The data key is also known as the envelope key. The top-level plaintext key encryption key is known as the master key.

GenerateDataKeyWithoutPlaintext is identical to the GenerateDataKey operation except that returns only the encrypted copy of the data key. 

---

__Elastic Beanstalk__

Elastic Beanstalk: simply upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring.

An Elastic Beanstalk platform comprises an AMI configured to run a set of software that supports an application, and metadata that can include custom configuration options and default configuration option settings.

During environment creation, configuration options follow the precedence: settings applied directly to environment, saved configurations (in s3), configurations files (.ebextensions), default values.

save config files in the .ebextensions folder!

must save a cron.yml file if need to prcoess periodic background tasks.

to change instance size: change launch configuration or environment manifest file (yaml).

instance profile is used to ensure the environment can interact with other aws resources

for deploying different versions, just upload to environment. 

when changing configuration on running environment: change using AWS console

__Others__

AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources - With AWS Config you can discover existing AWS resources, export a complete inventory of your AWS resources with all configuration details, and determine how a resource was configured at any point in time. These capabilities enable compliance auditing, security analysis, resource change tracking, and troubleshooting.

AWS OpsWorks is a configuration management service that provides managed instances of Chef and Puppet. OpsWorks lets you use Chef and Puppet to automate how servers are configured, deployed, and managed across your Amazon EC2 instances or on-premises compute environments. OpsWorks is best suited for when you have multiple stacks.

If using Docker, use elastic beanstalk.

If not using Docker currently, use Packer - an open-source tool for creating machine images for many platforms, including AMIs for use with Amazon Elastic Compute Cloud (Amazon EC2).

create new environment (instead of changing configuration file) - not cost effective?   

When a configuration change requires replacing instances, there are several deployment types.

to only deploy code to new instances: immutable deployment and blue/green deployment.

Rolling: During a rolling update, capacity is only reduced by the size of a single batch, which you can configure. Elastic Beanstalk takes one batch of instances out of service, terminates them, and then launches a batch with the new configuration. After the new batch starts serving requests, Elastic Beanstalk moves on to the next batch.

Blue/Green deployment: the blue environment is the production environment that normally handles live traffic. The CI/CD pipeline architecture creates a clone (green) of the live Elastic Beanstalk environment (blue). The pipeline then swaps the URLs between the two environments.

While CodePipeline deploys application code to the original environment—and testing and maintenance take place—the temporary clone environment handles the live traffic. Once deployment to the blue environment is successful, and code review and code testing are done, the pipeline again swaps the URLs between the green and blue environments. The blue environment starts serving the live traffic again, and the pipeline terminates the green environment.



AWS Codebuild: if no access to project, edit yaml file then use start-build command.

With CodeBuild, you don’t need to provision, manage, and scale your own build servers. CodeBuild scales continuously and processes multiple builds concurrently, so your builds are not left waiting in a queue. 

CloudTrail is used to monitor API activity, not lamdba metrics.

AWS WAF is a web application firewall that helps protect your web applications and APIs from attacks by allowing you to configure rules that allow, block, or monitor (count) web requests based on customizable rules and conditions that you define.

Resource policies for API Gateway can be used to deny a specific IP address.

AWS CodeDeploy is a fully managed deployment service that automates software deployments.

specs in appspec.yaml

Application Stop -> Before Install -> After Install -> Application Start -> Validate Service

Canary10percent5minutes: shifts 10% of traffic first, and all remaining traffic after 5 minutes.

Linear10percent1minute: shifts 10% every 1 minute.

AWS Systems Manager provides a centralized store to manage your configuration data, whether plain-text data such as database strings or secrets such as passwords

Code Pipeline: entire process will stop if there is any failure in build.

CodePipeline: for managing CI/CD pipelines

CodeBuild: for managing code builds

CodeCommit: for managing source code versioning repos

The simplest way to set up connections to AWS CodeCommit repositories is to configure Git credentials for CodeCommit in the IAM console, and then use those credentials for HTTPS connections.

For cross-account: create cross account role, give the role the privileges. provide role ARN to developers.

CodeStar: for complete life cycle of project

CodeDeploy: deploy functions e.g. lambda. specify version in AppSpec file.
for rollback, have to manually add required files to instance or create a new application revision.

for real-time logs: post log data to kinesis stream for processing.

Passing stage variable to HTTP URL: can be anywhere. just need: ${stageVariables.<variable_name>}

STS token is short-lived.

aws sts decode-authorisation-message

The message is encoded because the details of the authorization status can constitute privileged information that the user who requested the operation should not see. To decode an authorization status message, a user must be granted permissions via an IAM policy to request the DecodeAuthorizationMessage (sts:DecodeAuthorizationMessage ) action.

CodePipeline

For this example, you must create an AWS Key Management Service (AWS KMS) key to use, add the key to the pipeline, and set up account policies and roles to enable cross-account access. For an AWS KMS key, you can use the key ID, the key ARN, or the alias ARN.

CloudTrail captures all API calls for IAM and AWS STS as events, including calls from the console and from API calls.

CodeDeploy with parameters stored in AWS Systems Manager Parameter store.

aws ssm get-parameters --with-decryption : allow decryption to use password in app
give permission to CodeDeploy

Long polling: waits for message or timeout values for each queue (polling multiple queues with a single thread)

Using Jenkins:
As a best practice, when you use a Jenkins build provider for your pipeline’s build or test action, install Jenkins on an Amazon EC2 instance and configure a separate EC2 instance profile. Make sure the instance profile grants Jenkins only the AWS permissions required to perform tasks for your project, such as retrieving files from Amazon S3.

ECS Container: when stopped, instance status is still ACTIVE but connection is FALSE.

CORS: OPTIONS method is only for response 200. for others, need to manually configure.

AWS Trusted Advisor: for recommendations

AWS Data Pipeline to archive your web server's logs to Amazon Simple Storage Service (Amazon S3) each day and then run a weekly Amazon EMR (Amazon EMR) cluster over those logs to generate traffic reports. AWS Data Pipeline schedules the daily tasks to copy data and the weekly task to launch the Amazon EMR cluster. 

Amazon Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS. 

---

__Kinesis__

Kinesis: enable server side encryption for Kinesis streams, so that all data is encrypted at rest. not SDK or client side because too much effort.

Kinesis - cannot order across shards, only order within a shard.

When you configure a Kinesis data stream as the data source of a Kinesis Data Firehose delivery stream, Kinesis Data Firehose no longer stores the data at rest. Instead, the data is stored in the data stream.

When you send data from your data producers to your data stream, Kinesis Data Streams encrypts your data using an AWS Key Management Service (AWS KMS) key before storing the data at rest. 

SSL: for in transit

A consumer is an application that processes all data from a Kinesis data stream. When a consumer uses enhanced fan-out, it gets its own 2 MiB/sec allotment of read throughput, allowing multiple consumers to read data from the same stream in parallel, without contending for read throughput with other consumers.

With Amazon Kinesis Data Analytics for SQL Applications, you can process and analyze streaming data using standard SQL. The service enables you to quickly author and run powerful SQL code against streaming sources to perform time series analytics, feed real-time dashboards, and create real-time metrics.


__Cognito__

Cognito Streams: access to data in Cognito.

Amazon Cognito provides authentication, authorization, and user management for your web and mobile apps. 

AWS Cognito for external identities like Facebook or Google.

AssumeRolewithWebIdentity

SAML federation identities for external directories

Amazon Cognito Events allows you to execute an AWS Lambda function in response to important events in Amazon Cognito. Amazon Cognito raises the Sync Trigger event when a dataset is synchronized. You can use the Sync Trigger event to take an action when a user updates data. 

Retry sync operation when Lambda Throttled Exception.

Amazon Cognito - access rules assessed in sequential order and first matching rule used, unless CustomRoleArn overrides.

Use existing or create new kinesis stream and create IAM role to allow Amazon Cognito Stream to publish to those streams.

__Cloudformation__

Cloudformation: break the templates into smaller templates when designing

AWS CloudFormation StackSets extends the functionality of stacks by enabling you to create, update, or delete stacks across multiple accounts and regions with a single operation. 

cloudformation can deploy codedeploy automatically.

valid types for parameters: string, number, list

need to ensure all items in buckets are deleted before bucket can be deleted.

install and configure software applications by using cfn-init helper script. describe rather than script using AWS::CloudFormation::Init


__Cors__

Cross Origin:
A cross-origin HTTP request is one that is made to:

A different domain (for example, from example.com to amazondomains.com)
A different subdomain (for example, from example.com to petstore.example.com)
A different port (for example, from example.com to example.com:10777)
A different protocol (for example, from https://example.com to http://example.com)

To support CORS, therefore, a REST API resource needs to implement an OPTIONS method that can respond to the OPTIONS preflight request with at least the following response headers mandated by the Fetch standard:
Access-Control-Allow-Methods
Access-Control-Allow-Headers
Access-Control-Allow-Origin

enable CORS on the API Gateway level (not lambda level) so your API responds to the OPTIONS preflight request with the above headers.