
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


