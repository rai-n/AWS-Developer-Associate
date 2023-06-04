### S3
Amazon Simple Storage Service (S3) is an object storage service provided by Amazon Web Services (AWS). It offers industry-leading scalability, durability, and security for storing and retrieving any amount of data. S3 is designed to be highly available and can be used to store and retrieve any type of data, including images, videos, documents, and application backups. It provides a simple web services interface that allows developers to easily integrate S3 into their applications.

#### Use Cases:
1. Data Backup and Restore: S3 can be used as a backup storage solution for applications, databases, and file systems. It provides durability guarantees, making it a reliable option for long-term data retention.
2. Static Website Hosting: S3 allows you to host static websites and deliver content with low latency and high availability. It supports features like website redirection, error document handling, and custom error pages.
    - You enable the static website hosting option on S3 bucket and upload an `index.html` file. 

![])https://i.imgur.com/s6dM7Yt.png
![](https://i.imgur.com/tsg31Yo.png)

3. Data Archiving: S3 provides cost-effective storage options for archiving infrequently accessed data. You can leverage lifecycle policies to automatically transition data to lower-cost storage classes based on predefined rules.
4. Big Data Analytics: S3 is often used as a data lake for storing large datasets that can be processed by big data analytics frameworks like Amazon EMR, Amazon Athena, or Amazon Redshift. It provides a scalable and durable storage platform for big data workloads.
5. Content Distribution: S3 integrates with Amazon CloudFront, allowing you to distribute your content globally with low latency and high data transfer speeds. This makes it ideal for delivering static files, such as images, videos, and software downloads, to end-users.

#### Tips:
1. Set up proper access control: Use AWS Identity and Access Management (IAM) to manage access to your S3 buckets. Follow the principle of least privilege and assign appropriate permissions to users and roles to ensure the security of your data.
2. Enable versioning: Enable versioning for your S3 buckets to maintain a history of object modifications. This helps protect against accidental deletions or overwrites and allows you to restore previous versions of objects if needed.
    * Note that any file that is not versioned prior to enabling version will have version "null"
    * If you suspend versioning it does not delete previous version
    * Deleting a version reverts static site back to previous version

![](https://i.imgur.com/S3iYRcJ.png)
![](https://i.imgur.com/kTWVmUg.png)
3. Monitor bucket usage and metrics: Utilize Amazon CloudWatch to monitor S3 bucket metrics, such as data transfer rates, request rates, and error rates. This can help you optimize your S3 usage and detect any unusual behavior or performance issues.
4. Configure lifecycle policies: Take advantage of S3's lifecycle management feature to automatically transition objects to lower-cost storage classes or delete them based on predefined rules. This can help optimize costs and ensure compliance with data retention policies.

##### Buckets
In Amazon Simple Storage Service (S3), data "files" is organized and stored within containers "directories" called "buckets". Buckets are logical containers that hold objects, and they provide a way to organize and manage data stored in S3. Each bucket has a globally unique name, which is used to access and refer to the data stored within it but is defined at the region level. AWS buckets offer a scalable and highly available storage solution for a wide range of use cases. The naming convention is:
1. No uppercase or underscore
2. 3-63 characters long
3. Not an IP address
4. Must start with lowercase letter or number
5. Must not start with prefix xn-
6 .Must not end with suffix -s3alias

* Objects (files) have key
* The key is the FULL path:
    * s3://my-bucket/path/to/file.txt
* The key is composed of prefix + object name 
    * Prefix: `s3://my-bucket/path/to/`
    * Object name: `file.txt`
* There's no concept of directories within buckets (although the UI will trick you to think otherwise)
* Just keys with very long names that contain "/"
* Object values are the content of the body
    * Max object size is 5TB
    * If uploading more than 5GB, you must use "multi-part" upload
* Metadata (list of text key / value pairs - system or user metadata)
* Tags (Unicode key / value pair - up to 10) - useful for security / lifecycle
* Version ID - if versioning is enabled 

##### Uploading files to bucket
* Files can be uploaded to a bucket and organized using folders. Clicking on "Open" opens a link that has been presigned by AWS. Using the S3 public URL does not work by default and has to be made public.

![](https://i.imgur.com/zfR0T1F.png)
![](https://i.imgur.com/fMvrP2l.png)

You can learn more about Amazon S3 by visiting the official [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)

#### S3 Security and Bucket Policies
Securing your Amazon S3 (S3) buckets is crucial to protect your data and prevent unauthorized access. S3 provides various security features and mechanisms to help you control access and enforce security policies. One of the key components for securing S3 is the use of bucket policies. Bucket policies allow you to define fine-grained access controls at the bucket level, ensuring that only authorized entities can interact with your S3 resources.
You can have:
* User based 
    - IAM Policies - which API calls should be allowed for a specific user from IAM
* Resource based
    - Bucket Policies - bucket wide rules from the S3 console - allows cross account
    - Object Access Control List ACL - finer grain (can be disabled)
    - Bucket Access Control List ACL - less common (can be disabled)
* An IAM principle can access an S3 object if 
    - The user IAM permissions ALLOW it *OR* the resource policy allows it
    - *AND* there is no explicit DENY in the action 
* Objects can also be encrypted using encryption keys

##### Use Cases:
1. Access Control: Bucket policies enable you to control access to your S3 buckets based on a wide range of conditions. You can grant or deny permissions to specific AWS accounts, IAM users, IAM roles, or even to anonymous users. This level of granularity allows you to tailor access controls to meet the specific security requirements of your applications.
2. Secure Data Sharing: With bucket policies, you can securely share data stored in S3 buckets with other AWS accounts or external entities. By specifying the necessary permissions in the bucket policy, you can grant read or write access to selected parties while restricting access to others, ensuring data privacy and integrity.
3. Compliance and Regulatory Requirements: Bucket policies provide the means to enforce compliance and regulatory requirements for data stored in S3. You can set policies that dictate specific security controls, such as encryption requirements, access logging, or even geographical restrictions, to ensure compliance with industry regulations or internal security standards.

##### Tips:
1. Least Privilege Principle: Follow the principle of least privilege when defining bucket policies. Only grant the minimum permissions necessary for the intended actions. Regularly review and audit your bucket policies to ensure they align with your security requirements.
2. Regularly Monitor and Audit Policies: Periodically review and audit your bucket policies to ensure they are up-to-date and aligned with your organization's security standards. Regular monitoring helps detect any unintended access or unauthorized changes to the policies.
3. Enable Logging and Monitoring: Enable S3 server access logging to track access requests to your buckets. Additionally, utilize Amazon CloudTrail to monitor and log API activity related to your buckets. These logs provide valuable insights into the usage and potential security incidents involving your S3 resources.
4. Regularly Test and Validate Policies: Test your bucket policies by simulating different scenarios and access patterns to ensure they function as intended. AWS provides tools like IAM Policy Simulator and Access Analyzer to help you validate and troubleshoot your policies.

You can learn more about securing S3 and configuring bucket policies by visiting the official [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-iam-policies.html)

### S3 JSON-Based Policies
Amazon S3 (S3) allows you to define access controls and permissions using JSON-based policies. These policies provide a flexible and powerful way to manage access to your S3 resources at a granular level. By using JSON syntax, you can specify conditions, actions, and resources to define fine-grained access control rules for your buckets and objects stored in S3.

![](https://i.imgur.com/UuqbiJx.png)

#### How JSON Policies Work with S3:
1. JSON policies for S3 consist of a set of statements that define who has access to which resources and what actions they can perform. Each statement includes an "Effect" (allow or deny), a "Principal" (the entity that can access the resource), an "Action" (the specific actions that are allowed or denied), and a "Resource" (the S3 bucket or object that the policy applies to).
2. The "Effect" field determines whether the statement allows or denies the specified access. You can use multiple statements in a policy to define complex access control scenarios. For example, you can allow read access to a specific bucket for a particular IAM user, while denying write access to all other users.
3. The "Principal" field identifies the entity that the policy applies to. It can be an AWS account ID, an IAM user, an IAM role, or even anonymous users.
4. The "Action" field specifies the set of actions that are allowed or denied. These actions can include operations like `GetObject`, `PutObject`, `DeleteObject`, and more. You can specify wildcard characters (*) to allow or deny a range of actions.
5. The "Resource" field defines the S3 resources to which the policy applies. It can specify individual buckets, objects, or use wildcard characters to match multiple resources.

#### Tips:
1. Test and Validate Policies: Before applying a JSON policy to an S3 bucket, test and validate it using tools like IAM Policy Simulator. This helps ensure that the policy behaves as intended and provides the desired access control.
2. Follow the Principle of Least Privilege: When crafting JSON policies, adhere to the principle of least privilege. Grant only the necessary permissions required for the intended actions, minimizing the risk of accidental or unauthorized access to your S3 resources.
3. Regularly Review and Update Policies: Periodically review and update your JSON policies to reflect changes in your access requirements and security standards. This helps maintain the integrity of your access control mechanisms and ensures they align with your organization's evolving needs.
4. Utilize Policy Conditions: JSON policies allow you to specify conditions that must be met for the policy to take effect. Leverage policy conditions to enforce additional security controls, such as requiring requests to come from specific IP ranges or requiring the use of SSL/TLS encryption.

##### Making object in S3 public
1. Disable "Block all public access" option under Bucket > Permission 
2. Add a bucket policy which allows "GetObject" by all resources on the bucket

![](https://i.imgur.com/mkyJBp4.png)

You can learn more about JSON-based policies and how they work with S3 by visiting the official [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-bucket-user-policy-specifying-principal-intro.html)

### S3 Replication
Amazon S3 (S3) offers replication capabilities that allow you to automatically and asynchronously copy objects from one S3 bucket to another. This feature is useful for data replication, disaster recovery, and ensuring data availability across different regions. S3 provides two types of replication: Cross-Region Replication (CRR) and Same-Region Replication (SRR). Replicated objects use the same Version id key.

* If you enable replication, only new objects are replicated into other bucket. Optionally, you can replicate existing objects using S3 batch replication which replicates existing objects and objects that failed replication
* For delete operations, you can replicate delete markers from source to target (optional setting). Deletions with a version ID are not replicated to avoid malicious deletes from one bucket to another
* There is no "chaining" of replication so if bucket A has replication into bucket B, which has replication into bucket C then objects created in bucket A are not replicated to bucket 

#### Cross-Region Replication (CRR):
CRR enables automatic replication of objects from a source bucket in one AWS region to a destination bucket in another AWS region. It provides a disaster recovery solution by creating a redundant copy of your data in a different geographical location. With CRR, you can achieve low recovery time objectives (RTO) and recovery point objectives (RPO) by replicating your data in near real-time. CRR supports both same-account and different-account replication scenarios.

1. Creating a bucket in another region

![](https://i.imgur.com/i7ykSJ6.png)

2. Creating a replication rule. Object versioning has to be enabled on both source and target buckets

![](https://i.imgur.com/tY69p4A.png)

* By default, when S3 Replication is enabled and an object is deleted in the source bucket, Amazon S3 adds a delete marker in the source bucket only. This action protects data from malicious deletions
* If you have delete marker replication enabled, these markers are copied to the destination buckets, and Amazon S3 behaves as if the object was deleted in both source and destination buckets. Delete markets are replicated but not deletes so the destination bucket will still have the replicated object
    - a key name (or key) and version ID like any other object
    - It does not have data associated with it
    - It is not associated with an access control list (ACL) value
    - It does not retrieve anything from a GET request because it has no data; you get a 404 error
    - The only operation you can use on a delete marker is DELETE, and only the bucket owner can issue such a request

#### Same-Region Replication (SRR):
SRR allows you to replicate objects within the same AWS region. With SRR, you can replicate objects from a source bucket to a destination bucket within the same region, providing additional copies of your data for enhanced data durability and availability. SRR can be useful for scenarios such as data segregation or maintaining multiple copies of data for compliance purposes. SRR can be configured within the same AWS account only.

#### Tips:
1. Plan your replication architecture: Before setting up replication, consider your data replication needs, recovery objectives, and compliance requirements. Determine which regions or buckets need replication and define appropriate replication configurations.
2. Enable versioning on source and destination buckets: To ensure consistency and recoverability, enable versioning on both the source and destination buckets. Versioning allows you to track changes to objects and recover previous versions if needed.
3. Monitor replication status: Regularly monitor the replication status of your buckets to ensure that replication is functioning as expected. Use Amazon S3 replication metrics and
4. Amazon CloudWatch to track replication progress, lag time, and any replication errors.
5. Understand cost implications: Replication can incur additional data transfer costs between regions or within the same region. Review the AWS pricing documentation to understand the costs associated with data transfer and replication operations.

You can learn more about S3 replication, including configuring CRR and SRR, by visiting the official [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html)

### S3 Storage Classes
Amazon S3 (S3) offers multiple storage classes, each designed to provide a different combination of durability, availability, performance, and cost. Storage classes in S3 allow you to optimize the storage of your objects based on their access patterns and cost requirements. By choosing the appropriate storage class for your data, you can effectively balance performance and cost, ensuring your data is stored in the most suitable manner.

* S3 has a durability of 99.99999999999% of objects across multiple AZ for all storage classes
* S3 has 99.99% availability and this varies across storage classes

#### S3 Storage Classes
1. Standard: This is the default storage class and provides high durability, availability, and low latency. It is ideal for frequently accessed data and offers strong consistency for read-after-write operations. Good for big data analytics, mobile & gaming applications, content distribution, etc.
2. Intelligent-Tiering: This storage class is designed for data with unknown or changing access patterns. Intelligent-Tiering automatically moves objects between two access tiers: frequent access and infrequent access. It optimizes costs by automatically adjusting storage costs based on usage patterns.

![](https://i.imgur.com/cCXQPWA.png)

3. Standard-IA (Infrequent Access): Standard-IA is suitable for data that is accessed less frequently but requires rapid access when needed. It offers a lower storage cost compared to Standard, with a retrieval fee for accessing objects. Good for disaster recovery or backups.
4. One Zone-IA: This storage class is similar to Standard-IA but stores data within a single Availability Zone. It provides a lower-cost option for infrequently accessed data but does not have the same level of durability as Standard-IA.Good to store secondary backup copies on-premise data or data you can recreate.
5. Glacier: Glacier is designed for long-term archival and data retention. It offers the lowest storage cost but with longer retrieval times, ranging from minutes to hours. Glacier provides multiple retrieval options to accommodate various access requirements.
    - Glacier instant retrieval: Has millisecond retrieval, great for data accessed once a quarter and has a minimum storage duration of 90 days
    - Glacier flexible retrieval: Formerly S3 Glacier which is expedited (1 to 5 minutes data retrieval), standard (3 to 5 hours). bulk (5 to 12 hours) - free and also has a minimum storage duration of 90 days. 
6. Glacier Deep Archive: Glacier Deep Archive is the most cost-effective storage class for data archiving. It offers the lowest storage cost among all S3 storage classes but with 
longer retrieval times, typically ranging from 12 (standard) to 48 hours (bulk) with a minimum storage duration of 180 days.

#### Lifecycle rules
You can add a lifecycle rule in the management section of a bucket which lets you specify actions such as "Move current versions of objects between storage classes" and choose "Standard-IA" to move to Standard-IA class if the object after 30 days.

![](https://i.imgur.com/V8TZ7fx.png)

#### Tips
1. Analyze access patterns: Understand the access patterns of your data to determine the appropriate storage class. Frequently accessed data should be stored in Standard or Intelligent-Tiering, while infrequently accessed or archived data can be stored in Standard-IA, One Zone-IA, Glacier, or Glacier Deep Archive.
2. Leverage lifecycle policies: Use S3 lifecycle policies to automatically transition objects to different storage classes based on predefined rules. This helps optimize costs by moving data to more cost-effective storage classes as its access patterns change over time.
3. Consider data retrieval requirements: When selecting a storage class, consider the retrieval time requirements for your data. If rapid access is crucial, Standard or Intelligent-Tiering should be preferred, while Glacier and Glacier Deep Archive are suitable for data with longer retrieval time tolerance.
4. Monitor and adjust storage classes: Regularly monitor your data access patterns and make adjustments to the storage class as needed. This ensures that your data remains stored in the most cost-effective and appropriate storage class based on its changing requirements.

You can learn more about S3 storage classes, their features, and pricing details by visiting the official [AWS documentation](https://aws.amazon.com/s3/storage-classes/)