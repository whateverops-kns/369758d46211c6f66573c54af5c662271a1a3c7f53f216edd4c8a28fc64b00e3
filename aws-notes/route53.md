# Route 53

Owner: Khant

# DNS Base Knowledge

- DNS Records: A, AAAA, CNAME, NS, etc….
- Zone File: Contains DNS records
- Name Server: resolved DNS queries
- Top Level Domain (TLD): .com, .us, .in, .gov
- Second Level Domain (SLD): amazon.com, google.com

# What is Route 53?

- Highly available, scalable, fully managed service
- Also a domain register
- Has the ability to health check the resources
- Only AWS services which provides 100% SLA

Records

- Route traffic for a domain
- Each record contains:
    - Domain/Sub Domain Name
    - Record Type: AAAA or AA or CNAME
    - Value: IP Address
    - Routing Policy: Simple or Weighted or Latency or etc…
    - TTL: cache at DNS resolvers
- Route 53 record types:
    - Must know: A, AAAA, CNAME, NS
    - Advance (Not mandatory for exam): CAA, DS, MX, NAPTR, PTR, SOA, TXT, SPF, SRV
- A: maps a hostname to IPv4
- AAAA: maps a hostname to IPv6
- CNAME: maps a hostname to another hostname
    - Cannot directly map to IP or top node of the DNS namespace (example.com)
- Alias: maps a hostname to an AWS resource
    - Works for both root domain and non root domain
    - Free
    - Native health charge
    - For ELB
    - Cannot set TTL, set by route 53
    - Cannot set for EC2 DNS name
    - Basically native AWS record
    - Must BE A or AAAA record type
- NS: Name servers for the hosted zone
    - Control how traffic is routed for a domain

# Health Checks

- HTTP health checks are only for public resources
- Health Check types:
    - Health checks that monitor the endpoint
    - Health checks that monitor other health checks
    - Health checks that monitor CloudWatch Alarms
- Each health checks are integrated with CW metrics
- Cannot check private resource directly, need CW alarm for this use case

# Routing Policies

- Simple:
    - Route traffic to single resource
    - Can have multiple values but not recommended since it choose return value from DNS queries randomly
    - With alias, user can specify only one AWS resource
    - Can’t associated with heath checks
- Weighted:
    - Control the % of the request that got to each specific resource
    - Assign each records a relative weight. Records must have the same name and type
    - Can be associated with health checks.
    - Use case: Load balancing between regions, testing new app versions
- Latency
    - Redirect to the resource that has the least latency close to user
    - Latency is based on traffic between users and AWS regions
    - Can be associated with health checks
- Failover
    - Automatically route the request to failover instance based on health checks
- Geolocation
    - Different from latency based
    - Based on user precise location
    - Should create default record in case there is no match on location
    - Use case: Website localization, restrict content distribution
    - Can be associated with Health Checks
- Geoproximity
    - Geolocation features with bias
    - Can shift more traffic to resources based on the defined bias
    - Basically geolocation with weight
    - Use case: Load balancing to region which has more computing power, shift traffic to specific region.
- IP Routing
    - Routing based on clients IP addresses
    - Provide the list of CIDRs for your clients and the corresponding endpoints and locations
    - Use case: reduce network costs, route end users from a particular ISP to a specific endpoint
- Multi Value
    - Routing traffic to multiple resources
    - Can be associated with Health Checks
    - Return only values for healthy resources
    - Up to 8 healthy records are returned for each multi-value query
    - Not a substitute for having an ELB