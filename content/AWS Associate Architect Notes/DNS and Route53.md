---
title: '"DNS and Route53"'
draft: 
tags:
  - AWS
  - route53
  - routing
  - dns
  - recordTypes
  - basics
---



## **Domain Name System (DNS) and Amazon Route 53 (Need to go through again)**

  
**Domain Name System (DNS)** 

  

Amazon Route 53 is an authoritative DNS system. An authoritative DNS system provides an update mechanism that developers use to manage their public DNS names. It then answers DNS queries, translating domain names into IP addresses so that computers can communicate with each other. 

  

## **Concepts** 

  

**Top-Level Domain (TLD)** : .com, .net, .org, .gov, .edu, and .io. TLDs are at the top of the hierarchy in terms of domain names 

  

**IP Addresses** : An IP address is a network addressable location. Each IP address must be unique within its network. For public websites, this network is the entire Internet. 

  

**Hosts :** Within a domain, the domain owner can define individual hosts, which refer to separate computers or services accessible through a domain. For instance, most domain owners make their web servers accessible through the base domain (example.com) and also through the host definition www (as in www.example.com). 

  

**Subdomains :** DNS works in a hierarchal manner and allows a large domain to be partitioned or extended into multiple subdomains. TLDs can have many subdomains under them. For instance, zappos.com and audible.com are both subdomains of the .com TLD (although they are typically just called domains). The zappos or audible portion can be referred to as an SLD. 

  

**Fully Qualified Domain Name (FQDN) :** Domain locations in a DNS can be relative to one another and, as such, can be somewhat ambiguous. A Fully Qualified Domain Name (FQDN), also referred to as an absolute domain name, specifies a domain’s location in relation to the absolute root of the DNS. 

This means that the FQDN specifies each parent domain including the TLD. 

  

A proper FQDN ends with a dot, indicating the root of the DNS hierarchy. For example, **mail .amazon.com** is an FQDN. Sometimes, software that calls for an FQDN does not require the ending dot, but it is required to conform to ICANN standards. 

  

**Zone Files** 

A zone file is a simple text file that contains the mappings between domain names and IP addresses. This is how a DNS server finally identifies which IP address should be contacted when a user requests a certain domain name. 

Zone files reside in name servers and generally define the resources available under a specific domain, or the place where one can go to get that information. 

  

**More About Zone Files** 

Zone files are the way that name servers store information about the domains they know. The more zone files that a name server has, the more requests it will be able to answer authoritatively. Most requests to the average name server, however, are for domains that are not in the local zone file. 

  

  

## **Record Types**

  

- **Start of Authority (SOA) Record :** A Start of Authority (SOA) record is mandatory in all zone files, and it identifies the base DNS information about the domain. Each zone contains a single SOA record. 

- SOA records following data 

- The name of the DNS server for that zone 
- Administrator of the zone 
- Current version of the data file 
- Number of seconds that a secondary name server should wait before checking for updates 
- Number of seconds that a secondary name server should wait before retrying a failed zone transfer 

  

- **A and AAAA :** Both types of address records map a host to an IP address. The A record is used to map a host to an IPv4 IP address, while AAAA records are used to map a host to an IPv6 address. 
- **A Canonical Name (CNAME)** : Canonical Name (CNAME) record is a type of resource record in the DNS that defines an alias for the CNAME for your server (the domain name defined in an A or AAAA record). 
- **A Name Server (NS)** : Name Server (NS) records are used by TLD servers to direct traffic to the DNS server that contains the authoritative DNS records. 
- **Sender Policy Framework (SPF)** : Sender Policy Framework (SPF) records are used by mail servers to combat spam. An SPF record tells a mail server what IP addresses are authorized to send an email from your domain name. For example, if you wanted to ensure that only your mail server sends emails from your company’s domain, such as example.com, you would create an SPF record with the IP address of your mail server. That way, an email sent from your domain, such as marketing@example.com, would need to have an originating IP address of your company mail server in order to be accepted. This prevents people from spoofing emails from your domain name. 
- **Service (SRV):** A Service (SRV) record is a specification of data in the DNS defining the location (the host name and port number) of servers for specified services. The idea behind SRV is that, given a domain name (for example, example.com) and a service name (for example, web [HTTP], which runs on a protocol [TCP]), a DNS query may be issued to find the host name that provides such a service for the domain, which may or may not be within the domain. 

  

## **Amazon Route 53 Overview** 

  

Amazon Route 53 is a highly available and scalable cloud DNS web service that is designed to give developers and businesses an extremely reliable and cost-effective way to route end users to Internet applications. 

  

**Domain registration**—Amazon Route 53 lets you register domain names, such as example.com. 

  

**DNS service**—Amazon Route 53 translates friendly domain names like www.example.com into IP addresses like 192.0.2.1. Amazon Route 53 responds to DNS queries using a global network of authoritative DNS servers, which reduces latency. To comply with DNS standards, responses sent over User Datagram Protocol (UDP) are limited to 512 bytes in size. Responses exceeding 512 bytes are truncated, and the resolver must re-issue the request over TCP. 

  

**Health checking**—Amazon Route 53 sends automated requests over the Internet to your application to verify that it’s reachable, available, and functional. 

  

**Hosted Zones:** A hosted zone is a collection of resource record sets hosted by Amazon Route 53. Like a traditional DNS zone file, a hosted zone represents resource record sets that are managed together under a single domain name. Each hosted zone has its own metadata and configuration information. 

  

  

## **Routing policy** 

  

**Simple:** This is the default routing policy when you create a new resource. Use a simple routing policy when you have a single resource that performs a given function for your domain (for example, one web server that serves content for the example.com website. 

  

**Weighted:** With weighted DNS, you can associate multiple resources (such as Amazon Elastic Compute Cloud [Amazon EC2] instances or Elastic Load Balancing load balancers) with a single DNS name 

  

**Latency-Based :** Latency-based routing allows you to route your traffic based on the lowest network latency for your end user (for example, using the AWS region that will give them the fastest response time). 

  

**Failover:** Use a failover routing policy to configure active-passive failover, in which one resource takes all the traffic when it’s available and the other resource takes all the traffic when the first resource isn’t available. Note that you can’t create failover resource record sets for private hosted zones. 

  

**Geolocation :** Route user based on their geolocation and set default resource if the user is pinging form unknown location. You cannot create two geolocation resource record sets that specify the same geographic location. You also cannot create geolocation resource record sets that have the same values for “Name” and “Type” as the “Name” and “Type” of non-geolocation resource record sets. 

  

**Health Checking:** Amazon Route 53 health checks monitor the health of your resources such as web servers and email servers. You can configure Amazon CloudWatch alarms for your health checks so that you receive notification when a resource becomes unavailable. You can also configure Amazon Route 53 to route Internet traffic away from resources that are unavailable. 

  

**Amazon Route 53 Enables Resiliency** 

  

  

Concepts to build an application that is highly available and resilient to failures. 

  

- **Elastic Load Balancing load balancer** is set up with cross-zone load balancing and connection draining. This distributes the load evenly across all instances in all Availability Zones, and it ensures requests in flight are fully served before an Amazon EC2 instance is disconnected from an Elastic Load Balancing load balancer for any reason. 
- Each Elastic Load Balancing load balancer also has an Amazon Route 53 health check associated with it to ensure that requests are routed only to load balancers that have healthy Amazon EC2 instances 
- The application’s failover environment (for example, fail.domain.com) has an Amazon Route 53 alias record that points to an Amazon CloudFront distribution of an Amazon S3 bucket hosting a static version of the application 
- The application is deployed in multiple AWS regions, protecting it from a regional outage