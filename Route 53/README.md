<a name=“aws-route-53></a>
#### Route 53

##### Overview
Amazon Route 53 is a highly available, scalable, fully managed and "Authoritative" DNS web service. It is designed to give developers and businesses an extremely reliable and cost-effective way to route end users to Internet applications by translating URL like `www.google.com` into the numeric IP addresses like `172.217.18.36` that computers use to connect to each other. 
    - Authoritative means that the customer (you) can update the DNS records.
Route 53 is also a domain registrar which lets register domains and also gives the ability to check the health of your resources. This is the only AWS service which provides 100% availability SLA.

##### DNS Terminology
- Name Server: resolves DNS queries
- Top Level Domain (TLD): .com, .us, .gov, .org, ...
- Second Level Domain (SLD): amazon.com, google.com
- Sub Domain: www.google.com 
- Fully Qualified Domain Name (FQDN): api.www.example.com
- Protocol: http://, https://

![](https://i.imgur.com/IXTdZds.png)

##### How DNS Works
- Web browser accesses `example.com`.
- Local DNS server asks Root DNS server (managed by ICANN - est sep 1998), resolves the top level domain to be `.com` and returns a response to ask local DNS server to check `.com` NS in IP 1.2.3.4. 
- The Local DNS server asks the `.com` TLD DNS server (managed by IANA - est dec 1988), resolves the `example.com`. It does know `example.com` but does not know which record it is in, and returns a response to ask local DNS sever to check `example.com` NS in 5.6.7.8.
- The local DNS server then goes to the SLD DNS server (managed by Domain Registrar), resolves the `example.com`. It knows that it is a record and returns the IP for the record `example.com` IP 9.10.11.12. 
- The Local DNS server TTL caches the IP address for next time if someone is trying access `example.com`.
- The web browser can now access the web server.
 
![](https://i.imgur.com/9WfapHM.png)

##### Use cases
Some use cases for Amazon Route 53 include managing network traffic globally, building highly available applications, setting up private DNS, and registering domain names. You can also use Amazon Route 53 to map your zone apex (example.com versus www.example.com) to your Elastic Load Balancing instance, Amazon CloudFront distribution, AWS Elastic Beanstalk environment, API Gateway, VPC endpoint, or Amazon S3 website bucket using a feature called Alias record.

##### Records
- Records determine how you want to route traffic to a specific domain.
- Each record contains:
    1. Domain/subdomain name: e.g. `example.com`
    2. Record Type: e.g. A or AAAA 
    3. Value: e.g. 12.34.56.78
    4. Routing Policy: how Route 53 responds to queries
    5. TTL: amount of time record cached at DNS resolvers
- Route 53 supports the following DNS record types:
    1. (must know) A/ AAAA/ CNAME/ NS
    2. (advanced, not need to know for exam) CAA/ DS/ MX/ NAPTR/ PTR/ SOA/ TXT/ SPF/ SRV

##### Record Types
- A: maps a hostname to IPv4
- AAAA: maps a hostname to IPv6
- CNAME: maps a hostname to another hostname
    - The target must be an A or AAAA record
    - Can't create a CNAME record for the top node of a DNS namespace (Zone Apex)
    - E.g. you can't create for `example.com` but you can for `www.example.com`
- NS: Name servers for the hosted zone
    - They are the DNS/ IP Addresses of the servers that can respond to DNS queries for hosted zone
    - Control how traffic is routed for a domain

##### Hosted Zones
A hosted zone is a container for records in Amazon Route 53. Records contain information about how you want to route traffic for a specific domain and its subdomains. A hosted zone and the corresponding domain have the same name. 

There are two types of hosted zones: public hosted zones and private hosted zones.
1. Public hosted zones contain records that specify how you want to route traffic on the internet.

![](https://i.imgur.com/HXu0J8C.png)

2. Private hosted zones contain records that specify how you want to route traffic in one ore more Amazon VPC.

![](https://i.imgur.com/q9ulRq5.png)

There is a charge of $0.50 per month per hosted zone.

##### Registering a domain

1. Choosing an available domain name. You then enter contact details, select if you want to automatically renew the domain as well as enabling privacy protection for hiding contact details for .com domains.

![](https://i.imgur.com/WCsQZx1.png)

2. Once it is registered, you can see the details below.

![](https://i.imgur.com/WY1Jb90.png)

3. A hosted zone should also have been created for you by Route 53. On the hosted zone's records tab you should see the new records. For the domain name, the servers on the right are the authoritative servers that give you the values of the records in a table. 

![](https://i.imgur.com/PPzOIRL.png)
![](https://i.imgur.com/PmWhoYl.png)

4. Creating A type record for `test.domain.com`to redirect to an IPv4 address of value 11.22.33.44.

![](https://i.imgur.com/0h9CAvz.png)

5. You can use `nslookup test.domainname.com` to get the address answer for corresponding domain name. Or you can also use the `dig test.domainname.com` command to get the TTL, resolved IP addresses, as well as the record type.

![](https://i.imgur.com/LVpHXaN.png)
![](https://i.imgur.com/6fCr9dh.png)

##### Records TTL (Time To Live)

When a client sends a DNS request such as `myapp.example.com` to Route 53, a response of A record with an IP address of 12.34.56.78 might be sent back, alongside a TTL. This TTL value represents the amount of time the client should cache the result IP address. For example, the client could cache it for 300 seconds. Subsequently, any DNS request with the cached result will return the cached result if it is within the 300 seconds and will not issue a request to the DNS. We don't except the records to change a lot hence why this is a good approach.
- High TTL such as 24 hr will result in less traffic on Route 53 as the result IP address is cached for longer, but could result in stale records.
- Low TTL such as 60 sec will result in more traffic on Route 53, which is more costly. The records are outdated for less time and it is easier to change records.
- You could decrease the TTL for everyone if there are changes to records that you will be making, and after you could change the TTL back to high. 
- Except for Alias records, TTL is mandatory for each DNS record.

![](https://i.imgur.com/qzXqyzg.png)

In the answer section of the `dig demo.domainname.com` query, you can see 98 seconds which represents the TTL for that particular cached result along the IP address. If any changes to the records are made, such as a new target IP address for the domain the new IP address response will be reflected once the TTL is over.

![](https://i.imgur.com/qzf3mXh.png)

##### CNAME vs Alias
AWS resources such as load balancers, CloudFront... expose an AWS hostname such as `lbl-1234.us-east-2.elb.amazonaws.com` as `myappdomain.com`. 
- CNAME:
    - You can point a hostname to any other hostname (Canonical name). (app.mydomain.com => test.anotherdomain.com). This **only** works for non root domain e.g. `something.mydomain.com`.

![](https://i.imgur.com/AgfvPWb.png)

- Alias:
    - Points a hostname to an AWS Resource (app.mydomain.com => test.amazonaws.com). This works for **both** root domain and non root domain. `mydomain.com` and `something.mydomain.com`. Alias is free or charge and has native health check.

![](https://i.imgur.com/DEal9rD.png)

    - You can also create an A type Alias record type which routes to an application load balancer at the zone apex. E.g. `mydomain.com`. 

![](https://i.imgur.com/AyGwOYd.png)

##### Alias Records 
Alias records map a hostname to an AWS resource. This is an extension to DNS functionality, as if the underlying Application Load Balancer changes IP, the Alias record will recognize it. Unlike CNAME, it can be used for top node of a DNS namespace (Zone Apex). For example `example.com`. Alias Record is always of type A/AAAA for AWS resources like IPv4 and IPv6. The TTL is set automatically by Route 53. 

![](https://i.imgur.com/3oZL3zi.png)

##### Alias targets
1. Elastic Load Balancers
2. CloudFront Distributions
3. API Gateway
4. Elastic Beanstalk environments
5. S3 Websites
6. VPC Interface Endpoints
7. Route 53 record in the same hosted zone
You cannot set an Alias record for an EC2 DNS name

#### Routing Policy
Amazon Route 53 routing policies determine how Route 53 responds to DNS queries. When you create a record, you choose a routing policy based on your needs. Here are some of the routing policies available:

1. Simple routing policy: Use for a single resource that performs a given function for your domain, for example, a web server that serves content for a website.

![](https://i.imgur.com/c9xPAPx.png)


2. Failover routing policy: Use when you want to configure active-passive failover.

![](https://i.imgur.com/Kt0PgdE.png)

* Primary record

![](https://i.imgur.com/CZOWNoX.png)

* Secondary record

![](https://i.imgur.com/NYR96sz.png)

3. Geolocation routing policy: Use when you want to route traffic based on the location of your users
    * You can create records that point to different IP addresses based on location. E.g. IP X for US

![](https://i.imgur.com/6R3B2vc.png)

4. Geoproximity routing policy: Use when you want to route traffic based on the location of your resources and, optionally, shift traffic from resources in one location to resources in another
    * AWS Traffic Flow: No bias results traffic going to nearby resource

![](https://i.imgur.com/wpRGJob.png)

    * Higher bias in region: More traffic going to resource with higher bias

![](https://i.imgur.com/X7EpUnI.png)

5. Latency routing policy: Use when you have resources in multiple AWS Regions and you want to route traffic to the region that provides the best latency.
    * You create multiple records with same record name in a domain which points to different IP addresses based on different region. 

![](https://i.imgur.com/jV52ctW.png)

6. IP-based routing policy: Use when you want to route traffic based on the location of your users, and have the IP addresses that the traffic originates from.
7. Multivalue routing policy: Use when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random by the client. 
8. Weighted routing policy: Use to route traffic to multiple resources in proportions that you specify.
    * You can control the % of requests that go to each resource
    * Assign each record a relative weight:
        - Traffic % = weight for a specific record / sum of all weights for all records
        - Weights don't need to sum to 100 
    * DNS records must have the same name and type 
    * Can be associated with Health Checks
        - HTTP health check are for public resources mainly
        - It can be used to have automated DNS failover
            1. Health checks can monitor an endpoint such as an ALB, server or other sources
            2. Health checks can monitor other health checks (Calculated health checks)
            3. Health checks can monitor CloudWatch Alarms (full control) - E.g. throttles DynamoDB, alarms on RDS, custom metrics, etc
        - These health checks can be viewed on CloudWatch metrics
    * Use cases include:
        1. Load balancing between regions
        2. Sending some traffic with new application versions for testing
        3. Sending traffic to a backup if the primary is unavailable
    * Assigning a weight of 0 to a record stops sending traffic to a resource
    * If all records have a weight of 0, then all records will be returned equally 

When Alias is enabled, specify only one AWS resource. 

Some common use cases for these routing policies include optimizing network transit costs or performance, localizing content, and presenting some or all of the website in the user’s language. For more information about Amazon Route 53 and its use cases, you can refer to the official AWS documentation

For more information about Amazon Route 53 and its use cases, you can refer to the official [AWS documentation](https://aws.amazon.com/route53/)

 ##### Health Checks - Endpoint monitoring
AWS provides health check monitoring through Amazon Route 53. Route 53 health checks integrate with CloudWatch metrics so that you can verify that a health check is properly configured, review the status of a health check over a specified period of time, and configure CloudWatch to send an Amazon SNS alert when the status of a health check is unhealthy

![](https://i.imgur.com/lyHEyMY.png)
![](https://i.imgur.com/GGMXqpd.png)

When creating a health check, you can specify values such as:
    1. the IP address or domain name of the endpoint you want to monitor, 
    2. the protocol you want Route 53 to use to perform the check (HTTP, HTTPS, or TCP)
    3. how often you want Route 53 to send a request to the endpoint (the request interval) e.g. 30 sec. Can set it to lower but would lead to a higher cost
    4. how many consecutive times the endpoint must fail to respond to requests before Route 53 considers it unhealthy (the failure threshold). E.g. 3 times by default

Route 53 starts sending requests to the endpoint at the specified interval. If > 18% of health checkers report the endpoint is healthy, Route 53 considers it healthy and takes no action. If the endpoint doesn’t respond to a request, Route 53 starts counting the number of consecutive requests that the endpoint doesn’t respond to. If this count reaches the specified failure threshold, Route 53 considers the endpoint unhealthy.

* Health checks pass only when the endpoint responds with the 2xx and 3xx status codes
* Health checks can be set up to pass / fail based on the text in the first 5120 bytes of the response
* Configure your router/firewall to allow incoming requests from Route 53 health checkers


##### Calculated Health Checks
* You can combine the results of multiple health checks into a single health check 
* You can use OR, AND, or NOT
* You can monitor up to 256 Child Health Checks
* Specify how many of the health checks need to pass to make the parent pass
* E.g. you can perform maintenance to your website without causing all health checks to fail 

![](https://i.imgur.com/YruW3A3.png)


##### Private Hosted zone Health Checks
* Route 53 health checkers are outside the VPC
* They can't access private endpoints (private VPC or on-premise resources)
* You can create a CloudWatch Metric and associate a CloudWatch Alarm, then create a Health Check that checks the alarm itself

![](https://i.imgur.com/7NtNPbL.png)


### Traffic Flow

![](https://i.imgur.com/HHrrQQG.png)

#### Overview
Amazon Route 53 Traffic Flow is a service that simplifies the process of creating and maintaining records in large and complex configurations. It allows you to create complex trees of records and see the relationships among the records using a visual editor. Each configuration is known as a traffic policy. You can create multiple versions of a traffic policy so you don’t have to start all over when your configuration changes

![](https://i.imgur.com/R9252q9.png)

##### Geoproximity map

![](https://i.imgur.com/E6VN9Vq.png)

##### Pricing

Traffic flow policy records can be expensive to set up as the policy record costs monthly. E.g. $50 per month

![](https://i.imgur.com/TKzJIlr.png)

#### Use Case
Route 53 Traffic Flow can be used to manage related records in a hosted zone when you have a lot of resources that perform the same operation, such as web servers that serve traffic for the same domain. You can use a traffic policy to automatically create records in multiple public hosted zones

#### Tips
1. Configure AWS Route 53 health checks to give your application resiliency and automated failover
2. Configure short TTLs in your DNS. Typically, 300 seconds should be sufficient to move traffic away from a data center relatively quickly, but your use case may require longer or shorter TTLs

You can learn more about Route 53 Traffic Flow by visiting the official [AWS documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/traffic-flow.html)

#### IP-based Routing
Amazon Route 53 IP-based routing allows you to route traffic to resources of your domain based on the client subnet. With IP-based routing, you can fine-tune your DNS routing by using your understanding of your network, applications, and clients to make the best DNS routing decisions for your end users. IP-based routing gives you granular control to optimize performance or reduce network costs by allowing you to upload your data to Route 53 in the form of client IP to location mappings.

* You provide a list of CIDRs for your clients and the correspondent endpoint (IP to endpoint mapping)

![](https://i.imgur.com/VYUPD4L.png)

##### Use Case
Some common use cases for IP-based routing are:
1. You want to route end users from certain ISPs to specific endpoints in order to optimize network transit costs or performance.
2. You want to add overrides to existing Route 53 routing types such as geolocation routing based on your knowledge of your clients’ physical locations.

##### Tips
1. Configure AWS Route 53 health checks to give your application resiliency and automated failover.
2. Configure short TTLs in your DNS. Typically, 300 seconds should be sufficient to move traffic away from a data center relatively quickly, but your use case may require longer or shorter TTLs.

You can learn more about Route 53 IP-based routing by visiting the official [AWS documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-ipbased.html)

### Multivalue Answer Routing
Amazon Route 53 Multivalue Answer Routing lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries. You can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each resource, so Route 53 returns only values for healthy resources. It’s not a substitute for a load balancer, but the ability to return multiple health-checkable IP addresses is a way to use DNS to improve availability and load balancing.

* Up to 8 healthy records are returned for each multi value query and the client can pick a healthy resource to access

![](https://i.imgur.com/AtXhZQ0.png)

#### Use Case
When a client makes a DNS request with multivalue answer routing, Route 53 responds to DNS queries with up to eight healthy records selected at random for the particular domain name. These records can each be attached to a Route 53 health check, which helps prevent clients from receiving a DNS response that is not reachable.

#### Tips
1. To route traffic approximately randomly to multiple resources, such as web servers, you create one multivalue answer record for each resource and, optionally, associate a Route 53 health check with each record.
2. If you associate a health check with a multivalue answer record, Route 53 responds to DNS queries with the corresponding IP address only when the health check is healthy.

You can learn more about Route 53 Multivalue Answer Routing by visiting the official [AWS documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-multivalue.html)

### Domain Registrar 
* You can buy or register your domain name with a Domain Registrar typically by paying annual charges (GoDaddy, Amazon Registrar, etc)
* The Domain Registrar usually provides you with a DNS service to manage your DNS records, but you can also use another DNS service to manage your DNS records 
    1. You need to create a Hosted Zone in Route 53  
    2. You need to ensure that when you use a service like GoDaddy that your nameservers (NS) records that GoDaddy uses is set to the nameservers from Route 53 like `ns-1083.awsdns-07.org`

![](https://i.imgur.com/zqljhk5.png)