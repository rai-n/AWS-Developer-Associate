# Guide to AWS

## Table of Contents
- [Identity and Access Management](#aws-iam)
- [Elastic Compute Cloud](#aws-ec2)
- [Elastic Block Store](#aws-ebs)
    1. [Volume types](#ebs-volume-types)
    2. [Multi Attach](#ebs-multi-attach)
- [Amazon Machine Image](#aws-ami)
- [Elastic File System](#aws-efs)
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

#### While connected to the instance, you can then use the file system

![](https://i.imgur.com/Q3bvhdS.png)