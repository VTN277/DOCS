## General AWS DevOps Questions

### What is DevOps?

DevOps is a set of practices that integrates software development (Dev)
and IT operations (Ops) to shorten the development lifecycle and deliver
features, fixes, and updates frequently in close alignment with business
objectives.

### What are the benefits of using AWS for DevOps?

AWS provides flexible services like Elastic Compute Cloud (EC2), Elastic
Container Service (ECS), and Elastic Beanstalk, which help automate and
scale development and deployment pipelines. Features include
scalability, automation, CI/CD, Infrastructure as Code (IaC), and
monitoring tools.

### What is Infrastructure as Code (IaC) in AWS?

IaC refers to managing and provisioning infrastructure through code
instead of manual processes. In AWS, you can implement IaC using AWS
CloudFormation and AWS CDK (Cloud Development Kit).

Explain the difference between DevOps and Agile.

Agile is a methodology that focuses on iterative development, whereas
DevOps is a practice that bridges the gap between development and
operations to ensure faster and more reliable software delivery.

What are some popular AWS DevOps tools?

AWS CodePipeline (CI/CD) AWS CodeBuild (build automation) AWS CodeDeploy
(deployment automation) AWS CloudFormation (IaC) Amazon ECS/EKS
(container orchestration) CI/CD Pipeline Questions What is a CI/CD
pipeline?

A CI/CD pipeline automates the steps in software development, from
integration, testing, deployment, to delivery, ensuring continuous
improvement and delivery of applications.

How would you implement a CI/CD pipeline in AWS?

You can use AWS CodePipeline to create a CI/CD pipeline. Combine
CodeCommit (source control), CodeBuild (build), and CodeDeploy
(deployment) for a complete pipeline.

Explain AWS CodePipeline.

AWS CodePipeline is a continuous integration and continuous delivery
service that helps automate build, test, and deploy phases of your
release process every time there is a code change.

What is AWS CodeBuild?

AWS CodeBuild is a fully managed build service that compiles your source
code, runs tests, and produces artifacts that are ready for deployment.

What is AWS CodeDeploy?

AWS CodeDeploy automates code deployments to any instance, including
Amazon EC2 instances and on-premises servers.

Containerization and Orchestration What are containers?

Containers are lightweight, standalone executable packages that include
everything needed to run an application, including code, runtime,
libraries, and system dependencies.

What is the difference between Docker and a Virtual Machine (VM)?

Docker containers virtualize at the OS level, while VMs virtualize at
the hardware level. Containers are more lightweight, sharing the host OS
kernel, whereas each VM runs a full guest OS.

How do you orchestrate containers in AWS?

Use Amazon ECS (Elastic Container Service) or Amazon EKS (Elastic
Kubernetes Service) to manage and orchestrate containerized
applications.

What is Amazon ECS?

Amazon ECS is a fully managed container orchestration service that
allows you to run, stop, and manage containers in a cluster.

What is Amazon EKS?

Amazon EKS is a managed service that makes it easy to run Kubernetes on
AWS without needing to install and operate your own Kubernetes control
plane.

AWS Elastic Beanstalk and Lambda What is AWS Elastic Beanstalk?

AWS Elastic Beanstalk is a platform-as-a-service (PaaS) that allows you
to deploy and manage applications in various languages like Java,
Python, Ruby, etc., without worrying about infrastructure.

How do you deploy an application using Elastic Beanstalk?

You can deploy an application using Elastic Beanstalk through its
management console, CLI, or CI/CD pipeline. Upload your application and
specify the environment configuration.

What is AWS Lambda?

AWS Lambda is a serverless compute service that runs your code in
response to events and automatically manages the compute resources for
you.

How does AWS Lambda integrate with CI/CD?

You can deploy AWS Lambda functions as part of a CI/CD pipeline using
AWS CodePipeline, CodeBuild, and CodeDeploy. Lambda functions can also
be triggered by changes in repositories like CodeCommit or GitHub.

What are the limitations of AWS Lambda?

AWS Lambda has limits such as a maximum of 15 minutes execution time, 10
GB of memory, and limited support for certain libraries and
dependencies.

AWS EC2 and Load Balancing What is Amazon EC2?

Amazon Elastic Compute Cloud (EC2) is a web service that provides
resizable compute capacity in the cloud.

What is an EC2 instance?

An EC2 instance is a virtual server in AWS that can be used to run
applications on the AWS cloud.

How can you automatically scale EC2 instances?

Use Auto Scaling Groups and AWS CloudWatch to monitor your instances and
automatically scale the number of instances based on predefined
conditions, such as CPU utilization.

What is Elastic Load Balancer (ELB)?

ELB is a service that automatically distributes incoming traffic across
multiple EC2 instances, containers, or IP addresses to ensure high
availability.

What are the types of load balancers in AWS?

Application Load Balancer (ALB) Network Load Balancer (NLB) Gateway Load
Balancer (GLB) AWS CloudFormation and Automation What is AWS
CloudFormation?

AWS CloudFormation is a service that allows you to model, provision, and
manage AWS infrastructure as code by using templates.

How does CloudFormation help with DevOps?

It enables automated provisioning and management of infrastructure,
reducing the manual effort and errors while ensuring consistent
environments across development, staging, and production.

What is a CloudFormation stack?

A CloudFormation stack is a collection of AWS resources that you can
manage as a single unit using CloudFormation.

How do you update a CloudFormation stack?

Update the CloudFormation stack by modifying the template and applying
the changes. AWS will automatically determine the changes required and
apply them.

Can you roll back CloudFormation changes?

Yes, CloudFormation supports rollback on failure, where if any resource
creation or update fails, it will automatically roll back all changes to
the last known good state.

AWS Monitoring and Logging What is AWS CloudWatch?

AWS CloudWatch is a monitoring and management service that provides data
and actionable insights for AWS resources, applications, and services.

What are CloudWatch Alarms?

CloudWatch Alarms watch a metric over time and perform an action based
on predefined thresholds, such as sending notifications or scaling EC2
instances.

How does AWS CloudTrail help in monitoring?

AWS CloudTrail logs API calls made in your account, providing visibility
into user activity and changes to resources, which helps in auditing and
compliance.

What is AWS X-Ray?

AWS X-Ray helps developers analyze and debug distributed applications,
such as those built using a microservices architecture, by tracing
requests and monitoring their performance.

How do you integrate CloudWatch with a CI/CD pipeline?

Use CloudWatch metrics and alarms to trigger actions like automated
deployments or rollbacks in a CI/CD pipeline.

AWS IAM and Security What is AWS IAM?

AWS Identity and Access Management (IAM) is a service that allows you to
manage access to AWS services and resources securely.

What are IAM roles?

IAM roles are used to delegate access to users or services, allowing
them to interact with AWS services without needing long-term
credentials.

What is an IAM policy?

An IAM policy is a JSON document that defines permissions and controls
what actions are allowed or denied for AWS resources.

How can you secure your CI/CD pipeline in AWS?

Use IAM roles and policies to ensure that only authorized users and
services can access your CI/CD pipeline. Implement encryption for
sensitive data and use logging to monitor access.

What is AWS KMS?

AWS Key Management Service (KMS) is a managed service that allows you to
create and manage encryption keys and control the use of encryption
across AWS services.

AWS Security and Best Practices What is AWS Shield?

AWS Shield is a managed Distributed Denial of Service (DDoS) protection
service that safeguards applications running on AWS. AWS Shield comes in
two tiers: Standard and Advanced.

What is AWS WAF?

AWS Web Application Firewall (WAF) helps protect your web applications
from common web exploits like SQL injection and cross-site scripting
(XSS) by allowing you to define rules that allow or block specific
requests.

How do you secure data in transit and at rest in AWS?

For data in transit, use SSL/TLS encryption. For data at rest, AWS
provides services like S3 server-side encryption (SSE), AWS KMS, and EBS
encryption.

How can you implement MFA in AWS?

AWS provides Multi-Factor Authentication (MFA) to add an extra layer of
security by requiring two forms of identification to access AWS
services. It can be enabled via IAM for user accounts.

What is AWS Secrets Manager?

AWS Secrets Manager helps you securely store and manage access to
credentials, API keys, and other secrets necessary for accessing AWS
services or third-party applications.

Version Control and AWS CodeCommit What is AWS CodeCommit?

AWS CodeCommit is a fully managed source control service that allows you
to privately store and manage Git repositories in the cloud.

How does AWS CodeCommit integrate with CI/CD pipelines?

AWS CodeCommit integrates seamlessly with AWS CodePipeline, triggering
builds and deployments when changes are committed to the repository.

What is the difference between CodeCommit and GitHub?

Both are Git-based version control systems. CodeCommit is fully managed
by AWS with tighter integration into AWS services, while GitHub is an
external Git hosting service with broader community features.

How do you automate code deployment from CodeCommit to EC2 instances?

You can automate code deployment using CodePipeline, CodeDeploy, and
CodeCommit, where CodePipeline triggers a deployment when a commit is
pushed, and CodeDeploy deploys the code to EC2 instances.

Can you integrate CodeCommit with external CI/CD tools like Jenkins?

Yes, CodeCommit can be integrated with Jenkins using AWS SDKs and APIs.
Jenkins can poll the repository and trigger builds based on changes.

AWS Auto Scaling What is AWS Auto Scaling?

AWS Auto Scaling automatically adjusts the capacity of your resources to
maintain performance and availability at the lowest possible cost.

How does Auto Scaling work with EC2?

You create Auto Scaling groups, define scaling policies (such as scaling
based on CPU usage), and AWS will automatically increase or decrease the
number of EC2 instances as needed.

What is the difference between vertical and horizontal scaling?

Vertical scaling refers to increasing the size (e.g., CPU, memory) of an
instance. Horizontal scaling refers to adding more instances to handle
increased traffic. What is a launch configuration in Auto Scaling?

A launch configuration is a template that an Auto Scaling group uses to
launch EC2 instances, specifying the instance type, AMI, key pair, and
security groups.

How do you set up a highly available system using Auto Scaling and ELB?

Create an Auto Scaling group spread across multiple Availability Zones
(AZs), associate it with an Elastic Load Balancer (ELB), and configure
health checks to ensure traffic is only routed to healthy instances.

AWS S3 and Storage What is Amazon S3?

Amazon Simple Storage Service (S3) is an object storage service that
provides scalability, security, and performance for storing any amount
of data from anywhere.

What are the different storage classes in S3?

S3 Standard S3 Intelligent-Tiering S3 Standard-IA (Infrequent Access) S3
One Zone-IA S3 Glacier S3 Glacier Deep Archive What is versioning in S3?

S3 versioning allows you to keep multiple versions of an object in the
same bucket, protecting against accidental deletions or overwrites.

How does S3 encryption work?

S3 supports server-side encryption (SSE) with S3-Managed Keys (SSE-S3),
AWS KMS-Managed Keys (SSE-KMS), and customer-provided keys (SSE-C). It
also supports client-side encryption where the encryption is handled
outside of S3.

How do you manage permissions for S3 buckets?

Permissions for S3 buckets can be managed using bucket policies, ACLs
(Access Control Lists), and IAM policies.

AWS Networking What is a VPC in AWS?

Amazon Virtual Private Cloud (VPC) allows you to create a private,
isolated section of the AWS cloud where you can launch AWS resources in
a virtual network that you define.

What is a subnet in VPC?

A subnet is a range of IP addresses in your VPC. You can launch AWS
resources into a subnet. Subnets can be public (access to the internet)
or private (no internet access).

What is an Internet Gateway (IGW)?

An IGW is a horizontally scaled, redundant, and highly available VPC
component that allows communication between instances in your VPC and
the internet.

What is a NAT Gateway?

A NAT Gateway allows instances in a private subnet to access the
internet while preventing inbound traffic from the internet.

What are security groups and NACLs in AWS?

Security Groups act as a virtual firewall at the instance level,
controlling inbound and outbound traffic. Network Access Control Lists
(NACLs) provide stateless traffic filtering at the subnet level. What is
VPC Peering?

VPC Peering is a networking connection between two VPCs that allows you
to route traffic between them privately using IPv4 or IPv6 addresses.

What is AWS Direct Connect?

AWS Direct Connect is a dedicated network connection from your premises
to AWS, allowing for faster and more secure data transfer.

How do you achieve high availability in a VPC?

Use multiple Availability Zones (AZs) within a region, set up load
balancers, and design failover mechanisms for critical resources.

What is Amazon Route 53?

Amazon Route 53 is a scalable DNS and domain name registration service
that routes end-user requests to AWS services or other internet
resources.

How do you implement a multi-region architecture in AWS?

Use AWS services like Route 53 for DNS failover, replicate data across
regions using services like S3 Cross-Region Replication, and leverage
multi-region databases like Amazon DynamoDB Global Tables.

AWS Elastic Container Service (ECS) What is Amazon ECS?

Amazon Elastic Container Service (ECS) is a fully managed container
orchestration service that helps you run and scale containerized
applications on AWS.

What is the difference between ECS and EKS?

ECS is a native AWS service for container orchestration, while EKS is a
fully managed Kubernetes service that provides a Kubernetes control
plane in AWS.

What is an ECS Task?

An ECS task is a running instance of a task definition, which includes
the Docker container configuration, networking, and other settings for
your containers.

What is a Fargate launch type in ECS?

AWS Fargate is a serverless compute engine for containers that allows
you to run containers without having to manage the underlying
infrastructure.

How does ECS integrate with IAM?

ECS allows you to assign IAM roles to tasks, enabling your containers to
access AWS resources securely.

AWS Elastic Kubernetes Service (EKS) What is Amazon EKS?

Amazon Elastic Kubernetes Service (EKS) is a fully managed service that
allows you to run Kubernetes on AWS without managing the Kubernetes
control plane.

What is the difference between EKS and Kubernetes?

EKS is a managed service that handles the Kubernetes control plane for
you, while Kubernetes requires you to manually set up and manage the
control plane. EKS also provides integration with AWS services like IAM
and VPC.

How do you deploy a Kubernetes application to EKS?

You can deploy Kubernetes applications to EKS using kubectl. First, set
up your EKS cluster, configure kubectl to use it, and then apply your
Kubernetes manifests or Helm charts.

How does EKS integrate with IAM?

EKS integrates with IAM to control access to the Kubernetes control
plane. IAM roles can also be mapped to Kubernetes service accounts to
manage permissions for your workloads.

What is eksctl?

eksctl is a command-line tool that simplifies the creation, management,
and deletion of Amazon EKS clusters. It abstracts away the complexity of
setting up EKS clusters by providing a simple interface.

AWS Lambda and Serverless What are the key use cases for AWS Lambda?

Running event-driven workloads Processing data streams (e.g., AWS
Kinesis, DynamoDB streams) Building APIs with AWS API Gateway Automating
infrastructure management tasks How does AWS Lambda handle scaling?

AWS Lambda automatically scales based on the number of incoming requests
or events, creating as many function instances as needed to handle the
load.

What are cold starts in AWS Lambda?

A cold start occurs when a new Lambda instance is initialized due to an
increase in load or when the function hasn\'t been invoked for some
time. This can cause a slight delay in execution.

How can you reduce cold start latency in Lambda?

You can reduce cold starts by optimizing your function's memory
allocation, keeping the function size small, using provisioned
concurrency, or keeping the function "warm" by invoking it periodically.

What is provisioned concurrency in Lambda?

Provisioned concurrency ensures that a set number of Lambda instances
are always warm and ready to handle requests, reducing the cold start
problem.

AWS Monitoring and Troubleshooting How do you monitor AWS Lambda
functions?

Use AWS CloudWatch to monitor Lambda functions. CloudWatch provides
metrics such as invocation count, duration, error count, and throttles.

What is AWS CloudTrail, and how does it help with auditing?

AWS CloudTrail logs all API calls made within your AWS account, helping
with auditing, compliance, and security monitoring.

How do you troubleshoot failed deployments in AWS CodeDeploy?

You can troubleshoot failed deployments by checking the deployment logs
in AWS CodeDeploy, reviewing application logs in CloudWatch, and
verifying your deployment configurations.

What is AWS Trusted Advisor?

AWS Trusted Advisor is an online resource that provides real-time
guidance to help you follow AWS best practices for cost optimization,
performance, security, and fault tolerance.

What are CloudWatch Logs, and how do they help with monitoring?

CloudWatch Logs capture log data from AWS services and applications.
They help monitor, troubleshoot, and analyze application and system logs
in real-time.

AWS Elastic Load Balancer (ELB) What is the difference between
Application Load Balancer (ALB) and Network Load Balancer (NLB)?

ALB operates at the application layer (Layer 7) and is used for routing
HTTP/HTTPS traffic. NLB operates at the transport layer (Layer 4) and is
used for ultra-low latency and high-throughput connections, handling
TCP/UDP traffic. How does an Application Load Balancer (ALB) handle
routing?

ALB routes incoming traffic based on rules such as URL paths, hostnames,
and HTTP headers. It provides advanced features like sticky sessions and
SSL termination.

What is SSL termination, and how does ELB handle it?

SSL termination refers to the process of decrypting SSL traffic at the
load balancer level instead of the backend instances. ELB can manage SSL
certificates and terminate SSL connections for you.

How do you configure sticky sessions with an Application Load Balancer?

You can enable sticky sessions (session affinity) for an ALB by
configuring a target group to bind a user's session to a specific
backend instance.

What is Cross-Zone Load Balancing?

Cross-Zone Load Balancing ensures that incoming traffic is distributed
evenly across all instances registered to the load balancer, regardless
of which availability zone they are in.

AWS Elastic Block Store (EBS) and Databases What is Amazon EBS?

Amazon Elastic Block Store (EBS) provides persistent block storage
volumes for use with EC2 instances. EBS volumes can be used as primary
storage for data that requires frequent updates, like databases or log
files.

What are the different types of EBS volumes?

General Purpose SSD (gp3/gp2) Provisioned IOPS SSD (io2/io1) Throughput
Optimized HDD (st1) Cold HDD (sc1) What is Amazon RDS?

Amazon Relational Database Service (RDS) is a managed service that makes
it easy to set up, operate, and scale relational databases in the cloud.
It supports multiple engines like MySQL, PostgreSQL, MariaDB, and
Oracle.

What is Amazon Aurora?

Amazon Aurora is a fully managed MySQL- and PostgreSQL-compatible
relational database that offers performance improvements and high
availability.

What is Amazon DynamoDB?

Amazon DynamoDB is a fully managed NoSQL database service that provides
fast and predictable performance with seamless scalability, designed for
high-availability applications.
