EC2
* Infra as server
* Storing data on virtual drives
* Distributing load across machines ELB
* Scaling the services using an auto scaling group
* OS: linux, windows, macos
* Think about cpu, ram
* How much storage
* Network attached EBS, EFS
* Hardware attached storage – EC2 instance store
* Network card: speed of card, what kind of public IP address you want
* Firewall rules: security groups
* Bootstrap script (config at launch) EC2 user data
* Bootstrapping means launching commands when a machine starts
* Only run once when the instance first starts
* E.g. install updates, software, common files, etc
* Runs as root user
* T2.mcro – 1 cpu, 1 gb ram, ebs storage only, low to moderate network performance
* Windows 7 or windows 8 use .ppk for PuTTY and .prem for higher with OpenSSH
* Instance may change public ip address when restarting. Private ipv4 never changes
* You can use EC2 instance types which are optimized for different purposes
* m5.2xlarge
* m – instance class. E.g. general purpose
* 5 – generation (by AWS)
* 2xlarge – size within the instance class
* General purpose is good for diverse workload such as webserver or code repo, has balance between compute, memory, networking
* Compute optimized is good for compute intensive tasks that require high performance processors such as batch processing, media transcoding, high performance web servers, high * performance computing, scientific modelling/ machine learning or dedicated gaming servers. Named like c5, c4, c3n, etc
* Memory optimized is good for fast performance workloads that process large data sets in memory such as high performance relational/ nonrelational databases, distributed web scale * cache stores, in memory databases optimized for BI and applications performing real-time processing of big unstructured data. Names start with R like R4, R4, or X16 X1, high memory or z1d.
* Storage optimizes is good for storage intensive tasks that require high, sequential read and write access to large data sets on local storage.
Use case:
* High frequency online transaction processing (OLTP) systems
* Relational & NoSQL databases
* Cache for in-memory databases for example Redis
* Data warehousing applications
* Distributed file systems
* Security group
* Fundamental of network security in AWS
* Controls traffic into and out of EC2 instances
* Security groups only contain allow rules and reference by IP or security group
* Access to ports
* Authorize IP ranges – IPv4 and IPv6
* Can be attached to multiple instances, locked down to a region/ VPC, does live “outside” the EC2 if traffic is blocked the instance won’t see it.
* It is good to maintain one separate security group for SSH access
* Can assign multiple security groups to an instance – makes it easier to set up auto scaling groups/ load balancing
* Ports to know
* 22 – SSH
* 21 – FTP
* 22 – SFTP upload file using SSH
* 80 – HTTP access unsecured website
* 443 – HTTPS access secured websites
* 3389 – RDP (Remote desktop protocol)