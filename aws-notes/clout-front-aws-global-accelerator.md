# Cloud Front and AWS Global Accelerator

Owner: Khant

- AWS native CDN
- Improves read performance, improves user experience
- 216 Point of Presence globally
- DDOS Protection, integration with Shield, AWS web application firewall
- Edge location around the world
- Cache the requests in edge.

# Cloud Front - Origins

- S3 bucket
    - For distributing files and caching them ate the edge
    - Enhanced security with OAC
    - Ingress file upload to S3
- Custom Origin (HTTP)
    - ALB
    - EC2
    - S3 website (s3 must first enable the bucket as a static S3 website)
    - Any HTTP custom backend
- Geo restrict
    - Allow/Deny content based on geolocation - Geo-IP (Country in console)
    - Use case: Copyright laws to control access to content
- Pricing
    - Edge location price are differ based on country
    - Price classes
        - To reduce the number of edge locations for cost
        - Three price classes
            - All regions
            - Price class 200: most regions
            - Price class 100: only the least expensive regions
- Validate cache
    - use wild card

Cloud Front vs S3 Cross Region Replication

| Cloud Front | S3 Cross Region Replication |
| --- | --- |
| Global Edge network | Must be setup for desired regions for replication |
| File are cached for a TTL | Near real time update |
| Great for static content  | Read only/ Great for dynamic content |

# Use cases:

- Access S3 website without making objects public or pre-signed URL via OAC
- EC2 backend must be public
- ALB backend must be public

# AWS Global Accelerator

- Minimize latency for global applications
- Leverage AWS internal global network to route your application
- Use anycast IP to traffic directly to edge locations
    - Assign two same IP to applications
- Support services
    - ELB
    - EC2
    - IP
- Can health check
- Automatic failover
- Great for DR
- DDoS protection via AWS shield

## AWS Global Accelerator vs Cloud Front

| AWS GA | Cloud Front |
| --- | --- |
| Improves performance for cacheable content (images, videos) | Improve performance for wide range of apps over TCP or UDP |
| Dynamic Content | Good fit for non-HTTP use case (Gaming, MQTT - IOT, Voice over IP) |
| Content is served at the edge | Good fit for http use case that require static IP |
|  | Good for fast regional failover |