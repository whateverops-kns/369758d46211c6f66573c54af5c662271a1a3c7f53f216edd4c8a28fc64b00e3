# Regions and AZs

Owner: Khant
Tags: AWS

# Regions

- Regions are all around the world.
- Regions are cluster of datacentres.
- Most AWS services are region-scope.
- How to choose a region
    - **Compliance**
    - **Proximity**: nearest location to users
    - **Available services**: not all services are available in all regions
    - **Pricing:** Pricing depends on the region.

# AZs

- Each region has multiple zones, usually 3, min is 2, max is 6.
- Each AZ has one or more data centers.

# Edge Locations aka Point of Presence

- AWS has 216 points of presence, 205 edge locations in 84 cities across 42 countries
- Content is delivered to end users with lower latency

# AWS global and regional Services

- Global
    - IAM
    - Route53
    - CloudFront
    - WAF
- Regional
    - EC2
    - Elastic Beanstalk
    - Lambda, etc…