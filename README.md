# Guide to AWS

## Table of Contents
- [Identity and Access Management](#aws-iam)
- [Elastic Compute Cloud](#aws-ec2)
- [Elastic Block Store](#aws-ebs)
    1. [Volume types](#ebs-volume-types)
    2. [Multi Attach](#ebs-multi-attach)
- [Amazon Machine Image](#aws-ami)
- [Elastic File System](#aws-efs)
- [Elastic Load Balancing](#aws-elb)
    1. [Application Load Balancing](#aws-alb)
    2. [Network Load Balancing](#aws-nlb)
    3. [Gateway Load Balancing](#aws-gwlb)
    4. [Sticky Sessions](#aws-elb-sticky-sessions)
    5. [Cross-Zone Load Balancing](#aws-elb-cross-zone-load-balancing)
    6. [SSL/TLS Certificates](aws-elb-ssl-certificates)
    7. [Connection draining](#aws-elb-connection-draining)
    8. [Auto scaling groups](#aws-elb-auto-scaling-groups)
    9. [Scaling policies](#aws-elb-auto-scaling-groups-scaling-policies)
    10. [Instance refresh](#aws-elb-auto-scaling-groups-instance-refresh)
- [Relational Database Service](aws-rds)
    1. [Read Replicas](#aws-rds-read-replicas)
    2. [Proxy](#aws-rds-proxy)
- [Aurora](#aws-aurora)
- [ElastiCache](#aws-elasticache)
- [MemoryDB for Redis](#aws-memorydb-for-redis)
<a name="aws-iam"></a>
### AWS Identity and Access Management (IAM)

#### Overview
AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. With IAM, you can centrally manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.

#### Use Cases
- Shared access to your AWS account: You can grant other people permission to administer and use resources in your AWS account without having to share your password or access key.
- Granular permissions: You can grant different permissions to different people for different resources.
- Secure access to AWS resources for applications that run on Amazon EC2: You can use IAM features to securely provide credentials for applications that run on EC2 instances. These credentials provide permissions for your application to access other AWS resources.

#### Tips
- Don't use the root user for everyday tasks: When you create an AWS account, you begin with one sign-in identity that has complete access to all AWS services and resources in the account. This identity is called the AWS account root user and is accessed by signing in with the email address and password that you used to create the account. We strongly recommend that you don't use the root user for your everyday tasks.
- Use multi-factor authentication (MFA): You can add two-factor authentication to your account and to individual users for extra security.

For more information about IAM, you can refer to the [official AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)


#### User Groups
An IAM user group is a collection of IAM users managed as a unit. You can use user groups to specify permissions for a collection of users, which can make it easier to manage the permissions for those users.

![](https://i.imgur.com/bCEELJm.png)

#### Users
An IAM user is an identity within your AWS account that has specific permissions for a single person or application. You can create IAM users and assign them individual security credentials (such as access keys or multi-factor authentication devices) or request temporary security credentials to provide users with access to AWS services and resources. 

![](https://i.imgur.com/Ejrs4cp.png)

#### Roles
An IAM role is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. You can use roles to delegate access to users, applications, or services that don't normally have access to your AWS resources. 

![](https://i.imgur.com/jiGvOYQ.png)

#### Policies
A policy is an object in AWS that, when associated with an identity or resource, defines their permissions. Policies determine what actions a user, role, or member of a user group can perform, on which AWS resources, and under what conditions.

For more information about IAM, you can refer to the [official AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

![](https://i.imgur.com/QBWxQBZ.png)

<a name="aws-ec2"></a>
## Amazon Elastic Compute Cloud (EC2)
 
#### Overview
Amazon Elastic Compute Cloud (EC2) is a web service that provides resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.

#### Use Cases
- Hosting web applications
- Running batch processing and big data analytics workloads
- Building and testing enterprise applications
- Running high-performance computing workloads

#### Tips
- Implement the least permissive rules for your security group.
- Regularly patch, update, and secure the operating system and applications on your instance.
- Use separate Amazon EBS volumes for the operating system versus your data for data persistence. Ensure that the volume with your data persists after instance termination.
- Use instance metadata and custom resource tags to track and identify your AWS resources.
- View your current limits for Amazon EC2. Plan to request any limit increases in advance of the time that you’ll need them.

#### Documentation
You can find more information about EC2 in the [official AWS documentation](https://aws.amazon.com/ec2/).

#### Creating an EC2 instance 
- You can specify the hardware requirements and then launch the instance.

![](https://i.imgur.com/IrTLbTr.png)

##### Displaying dummy data for instance
```
#!/bin/bash
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello world from $(hostname -f)</h1>" > /var/www/html/index.html
```

#### Connecting to an EC2 instance using SSH

To connect to an EC2 instance from Windows 11 using OpenSSH, you need to have OpenSSH installed on your system.

Once you have OpenSSH installed, you can use the `ssh` command in PowerShell or the Command Prompt to connect to your EC2 instance. You'll need to specify the path and file name of the private key (`.pem`), the user name for your instance (e.g., `ec2-user`, `ubuntu`, etc.), and the public DNS name or IPv6 address for your instance.

Here's an example of the `ssh` command: `ssh -i "C:\Path\To\Your\Key.pem" ec2-user@your-ec2-instance-public-dns`

For more detailed instructions, you can refer to the [official AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/openssh.html)

<a name="aws-ebs"></a>
### Amazon Elastic Block Store (EBS) Volumes

#### Overview
An Amazon EBS volume is a durable, block-level storage device that you can attach to your EC2 instances. After you attach a volume to an instance, you can use it as you would use a physical hard drive. EBS volumes are flexible and can be dynamically increased in size, modified for provisioned IOPS capacity, and changed in volume type on live production volumes.

#### Use Cases
- Primary storage for data that requires frequent updates: You can use EBS volumes as primary storage for data that requires frequent updates, such as the system drive for an instance or storage for a database application.
- Throughput-intensive applications: You can use EBS volumes for throughput-intensive applications that perform continuous disk scans.

#### Tips
- Choose the right volume type: Amazon EBS provides several volume types that differ in performance characteristics and price, allowing you to tailor your storage performance and cost to the needs of your applications. For example, General Purpose SSD (gp2 and gp3) volumes balance price and performance for a wide variety of transactional workloads, while Provisioned IOPS SSD (io1 and io2) volumes are designed to meet the needs of I/O-intensive workloads that are sensitive to storage performance and consistency.
- Monitor your EBS volumes: It is important to monitor your EBS volumes to ensure that they are performing as expected. Common factors impacting Amazon EBS performance include a lack of free storage space, volume-type performance limits, and latency.

For more information about EBS volumes, you can refer to the [official AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes.html)

#### Volumes

![](https://i.imgur.com/qchPRzf.png)

* You can attach, detach volume from an EC2 instance

![](https://i.imgur.com/xV1THSy.png)

* You can also create snapshots of the volume. It is not necessary to detach volume while performing a snapshot, but recommended.

#### Snapshots
* Can copy snapshots across AZ or region.
* Snapshots Archive: lets you archive a snapshot, making it 75% cheaper to store. However, it takes 24 to 72 hours for restoring the archive.
* Recycle bin: there is also a recycle bin for EBS Snapshots. You can set up retention plans for deleted snapshots to recover them before a specified time.
* Fast Snapshot Restore (FSR): forces a full initialization of snapshot to have 0 latency on first use. Useful if files are big. This feature can be expensive. 

##### Creating a snapshot from a volume

![](https://i.imgur.com/DrBs1G3.png)

##### Moving a snapshot to a different region

![](https://i.imgur.com/jq2TZql.png)

##### Creating a volume from snapshot

![](https://i.imgur.com/5pwAGiB.png)

##### Creating a retention rule
1. Keeping deleted EBS Snapshots in the recycle bin for a day

![](https://i.imgur.com/KyB3vjh.png)

2. Deleting snapshot 

![](https://i.imgur.com/YOUNiAC.png)

3. Resources in recycle bin can be recovered

![](https://i.imgur.com/7VJtdeA.png)

<a name=“ebs-volume-types”></a>
### Amazon EBS Volume Types

#### Overview
Amazon Elastic Block Store (EBS) provides block-level storage volumes for use with Amazon EC2 instances. EBS volumes are highly available and reliable storage volumes that can be attached to any running instance in the same Availability Zone. EBS volumes come in several types, each optimized for a particular use case. These include General Purpose SSD (gp2), Provisioned IOPS SSD (io1 and io2), Throughput Optimized HDD (st1), and Cold HDD (sc1).

##### Use Cases
1. General Purpose SSD (gp2): This is the default EBS volume type for Amazon EC2 instances. It is suitable for a broad range of workloads, including small to medium-sized databases, development and test environments, and boot volumes.
2. Provisioned IOPS SSD (io1 and io2): This volume type is designed for I/O-intensive workloads such as large relational or NoSQL databases that require low latency and high performance.
3. Throughput Optimized HDD (st1): This volume type is designed for frequently accessed, throughput-intensive workloads such as big data, data warehouses, and log processing. (Standard throughput 1)
4. Cold HDD (sc1): This volume type is designed for less frequently accessed workloads such as colder data that requires fewer scans per day. (Standard cold 1)

##### Tips  
1. Choose the right volume type: It is important to choose the right EBS volume type for your workload to ensure optimal performance and cost-effectiveness. Consider factors such as the required IOPS, throughput, and capacity when selecting a volume type.
2. Monitor performance: Use Amazon CloudWatch to monitor the performance of your EBS volumes. This can help you identify any performance issues and take corrective action.
For more information about Amazon EBS volume types, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html).

<a name=“ebs-multi-attach”></a>
### Amazon EBS Multi-Attach

#### Overview
Amazon EBS Multi-Attach enables you to attach a single Provisioned IOPS SSD (io1 or io2) volume to multiple instances that are in the same Availability Zone. Each instance to which the volume is attached has full read and write permission to the shared volume. Multi-Attach makes it easier for you to achieve higher application availability in clustered Linux applications that manage concurrent write operations1.

##### Use Cases
Multi-Attach is useful for achieving higher application availability in clustered Linux applications that manage concurrent write operations1.

##### Tips
1. Multi-Attach enabled volumes can be attached to up to 16 Linux instances built on the Nitro System that are in the same Availability Zone.
2. Multi-Attach is supported exclusively on Provisioned IOPS SSD (io1 and io2) volumes.
3. Standard file systems, such as XFS and EXT4, are not designed to be accessed simultaneously by multiple servers, such as EC2 instances. Using Multi-Attach with a standard file system can result in data corruption or loss, so this is not safe for production workloads. You can use a clustered file system to ensure data resiliency and reliability for production workloads.
4. Your applications must provide write ordering for the attached instances to maintain data consistency.
5. Multi-Attach enabled volumes can’t be created as boot volumes.
For more information about Amazon EBS Multi-Attach, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes-multi.html).

<a name="aws-ami"></a>
### Amazon Machine Images (AMI)

#### Overview
An [Amazon Machine Image (AMI)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) is a pre-configured virtual machine image that you can use to launch an EC2 instance. AMIs include the operating system, application server, and any additional software needed to run your application. You can create your own custom AMIs or choose from thousands of pre-built AMIs provided by AWS and the AWS Marketplace.

##### Use Cases
- Launching EC2 instances: You can use an AMI to launch an EC2 instance with the operating system and software configuration that you need for your application.
- Sharing configurations: You can share your custom AMIs with other AWS accounts, allowing them to launch EC2 instances with the same configuration as yours.

##### Tips
- Choose the right AMI: When launching an EC2 instance, it is important to choose the right AMI for your needs. Consider factors such as the operating system, software packages, and security settings when selecting an AMI.
- Keep your AMIs up to date: It is important to keep your custom AMIs up to date with the latest security patches and software updates. You can create new versions of your AMIs with updated software and use them to launch new EC2 instances.
- AMIs are built for a specific region and can be copied across.
For more information about Amazon Machine Images (AMI), you can refer to the [official AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html).

##### Differences
- "Create image": exact copy of the software.
- "Template": exact copy of the EC2 hardware, without software configuration.
- "Create more like this": opens EC2 wizard, without software configuration
##### Creating an AMI
1. Create an image from EC2 instance. You can add configurations such as installing Apache HTTPd or security software so that they do not need to be installed again when an instance is launched from an AMI.

![](https://i.imgur.com/rmoOR6V.png)

2. Creating image

![](https://i.imgur.com/vcCtek1.png)

3. Launching instance using AMI

![](https://i.imgur.com/2S4L7s3.png)

<a name="aws-instance-store"></a>
### Amazon EC2 Instance Store

#### Overview
An Amazon EC2 Instance Store provides temporary block-level storage for your EC2 instance. This storage is located on disks that are physically attached to the host computer. Instance store is ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content. It can also be used to store temporary data that you replicate across a fleet of instances, such as a load-balanced pool of web servers.

##### Use Cases
- Temporary storage: You can use instance store for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content.
- Replicated data: You can use instance store to store temporary data that you replicate across a fleet of instances, such as a load-balanced pool of web servers.

##### Tips
- Data persistence: The data on an instance store volume persists only during the lifetime of its associated instance. If an instance reboots (intentionally or unintentionally), data in the instance store persists. However, data in the instance store is lost under any of the following circumstances: The underlying disk drive fails; The instance stops; The instance hibernates; The instance terminates. Therefore, do not rely on instance store for valuable, long-term data.
- Choose the right instance type: The size of the instance store available and the type of hardware used for the instance store volumes are determined by the instance type. Choose an instance type that provides the appropriate amount of instance store for your needs.

For more information about EC2 Instance Store, you can refer to the [official AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html).

<a name=“aws-efs”></a>
### Amazon Elastic File System (EFS)

#### Overview
Amazon Elastic File System (EFS) is a fully managed service that provides scalable file storage for use with Amazon EC2 instances. EFS is designed to be highly available and durable, automatically growing and shrinking as you add and remove files. This makes it easy to set up and use shared file storage in the cloud without having to worry about managing capacity or infrastructure. EFS works with EC2 instances in multi-AZ. It is 3x the price of a gpt2 volume.

##### Use Cases
Amazon EFS can be used for a variety of use cases, including:
1. Simplifying DevOps: Share code and other files in a secure, organized way to increase DevOps agility and respond faster to customer feedback.
2. Modernizing application development: Persist and share data from your AWS containers and serverless applications with zero management required.
3. Enhancing content management systems: Simplify persistent storage for modern content management system (CMS) workloads.
4. Accelerating data science: Easier to use and scale, Amazon EFS offers the performance and consistency needed for machine learning (ML) and big data analytics workloads.

##### Tips
1. Choose the right storage class: Amazon EFS offers several storage classes that allow you to optimize your costs based on your access patterns. For example, you can use the EFS Infrequent Access (IA) storage class to automatically move infrequently accessed files to lower-cost storage.
3. Monitor performance: Use Amazon CloudWatch to monitor the performance of your EFS file systems. This can help you identify any performance issues and take corrective action.
4. Use lifecycle policies: You can use lifecycle policies to automatically transition files between storage classes based on their age. This can help you reduce your storage costs by automatically moving older, less frequently accessed files to lower-cost storage.
For more information about Amazon EFS, you can refer to the official [AWS documentation](https://aws.amazon.com/efs/).

##### Creating an Elastic File System

![](https://i.imgur.com/EvmG6Dj.png)

##### Configuring mount targets 
![](https://i.imgur.com/qBWtVw0.png)

##### Created EFS 

![](https://i.imgur.com/7unMxN0.png)

##### Launching instance with EFS mounted 
* A subnet has to be selected
* EFS can be mounted with security groups created and attached automatically to the file system and instance. 
* EFS can be mounted with the required user data script containing `efs-utils`.

![](https://i.imgur.com/NF10kmo.png)

##### Launched instances in different availability zones

![](https://i.imgur.com/pZz7vrF.png)

#### Relevant security groups are automatically attached to the EFS availability zones

![](https://i.imgur.com/d7ijFrB.png)

#### While connected to the instance, you can then use the file system and access its contents from instances in different availability zones

![](https://i.imgur.com/Q3bvhdS.png)

<a name=“aws-elb”></a>
### Amazon Elastic Load Balancer (ELB)

#### Overview
Amazon Elastic Load Balancer (ELB) is a fully managed service that automatically distributes incoming application traffic across multiple targets and virtual appliances in one or more Availability Zones (AZs). ELB is designed to improve application scalability and availability by distributing traffic to healthy targets and automatically scaling resources as needed. With ELB, you can secure your applications with integrated certificate management, user-authentication, and SSL/TLS decryption.

##### Use Cases
Amazon ELB can be used for a variety of use cases, including:
1. Modernizing applications with serverless and containers: Scale modern applications to meet demand without complex configurations or API gateways.
2. Improving hybrid cloud network scalability: Load balance across AWS and on-premises resources using a single load balancer.
3. Retaining your existing network appliances: Deploy network appliances from your preferred vendor while taking advantage of the scale and flexibility of the cloud1.

##### Tips
1. Monitor performance: Use Amazon CloudWatch to monitor the health and performance of your applications in real-time, uncover bottlenecks, and maintain SLA compliance.
2. Choose the right type of load balancer: Amazon ELB offers several types of load balancers, including Application Load Balancer, Gateway Load Balancer, and Network Load Balancer. Choose the type that best fits your needs.

For more information about Amazon EFS, you can refer to the official [AWS documentation](https://aws.amazon.com/elasticloadbalancing/).

<a name=“aws-alb”></a>
### Amazon Application Load Balancer (ALB)

#### Overview
Amazon Application Load Balancer (ALB) is a feature of Elastic Load Balancing that operates at the request level (layer 7), routing traffic to targets (EC2 instances, containers, IP addresses, and Lambda functions) based on the content of the request. It is ideal for advanced load balancing of HTTP and HTTPS traffic and provides advanced request routing targeted at delivery of modern application architectures, including microservices and container-based applications. ALB simplifies and improves the security of your application by ensuring that the latest SSL/TLS ciphers and protocols are used at all times.

##### Use Cases
Amazon ALB can be used for a variety of use cases, including:
1. Load balancing HTTP/HTTPS traffic: You can load balance HTTP/HTTPS traffic to targets such as Amazon EC2 instances, microservices, and containers based on request attributes (such as X-Forwarded-For headers).
2. Improving application security: When using Amazon Virtual Private Cloud (VPC), you can create and manage security groups associated with Elastic Load Balancing to provide additional networking and security options. You can configure an ALB to be Internet facing or create a load balancer without public IP addresses to serve as an internal (non-internet-facing) load balancer.

For more information about Amazon EFS, you can refer to the official [AWS documentation](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/).

#### Creating an Application Load Balancer  

I created two EC2 instances to demonstrate the load balancers working.

![](https://i.imgur.com/qlbfImL.png)

1. I created an Application Load Balancer
    - This is for HTTP and HTTPS kind of traffic.
    - Network Load balancer is for TCP, UDP or TLS and useful for maintaining low latencies.
    - Gateway Load Balancer  

![](https://i.imgur.com/mQ7xdLx.png)

2. I added some configuration to the ALB
    - I selected internet facing, IPv4 config. 
    - I created a security group to allow all HTTP traffic. 
    - I selected all availability zones so the ALB routes traffic in all these regions.

![](https://i.imgur.com/7IXnlhH.png)

3. I then created an instance target group and assigned the two EC2 instances so the ALB can automatically route traffic between those instances.

![](https://i.imgur.com/K89eC0P.png)
![](https://i.imgur.com/uwKQ5Zh.png)
![](https://i.imgur.com/cfDLEXh.png)

4. Once the ALB is launched, I can visit the public DNS on a browser, which load balances and redirects to different instances via the target group.
    - If I stop an instance, that instance would no longer be healthy in the target group and the user would no longer be directed to it via the ALB.

![](https://i.imgur.com/PURYuUC.png)
![](https://i.imgur.com/vX72rgz.png)

5. To ensure that the instances cannot be accessed unless it is getting an inbound request from ALB's security group, I added it as an HTTP inbound rule on port 80.
    - This is an added network security feature.

![](https://i.imgur.com/DN65xM3.png)

6. You can also add complex rules in the ALB for handling requests to simplify deployment and manage some of the routing logic from the application to the load balancer, as well as reducing duplicating routing logic across multiple instances of the application. 
    - For example, I can set an `IF Path is /error THEN Return fixed response 404 with message 'Not found page'` rule as a fallback page. 

![](https://i.imgur.com/rK0jtes.png)
![](https://i.imgur.com/kPNdOgp.png)
![](https://i.imgur.com/edY8o2i.png)

<a name=“aws-nlb”></a>
### Amazon Network Load Balancer (NLB)

#### Overview
A Network Load Balancer (NLB) functions at the layer 4 of the Open Systems Interconnection (OSI) model. It can handle millions of requests per second. After the load balancer receives a connection request, it selects a target from the target group for the default rule.

##### Use Cases
Some best use cases for Network Load Balancer include:

1. When you need to seamlessly support spiky or high-volume inbound TCP requests.
2. When you need to support a static or elastic IP address.
3. If you are using container services and/or want to support more than one port on an EC2 instance. NLB is especially well suited to ECS (The Amazon EC2 Container Service). 

##### Tips
1. To increase the fault tolerance of your applications, you can enable multiple Availability Zones for your load balancer and ensure that each target group has at least one target in each enabled Availability Zone.

For more information about Amazon NLB, you can refer to the official [AWS documentation](https://aws.amazon.com/elasticloadbalancing/network-load-balancer/).

#### Creating an Network Load Balancer 
    - Network Load balancer is for TCP, UDP or TLS and useful for maintaining low latencies.
    - Latency ~ 100ms (vs 400 ms for ALB).
    - NLB has one static IP per AZ, and supports assigning Elastic IP.
    - Target group includes routing EC2 instances, private IP Addresses, Application Load Balancer.
    - Health Checks support TCP, HTTP and HTTPS protocol.
    - The general flow for creating a NLB is same as creating an ALB except the use of TCP or UDP. 
    - NBL itself does not have security group unlike ALB so you control access using the security groups attached to the EC2 instances using the source IP. You can add rules to the security group of the EC2 instances to allow traffic from NBL.

<a name=“aws-gwlb”></a>
## Amazon Gateway Load Balancer (GWLB)

#### Overview
A Gateway Load Balancer (GWLB) functions at the layer 3 of the Open Systems Interconnection (OSI) model. It is a service that makes it easy and cost-effective to deploy, scale, and manage the availability of third-party virtual appliances. These appliances include firewalls (FW), intrusion detection and prevention systems, and deep packet inspection systems in the cloud.

##### Use Cases
With Gateway Load Balancer, you can easily add or remove advanced network functionality without extra management overhead. It provides the bump-in-the-wire technology you need to ensure all traffic to a public endpoint is first sent to the appliance before your application. Some key scenarios that you can accomplish using Gateway Load Balancer include:
1. Firewalls
2. Advanced packet analytics
3. Intrusion detection and prevention systems
4. Traffic mirroring
5. DDoS protection
6. Custom appliances

##### Tips
1. Tune TCP keep-alive or timeout values to support long-lived TCP flows.

For more information about Amazon GWLB, you can refer to the official [AWS documentation](https://aws.amazon.com/elasticloadbalancing/gateway-load-balancer/).

#### Creating a Gateway Load Balancer
    - Route tables are modified in the VPC and traffic is routed to a Gateway Load Balancer which spreads the traffic to some target group 3rd party security virtual appliances. If fine, the GWLB gets a return response and the traffic is sent to the destination application. Otherwise, the request is dropped.
    - Uses GENEVE protocol on port 6081.

<a name=“aws-elb-sticky-sessions”></a>
## Amazon Elastic Load Balancer (ELB) - Sticky Sessions

### Overview
Sticky sessions, also known as session affinity, is a feature of Amazon Elastic Load Balancer (ELB) that enables the load balancer to bind a user’s session to a specific target. This ensures that all requests from the user during the session are sent to the same target. This feature is useful for servers that maintain state information in order to provide a continuous experience to clients. To use sticky sessions, the client must support cookies.

##### Use Cases
1. Sticky sessions can be more efficient because unique session-related data does not need to be migrated from server to server. For each request from the same client, the load balancer processes the request to the same web server each time, where data is stored and updated as long as the session exists.

#### Tips
1. Application Load Balancers support both duration-based cookies and application-based cookies.
2. Sticky sessions are enabled at the target group level.
3. The key to managing sticky sessions is determining how long your load balancer should consistently route the user’s request to the same target.

For more information about Amazon ELB Sticky Sessions, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/sticky-sessions.html)

<a name=“aws-elb-cross-zone-load-balancing”></a>
### Amazon Elastic Load Balancer (ELB) - Cross-Zone Load Balancing

#### Overview
Cross-zone load balancing is a feature of Amazon Elastic Load Balancer (ELB) that enables the load balancer to distribute incoming traffic evenly across all registered targets in all enabled Availability Zones. When cross-zone load balancing is disabled, each load balancer node distributes traffic only across the registered targets in its Availability Zone.

##### Pricing
- ALB has cross zone load balancing enabled by default and concurs no charges for inter AZ data transfer.
- NLB & GWLB has cross zone load balancing disabled by default and concurs charges for inter AZ data if enabled.
- Classic load balancing has it disabled by default, and concurs no charges for inter AZ data if enabled.
![](https://i.imgur.com/RrEodKg.png)

##### Use Cases
1. Cross-zone load balancing reduces the need to maintain equivalent numbers of instances in each enabled Availability Zone and improves your application’s ability to handle the loss of one or more instances. However, it is still recommended that you maintain approximately equivalent numbers of instances in each enabled Availability Zone for higher fault tolerance.

##### Tips
1. When you create a Classic Load Balancer, the default for cross-zone load balancing depends on how you create the load balancer. With the API or CLI, cross-zone load balancing is disabled by default. With the AWS Management Console, the option to enable cross-zone load balancing is selected by default (but can be disabled at the Target Group level).
2. After you create a Classic Load Balancer, you can enable or disable cross-zone load balancing at any time.

For more information about Amazon ELB Cross-Zone Load Balancing, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#cross-zone-load-balancing)

<a name=“aws-elb-ssl-certificates”></a>

## Amazon Elastic Load Balancer (ELB) - SSL Certificates

### Overview
An SSL/TLS certificate is required for your Amazon Elastic Load Balancer (ELB) if you use HTTPS (SSL or TLS) for your front-end listener. The load balancer uses the certificate to terminate the connection and then decrypt requests from clients before sending them to the instances. The SSL and TLS protocols use an X.509 certificate (SSL/TLS server certificate) to authenticate both the client and the back-end application.

#### Use Cases
You can manage certificates using AWS Certificate Manager (ACM), which integrates with Elastic Load Balancing so that you can deploy the certificate on your load balancer.
Alternatively, you can create or import your own certificates using a tool that supports the SSL and TLS protocols, such as OpenSSL.

#### Tips
1. When you create a certificate for use with your load balancer, you must specify a domain name. The domain name on the certificate must match the custom domain name record so that we can verify the TLS connection.
2. You can use an asterisk (*) as a wild card to protect several site names in the same domain when requesting a wild-card certificate.
3. ALB and NLB can present multiple certificates through the same secure listener using Server Name Indication (SNI), which enables it to support multiple secure websites using a single secure listener. CLB however only supports one SSL certificate and require multiple CLB for multiple hostname with multiple SSL certificates.

For more information about Amazon ELB SSL Certificates, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/ssl-server-cert.html)

##### Adding SSL certificate
1. Select your load balancer.

![](https://i.imgur.com/eEbmY8I.png)
2. In the Listeners tab, choose Add listener.

![](https://i.imgur.com/0dANQJI.png)

3. For Protocol, choose HTTPS.
4. For Default actions, choose Forward to and then choose the target group for your instances.
5. Choose either:
    - Choose an existing certificate from AWS Certificate Manager (ACM): Select this option to use a certificate that you already requested or imported into ACM.
    - Import a new SSL/TLS certificate: Select this option to import a certificate into ACM for use with this load balancer. You must specify the certificate and private key in PEM format and the certificate chain in PEM format.
6. Choose Save.
7. After you add the HTTPS listener, you can update your DNS records to point to the DNS name of your load balancer. The DNS name is displayed in the Description tab for your load balancer.

For more detailed instructions and information about adding an SSL certificate to an Amazon ELB, you can refer to the official AWS documentation.

<a name=“aws-elb-connection-draining”></a>
### Amazon Elastic Load Balancer (ELB) - Connection Draining

#### Overview
Connection Draining is a feature of Amazon Elastic Load Balancer (ELB) that ensures that a Classic Load Balancer stops sending requests to instances that are de-registering or unhealthy, while keeping the existing connections open. This enables the load balancer to complete in-flight requests made to instances that are de-registering or unhealthy. It is also known as "Deregistration delay" for ALB and NLB.

##### Use Cases
1. When an instance fails health checks, the ELB will not send any new requests to the unhealthy instance. However, it will still allow existing (in-flight) requests to complete for the duration of the configured timeout.
2. Connection Draining can be used to prevent breaking open network connections while taking an instance out of service, updating its software, or replacing it with a fresh instance that contains updated software.

##### Tips
1. You can enable Connection Draining for your load balancer at any time and specify a maximum time for the load balancer to keep connections alive before reporting the instance as de-registered.
2. When the maximum time limit is reached, the load balancer forcibly closes connections to the de-registering instance.

For more information about Amazon ELB Connection Draining, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/config-conn-drain.html)

<a name=“aws-elb-auto-scaling-groups”></a>
### Amazon Elastic Load Balancer (ELB) - Auto Scaling Groups

#### Overview
Amazon Elastic Load Balancer (ELB) integrates with Amazon EC2 Auto Scaling to help you to insert an Application Load Balancer, Network Load Balancer, Classic Load Balancer, or Gateway Load Balancer in front of your Auto Scaling group. When you attach a load balancer to your Auto Scaling group, this registers the group with the load balancer, which acts as a single point of contact for all incoming web traffic to your Auto Scaling group. Auto Scaling groups itself is free but you are charged for the underlying hardware provisioned.

##### Use Cases
1. Instances that are launched by your Auto Scaling group are automatically registered with the load balancer. Likewise, instances that are terminated by your Auto Scaling group are automatically deregistered from the load balancer.
2. After attaching a load balancer to your Auto Scaling group, you can configure your Auto Scaling group to use Elastic Load Balancing metrics (such as the Application Load Balancer request count per target) to scale the number of instances in the group as demand fluctuates.
3. You can set a minimum and maximum capacity as well as setting a desired capacity of EC2 instances. 
4. You can also use CloudWatch Alarms to scale an ASG based on metrics such as "Average CPU" or other custom metrics.

##### Tips
1. You can attach an Elastic Load Balancing load balancer to your Auto Scaling group at any time. This registers the group with the load balancer, which acts as a single point of contact for all incoming web traffic to your Auto Scaling group.
2. Optionally, you can add Elastic Load Balancing health checks to your Auto Scaling group so that Amazon EC2 Auto Scaling can identify and replace unhealthy instances based on these additional health checks.

For more information about Amazon ELB and Auto Scaling groups, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-load-balancer.html)

##### Creating an Auto Scaling group
1. Define the launch template with AMI type, instance type, key pair, network settings, storage and more.

![](https://i.imgur.com/LFc01QT.png)

2. Choose the network, AZ and subnets the ASG can be launched on.

![](https://i.imgur.com/YcEgaRV.png)

3. You can use launch template or override it to specify instance attributes, manually add instance types and have a mix of on-demand and spot instances.

![](https://i.imgur.com/WxYBLbb.png)

4. You can attach a load balancer to the Auto Scaling group.  

![](https://i.imgur.com/S6HdsaQ.png)    

5. You can then define the health checks. EC2 health check can trigger an Auto scaling group to remove an instance from the group if an EC2 instance is unhealthy. ELB health checks can trigger if the ELB deems the instance to be unhealthy.

6. You can specify the group size.

![](https://i.imgur.com/CTXJl8R.png)

7. Auto Scaling group is created.

![](https://i.imgur.com/CLEPv8x.png)

8. In the Auto Scaling group's "Activity" tab, you can see the Auto Scaling group provisioning the instances. The actual instances can be found on the Auto Scaling groups's "Instance management" tab as well as the EC2 instances page.

![](https://i.imgur.com/0TjAMlG.png)
![](https://i.imgur.com/egf6jVW.png)
![](https://i.imgur.com/k4qDpoS.png)

9. If I terminate of the instances created by the Auto scaling group, another one is automatically provisioned to meet the 2 instance group size requirement.

![](https://i.imgur.com/Fu8dwFQ.png)

<a name=“aws-elb-auto-scaling-groups-scaling-policies”></a>
#### Scaling Policies

With step scaling and simple scaling, you choose scaling metrics and threshold values for the CloudWatch alarms that invoke the scaling process. You also define how your Auto Scaling group should be scaled when a threshold is in breach for a specified number of evaluation periods. When step adjustments are applied, and they increase or decrease the current capacity of your Auto Scaling group, the adjustments vary based on the size of the alarm breach. In most cases, step scaling policies are a better choice than simple scaling policies, even if you have only a single scaling adjustment.

##### Scaling cooldown
After a scaling activity, there is a 5 minute minute cooldown where the ASG will not launch or terminate additional instances to allow metrics to stabilize.
Therefore, use a ready-to-use AMI to reduce configuration time in order to be serving requests faster and reduce the cooldown period.

The scaling policies include:

![](https://i.imgur.com/3dMS5YJ.png)

1. Target Tracking Scaling - "I want average CPU to stay around 40%" 

![](https://i.imgur.com/7VhMdeR.png)

2. Step scaling - When a CloudWatch alarm is triggered (CPUUtilization > 70%), then add 2 instances.

![](https://i.imgur.com/psLaYtw.png)

3. Scheduled actions - Scaling based on known pattern. E.g. increase minimum capacity to 8 at 5pm on Friday. 

![](https://i.imgur.com/60qP7Sy.png)

4. Predictive scaling - forecasting load and scheduling based on historical load

![](https://i.imgur.com/hWatEOL.png)

##### Good metrics to scale on:
1. CPUUtilization - Average CPU utilization across your instances
2. RequestCountPerTarget - To make sure the number of requests per EC2 instances is stable
3. Average Network In/Out (if the application is network bounded)
4. Other custom metric (that you can create using CloudWatch)

For more information about Amazon ELB and Auto Scaling groups, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-load-balancer.html)

##### Target Tracking Scaling example

1. Set the desired and minimum capacity to 1 and maximum capacity to 3 so that the Auto Scaling group can scale out.

![](https://i.imgur.com/r3rCT20.png)

2. Create a Target Tracking Policy with a target of CPU Utilization at 40%. This will create CloudWatch alarms and trigger scale out or scale in behavior.

![](https://i.imgur.com/LV4f0n3.png)

3. I installed Stress Util on Amazon Linux 2 using:
```
sudo amazon-linux-extras install epel -y
sudo yum install stress -y
stress -c 4
```

`stress -c 4` makes 4 vCPU being used at a time to increase CPU utilization.

![](https://i.imgur.com/pBs0jeH.png)

4. CPU Utilization exceeds 40%

![](https://i.imgur.com/jRybxme.png)

5. The activity section in Auto Scaling group showing the capacity scaling out from 1 to 3 to maintain CPU Utilization at 40%.

![](https://i.imgur.com/1XsHs3s.png)

6. A CloudWatch Alarm is also triggered where it shows the condition triggered. 

![](https://i.imgur.com/3z2U8W5.png)


6. Once the alarm is triggered, the new instances are automatically provisioned to meet the increase in CPU demand. Once the script is closed, the desired capacity scales in from 3 to 1 and the two extra instances are terminated.

![](https://i.imgur.com/8AqyCQs.png)

<a name=“aws-elb-auto-scaling-groups-instance-refresh”></a>
#### Instance Refresh

Instance refresh is a feature that helps you update all instances in an Auto Scaling group in a rolling fashion (i.e., one batch at a time). You can use instance refresh to update instances to use a new launch template or launch configuration.
During an instance refresh, Amazon EC2 Auto Scaling takes care of updating instances for you by terminating instances and replacing them with new instances through setting a minimum healthy percentage such as 70%. 
You can also set up a warm-up time which is a prediction of how long until the instance is ready to use

For more information about Amazon ELB and Auto Scaling groups, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-load-balancer.html)

<a name=“aws-rds”></a>
### Amazon Relational Database Service (RDS)

#### Overview
Amazon Relational Database Service (Amazon RDS) is a web service that makes it easier to set up, operate, and scale a relational database in the AWS Cloud. It provides cost-efficient, resizable capacity for an industry-standard relational database and manages common database administration tasks. You can choose from seven popular engines — Amazon Aurora with MySQL compatibility, Amazon Aurora with PostgreSQL compatibility, MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server — and deploy on-premises with Amazon RDS on AWS Outposts.

##### Use Cases
1. Build web and mobile applications: Support growing apps with high availability, throughput, and storage scalability. Take advantage of flexible pay-per-use pricing to suit various application usage patterns.
2. Move to managed databases: Innovate and build new apps with Amazon RDS instead of worrying about self-managing your databases, which can be time consuming, complex, and expensive such as: 
    - Provisioning, OS patching
    - Backups, restoring to a timestamp (Point in Time Restore)
    - Monitoring dashboards
    - Read replicas for better read performance
    - Multi AZ setup for disaster recovery
    - Maintenance windows for upgrades
    - Scaling capability (horizontal and vertical)
    - Storage backed by EBS (gp2 or io1)
3. Break free from legacy databases: Free yourself from expensive, punitive, commercial databases by migrating to Amazon Aurora. When you migrate to Aurora, you get the scalability, performance, and availability of commercial databases at 1/10th the cost.
4. RDS Storage Auto Scaling .
    - Increase storage on RDS DB instance dynamically
    - When RDS detects you are running out of space, it scales automatically up to a maximum storage threshold and if:
        1. Free storage is less than 10% of allocated storage
        2. Low storage lasts at least 5 minutes
        3. 6 hours have passed since last modification 
    - Useful for applications with unpredictable workloads
##### Tips
1. You can use the AWS Database Migration Service (DMS) to easily migrate or replicate your existing databases to Amazon RDS.
2. Amazon RDS Partners help you with database monitoring, security, and performance using Amazon RDS database engines.

For more information about Amazon RDS you can refer to the [official documentation](https://aws.amazon.com/rds/)

<a name=“aws-rds-read-replicas”></a>
### Amazon RDS Read Replicas

#### Overview
Amazon RDS Read Replicas provide enhanced performance and durability for Amazon RDS database (DB) instances. They make it easy to elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads. You can create one or more replicas of a given source DB Instance and serve high-volume application read traffic from multiple copies of your data, thereby increasing aggregate read throughput. Read replicas can also be promoted when needed to become standalone DB instances. For RDS Read Replicas, you are not charged for networking cost of asynchronous replication to another AZ in the same region, but cross region replication is charged.

![](https://i.imgur.com/PKVwixD.png)

##### Use Cases
1. Scaling beyond the compute or I/O capacity of a single DB instance for read-heavy database workloads. You can direct this excess read traffic to one or more read replicas.
2. Serving read traffic while the source DB instance is unavailable. In some cases, your source DB instance might not be able to take I/O requests, for example due to I/O suspension for backups or scheduled maintenance. In these cases, you can direct read traffic to your read replicas.
3. Business reporting or data warehousing scenarios where you might want business reporting queries to run against a read replica, rather than your production DB instance.
4. Implementing disaster recovery. You can promote a read replica to a standalone instance as a disaster recovery solution if the primary DB instance fails.

##### Tips
1. For the MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server database engines, Amazon RDS creates a second DB instance using a snapshot of the source DB instance. It then uses the engines’ native asynchronous replication to update the read replica whenever there is a change to the source DB instance.
2. The read replica operates as a DB instance that allows only read-only connections; applications can connect to a read replica just as they would to any DB instance.

##### Multi AZ Disaster recovery
Amazon Multi AZ helps with diaster recovery by synchronously replicating master DB instance to a standby instance in another AZ. There is only one DNS name and the application automatically failover to standby instance if there are any issues with the master DB such as loss of AZ, network, instance or storage failure. This increases availability. 

![](https://i.imgur.com/ASrkfPo.png)

##### Launching an RDS instance

1. You can choose between "standard create" which lets you configure the database, and "easy create" which uses best practice configuration and lets you quickly deploy the database. There are many relational database engines that you can select.

![](https://i.imgur.com/e9o1RAq.png)

2. For production and dev environments, you might use Multi-AZ deployment to synchronously replicate master DB to a standby database for better availability and disaster recovery. A Multi-AZ DB cluster deployment in Amazon RDS provides a high availability deployment mode of Amazon RDS with two readable standby DB instances. A Multi-AZ DB cluster has a writer DB instance and two reader DB instances in three separate Availability Zones in the same AWS Region. Multi-AZ DB clusters provide high availability, increased capacity for read workloads, and lower write latency when compared to Multi-AZ DB instance deployments.

You should consider using Multi-AZ DB cluster deployments with two readable DB instances if you need additional read capacity in your Amazon RDS Multi-AZ deployment and if your application workload has strict transaction latency requirements such as single-digit milliseconds transactions.
 
![](https://i.imgur.com/bCotxb1.png)

3. After entering the database identifier and password, you can select the type of class. I selected `db.t2.micro` for the instance class, general purpose storage `gp2` with a maximum storage auto scaling of 1000 GB to remain in free tier. I used 

![](https://i.imgur.com/Y7DC30h.png)

4. For connectivity settings, you can automatically connect the DB to an EC2 instance which configures the underlying networking. I used my default VPC and set public access to true so that I can access the DB from my pc using the internet.

![](https://i.imgur.com/gFckQY0.png)

5. Users can authenticate to the database using password, IAM credentials or kerberos authentication. 

![](https://i.imgur.com/G5buj4L.png)

6. Giving an initial DB name creates the database when the instance is launched. The backup options can be used to specify if you want a backup and you can also define the retention period from 0-35 days. You can also select a window where you want the backups to be done. You can also 

![](https://i.imgur.com/xZxuH7h.png)

7. You can also select the type of logs you want to publish to CloudWatch logs, such as audit log, error, general and slow query log for long term retention. You can also choose to enable maintenance window and specify a time for automatic minor DB updates. Selecting "deletion protection" prevents the database from being deleted.

![](https://i.imgur.com/ek8Qjs9.png)

8. You can use a tool like Sql Electron client to connect and interface with the database.

![](https://i.imgur.com/znbt581.png)
![](https://i.imgur.com/bnaFKXo.png)

9. From the RDS instance console, you can also directly create read replicas to increase read capacity, migrate to another region, take snapshots and restore in a different AZ or restore to a point in time. In the "Monitoring" tab you can also see DB analytics such as CPU Utilization.

![](https://i.imgur.com/lR8qD3c.png)
![](https://i.imgur.com/zif4i8Q.png)

For more information about Amazon RDS Read Replicas you can refer to the [official documentation](https://aws.amazon.com/rds/features/read-replicas/)

<a name=“aws-aurora”></a>
### Amazon Aurora

#### Overview
Amazon Aurora is a fully managed relational database engine that’s compatible with MySQL and PostgreSQL. It combines the speed and reliability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. With some workloads, Aurora can deliver up to five times the throughput of MySQL and up to three times the throughput of PostgreSQL without requiring changes to most of your existing applications. Aurora storage automatically grows in increments of 10GB, up to 128 TB and have up to 15 replicas and the replication process is faster than MySQL. Aurora costs 20% more than RDS but is more efficient. Failover in Aurora is instantaneous compared to multi-az or cluster for RDS. As well there is:
- Industry compliance
- Push-button scaling
- Automated patching with 0 downtime
- Backtrack (restore data at any point without using backups)
    - Amazon Aurora Backtrack is a feature that allows you to “rewind” a database cluster to a specific point in time without restoring data from a backup. This feature works by using the distributed, log-structured storage system of Amazon Aurora. Each change to your database generates a new log record, identified by a Log Sequence Number (LSN). When you enable the backtrack feature, Aurora provisions a FIFO buffer in the cluster for storage of LSNs, allowing for quick access and recovery times measured in seconds
- Regular backup 
- Isolation and security.

##### Use Cases
1. Modernize enterprise applications: Operate enterprise applications, such as customer relationship management (CRM), enterprise resource planning (ERP), supply chain, and billing applications, with high availability and performance.
2. Build SaaS applications: Support reliable, high-performance, and multi-tenant Software-as-a-Service (SaaS) applications with flexible instance and storage scaling.
3. Deploy globally distributed applications: Develop internet-scale applications, such as mobile games, social media apps, and online services, that require multi-Region scalability and resilience.

##### High Availability and Read Scaling
1. 6 copies of your data is made across 3 AZ
    - 4 copies out of 6 needed for writes
    - 3 copies out of 6 needed for reads
    - self healing with peer-to-peer replication if there is data corruption
    - storage is striped across 100s of volumes
2. There is only once Aurora instance that takes writes (master)
3. If the master doesn't work, failover happens in < 30 seconds

![](https://i.imgur.com/8Ot8FIL.png)

##### Aurora DB Cluster
- Master writes to shared storage volume (Auto expanding 10GB - 128TB)
- Aurora provides "Writer Endpoint" which always points to the master, even if the instance failover. The client is always talking to the writer endpoint. 
- You can set up auto scaling on the read replicas so the read requirements are fulfilled.
- There is also a "Reader Endpoint" which automatically connects to all the read replicas and load balances connections; when the client connects they are redirected to one of the read replicas.

![](https://i.imgur.com/x5MEcll.png)
![](https://i.imgur.com/Pr2rJTE.png)

##### Tips
1. Easily migrate MySQL or PostgreSQL databases to and from Aurora using standard tools, or run legacy SQL Server applications with Babelfish for Aurora PostgreSQL with minimal code change.

![](https://i.imgur.com/ZbjXnfm.png)

##### Security for RDS & Aurora 
1. Data encrypted in volumes at rest 
    - Database master & replica encryption using AWS KMS - must be defined as launch time
    - If the master is not encrypted, the read replicas cannot be encrypted
    - To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted
2. In-light encryption
    - TLS-ready by default, use the AWS TLS root certificates client-side
3. IAM Authentication
    - IAM roles to connect to your database (instead of username/ password)
4. Security Groups
    - Control Network access to your RDS/ Aurora DB
5. Audit Logs can be enabled and sent to CLoudWatch Logs for longer retention

For more information about Amazon Aurora, you can refer to the official [AWS documentation](https://aws.amazon.com/rds/aurora/)

<a name=“aws-rds-proxy”></a>
### Amazon RDS Proxy

#### Overview
Amazon RDS Proxy is a fully managed, highly available database proxy for Amazon Relational Database Service (RDS) that makes applications more scalable, more resilient to database failures, and more secure. RDS Proxy allows applications to pool and share connections established with the database, improving database efficiency by reducing the stress on database resources like CPU, CRAM and minimize open connections and timeouts. RDS Proxy is never publicly accessible and must be accessed from VPC.

##### Use Cases
RDS Proxy is particularly helpful for applications that have unpredictable workloads, frequently open and close database connections, or require higher availability during transient database failures.

##### Tips
1. RDS Proxy can be enabled for most applications with no code changes.
2. RDS Proxy can enforce AWS Identity and Access Management (IAM) authentication for databases and securely store credentials in AWS Secrets Manager.
3. RDS Proxy can reduce failover times for Aurora and RDS databases by up to 66%.

##### RDS Proxy vs Multi-AZ 

Amazon RDS Proxy and Multi-AZ deployments are two different features that can be used together to improve the scalability, availability, and resilience of your database.

Multi-AZ deployments provide high availability by automatically failing over to a standby replica in a different Availability Zone in the event of a primary DB instance failure or planned maintenance. This can help minimize downtime and data loss. On the other hand, RDS Proxy is a fully managed database proxy that sits between your application and your RDS database. It allows applications to pool and share connections to the database, reducing the overhead of establishing new connections and improving database efficiency and application scalability. RDS Proxy can also reduce failover times for Aurora and RDS databases by up to 66%1.

Using reader/writer endpoints with Aurora allows you to distribute read traffic across multiple read replicas, improving read scalability. However, this approach requires you to manage connections to multiple endpoints in your application code.

In summary, RDS Proxy can provide additional benefits over Multi-AZ deployments and using reader/writer endpoints with Aurora by improving connection management, reducing failover times, and making applications more resilient to database failures.

For more information about Amazon RDS Proxy, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html)

<a name=“aws-elasticache”></a>
### Amazon ElastiCache

#### Overview
Amazon ElastiCache is a fully managed in-memory data store and cache service that supports two open-source in-memory engines: Redis and Memcached. ElastiCache can be used to improve the performance of web applications by allowing you to retrieve information from fast, managed, in-memory data stores instead of relying on slower disk-based databases.


##### ElastiCache Strategies
ElastiCache allows you to implement various caching strategies to improve the performance of your application. Two common strategies are Lazy Loading and Write-Through.

1. Lazy Loading, also known as “cache aside,” is a caching strategy that loads data into the cache only when necessary. When your application requests data, it first makes the request to the cache. If the data exists in the cache and is current, the cache returns the data to your application. If the data doesn’t exist in the cache or has expired, your application requests the data from your data store. Your data store then returns the data to your application. Your application next writes the data received from the store to the cache. See pseudocode below:

![](https://i.imgur.com/MgykC7V.png)
![](https://i.imgur.com/gUXT6Cv.png)

2. Write-Through is a caching strategy where data is written to both the cache and the underlying data store at the same time. This ensures that the cache always has the most up-to-date version of the data. However, this approach can increase write latency since writes must be performed on both the cache and the underlying data store.

![](https://i.imgur.com/bz5Idez.png)
![](https://i.imgur.com/bYJmk7s.png)

##### Cache Eviction and Time-to-live (TTL)
Cache eviction refers to the process of removing data from a cache when it is no longer needed or when the cache is full and needs to make room for new data. Time-to-live (TTL) is a common cache eviction strategy that automatically removes data from the cache after a specified period of time has passed. This can help ensure that the data in the cache is up-to-date and relevant.

For example, Hazelcast has a time-to-live-seconds attribute that can be set for entries in a map. Entries that are older than the specified number of seconds and have not been updated within that time will be automatically evicted from the map. Similarly, Spring Boot allows you to set a default TTL for cache entries using the spring.cache.redis.time-to-live property.

- TTL are helpful for any kind of data:
    1. Leaderboards
    2. Comments
    3. Activity streams
- TTL can range from few seconds to hours or days
- If there are too many evictions happening due to memory, you should scale up or out

##### Solutions Architecture 
1. DB Cache
    - Applications query ElastiCache and if there is a miss, the data is fetched from RDS and sent to application and written to ElastiCache
    - Relieves load in RDS
    - Cache must have invalidation strategy to ensure only most current data is used there

![](https://i.imgur.com/ztnSXIq.png)

2. User Session Store
    - User logs into any of the application
    - The application writes the session data into ElastiCache
    - The user hits another instance of our application
    - The instance retrieves the data if the user is already logged in
    - This makes the application stateless
        1. Making an application stateless means that the application does not store any data about client state or session information. Instead, this information is offloaded to a cache or a database so that the application itself does not need to keep track of it. This allows the application to scale horizontally and be tolerant to the failure of an individual node.

        2. Amazon ElastiCache is a service that can be used to offload state from an application. It provides an in-memory cache that can be used to store frequently accessed data, such as session information, at low latency. By using ElastiCache, you can improve the performance of your application and make it stateless.

![](https://i.imgur.com/nXuLy6G.png)

##### Amazon MemoryDB for Redis
Amazon MemoryDB for Redis is a Redis-compatible, durable, in-memory database service that delivers ultra-fast performance with high availability and durability. MemoryDB for Redis is built for applications that require microsecond read and write latencies with strong durability guarantees.

##### Use Cases
1. ElastiCache can be used for a variety of use cases such as lowering total cost of ownership by caching your data and offloading database I/O to reduce operational burden, lower costs, and improve performance of both the database and the application. It can also be used for real-time application data caching by storing frequently used data 2. in-memory for microsecond response times and high throughput to support hundreds of millions of operations per second.

##### Tips
1. ElastiCache integrates with other AWS services such as Amazon CloudWatch for enhanced visibility into key performance statistics associated with your cache.
2. You can use ElastiCache’s built-in data structures to simplify application development.
3. ElastiCache makes setup, scaling, and cluster failure handling much simpler than in a self-managed cache deployment.

##### Redis vs Memcached 

- Similar to RDS, REDIS has Multi AZ deployments with auto failover. It uses Read Replicas to scale reads and have high availability. It has high data durability using AOF persistence. It has backup and restore features as well as support for sets and sorted sets.
    1. Redis can use an Append Only File (AOF) for high data durability. AOF is a logging mechanism that writes every write operation performed on the Redis database to a log file on disk. This log file can be used to reconstruct the database in the event of a crash or failure. When Redis is restarted, it reads the log file and re-executes the write operations in the file to restore the database to its previous state.
    2. AOF provides better data durability than the snapshot persistence option, which only creates point-in-time snapshots of data. However, it is slower and requires more disk space, as it must write every write operation to the log file. The AOF file can be configured to be rewritten in the background when it gets too large, using a process called AOF fsync.
- Memcached in the other hand uses multi-node partitioning of data (sharding). It does not have high availability as there is no replications happening. There is no data persistence, and the ability to backup and restore. It has a multi-threaded architecture.

![](https://i.imgur.com/H5jyopA.png)

##### Creating an ElastiCache Redis cluster

- I disabled cluster mode to stay in the free tier and not use replication across multiple shards.

![](https://i.imgur.com/CKPYfp5.png)

- Using On Cloud location but you can create the Redis instance on premise by specifying an outpost id.

![](https://i.imgur.com/DgttbMW.png)

- I used `cache.t2.micro` and set the number of replicas to zero to remain in free tier.

![](https://i.imgur.com/dVtOdDQ.png)

- Then just like RDS, the network and subnets are defined. 

![](https://i.imgur.com/0MriXZf.png)

- Then for security, you can enable encryption at rest using AWS KMS as well as encryption in transit where you can use access control such as Redis Auth. 

![](https://i.imgur.com/vpUnvIC.png)

- Just like RDS, you can also set up backup and retention periods. 

![](https://i.imgur.com/lIXI6xP.png)

- Just like RDS, you can define the maintenance window for updates, as well as auto upgrade for minor versions.

![](https://i.imgur.com/9ULBfo7.png)

- Just like RDS, you can also enable logs for logging queries based on some criteria.

![](https://i.imgur.com/n0PXf9U.png)

##### Summary
1. Lazy loading / Cache aside is easy to implement and works for many situations as a foundation, especially on read side. 
2. Write-through is usually combined with lazy loading as targeted for queries or workloads that benefit from this optimization to improve cache staleness.
3. Setting a TTL is usually not a bad idea, except when you're using Write-through. Set it to a sensible value for the application.
4. Only cache things that make sense (user profile, blogs, etc)

For more information about Amazon ElastiCache, you can refer to the official [AWS documentation](https://aws.amazon.com/elasticache/)

<a name=“aws-memorydb-for-redis”></a>
### MemoryDB for Redis Overview

##### Overview
Amazon MemoryDB for Redis is a durable, in-memory database service that delivers ultra-fast performance. It is purpose-built for modern applications with microservices architectures and is compatible with Redis, a popular open source data store. With MemoryDB, all of your data is stored in memory, which enables you to achieve microsecond read and single-digit millisecond write latency and high throughput.

![](https://i.imgur.com/7N50AaX.png)

##### Tips
1. MemoryDB for Redis can be used as a high-performance primary database for your microservices applications, eliminating the need to separately manage both a cache and durable database.
2. You can build applications quickly with Redis, Stack Overflow’s “Most Loved” database for five consecutive years.
3. MemoryDB also stores data durably across multiple Availability Zones (AZs) using a Multi-AZ transactional log to enable fast failover, database recovery, and node restarts.
 
For more information about Amazon Aurora, you can refer to the official [AWS documentation](https://aws.amazon.com/memorydb/)