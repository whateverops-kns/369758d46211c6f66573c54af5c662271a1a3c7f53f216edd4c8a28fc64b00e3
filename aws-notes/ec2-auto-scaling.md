# EC2 HA and Scaling

Owner: Khant

- Database are scale vertically
- Modern web app are scale horizontally
- HA
    - Avoid data lose
    - No single point of failure

# ELB (Elastic Load Balancing)

- What is ELB and Use case
    - Forward traffics to multiple servers across multi AZ
    - Spread load across multiple downstream instances
    - Expose a single DNS access
    - HA across multiple AZ
    - Separate layers in System
    - Provide SSL for websites
    - Health Check
- AWS Managed service
- Can be integrate with EC2, EC2 ASG, ECS, ACM, CloudWatch, Route53, AWS WAF, AWS Global Accelerator
- Load balancer use port or route to health check
- Load balancer can be public or private
- 4 kinds of Load Balancer
    - Classic Load Balancer
        - Very old
    - Application Load Balancer (ALB)
        - Http, Https, WebSocket
    - Network Load Balancer
        - TCP,TLS, UDP
    - Gateway Load Balancer
        - Operates at Layer 3 (Network Layer)
- Load balancers also has Security Group

# Application Load Balancer (ALB)

- ALB is on Layer 7 (HTTP)
- Load balance to multiple http application across machines via target group
- Load balance to multiple app on the same machine
- Support HTTP/2 and WebSocket
- Support redirects
- Routing tables to different groups
    - Route based on path URL
    - Route based on path domain
    - Route base on query string
- ALB is basically microservice router or app load balancer
- Good for ECS or microservice based application
- Has a port mapping to ECS dynamic port
- Target group
    - EC2 instances
    - ECS tasks
    - Lambda functions
    - IP Address
- ALB can be route to multiple target group
- ALB facts
    - Fixed hostname
    - client IP are forwarded with X-Forwarded-For header
    - Port and Proto are in X-Forwarded-Port and X-Forwarded-Proto
- Can route to specific webpage or service depend on rules
    - Rule can be anything path or query string or host.

# Network Load Balancer (NLB)

- Operate on Layer 4 (TCP/UDP)
- Handle millions of request per second
- High performance and Low latency
- NLB can be assigned with Elastic IP, useful for IP whitelisting
- Extreme performance
- Not a free tier service
- Load balance based on protocols (TCP or UDP)
- Target Groups
    - EC2 Instances
        - Must be private IPs
    - IP address
- NLB in front of ALB is best for leveraging performance and routing rule form ALB
- Not good for microservice applications
- Good for replication HA app

# Gateway Load Balancer (GWLB)

- Run at network level
- Operate at lowest level (Layer 3)
- For 3rd party network virtual appliances
    - SIEM
    - Deep Packet inspection
    - Payload manipulation
- Use third party service to validate the traffic and drop or redirect the request to destination depends on the response from the third party service
- For example: GWLB use third party security tools to validate the request and depends on that request it will redirect to application.
- Single entry and single exit
- Distributed traffic across 3rd party services
- For GENEVE protocol
- Target group
    - EC2 instances
    - IP Addresses

# ALB vs NLB vs GWLB

| ALB | NLB | GWLB |
| --- | --- | --- |
| Layer 7 | Layer 4 | Layer 3 |
| EC2 instances/ Lambda Function/ IP Address/ ECS tasks | EC2 instances/ IP addresses (Must be private IP addresses) | EC2 instances/ IP addresses |
| Application level routing | Protocol level routing | Network level routing |
| Microservice router | Load balancer across replicate instances | Security  |
| Cross Az | Can attach to IP |  |
| Support HTTP, HTTPS, WebSocket | HA |  |
| Can configure HTTP to HTTPS redirect | Support HTTP, HTTPS, WebSocket, TCP, UDP |  |

# Sticky Sessions

- Use for redirect the same client to same instance behind the load balancer
- Support CLB, NLB, ALB
- ALB need cookie for sticky sessions
- Use case: undistributed session data persist
- Trade off: Imbalance load
- Sticky Cookie types:
    - Application based cookie
        - Custom Cookie
        - Application Cookie
    - Duration based cookie or Load Balancer generated cookie
- Cookie in ALB only made sense

# Cross Zone Load Balancing

- Distributed load evenly across the AZs
- Without cross zone load balancing, load are restricted to each AZs.
- ALB enable cross zone by default, no extra charge
- NLB and GLWB disabled by default, extra charge if enabled
- Target group can also inherit from Load balancer or on/off cross zone setting.

# SSL/TLS Certificates

- SSL cert allow traffic between clients and load balance to be encrypted
- Load Balancer usually connect EC2 instance using HTTP but since EC2 are inside VPC these approach is secure.
- SNI (Server Name Indication)
    - SNI solves multiple SSL certs in one web server
    - Newer protocol
    - Only support in ALB and NLB
    - Basically Different SSL certs on Load balancers
- ALB and NLB support multi certs with multi listeners
- Can leverage ACM for certs management or Can import custom certs

# Connection Draining

- Deregistration delay for ALB and NLB
- Stop sending requests to unhealthy EC2
- Set low value if request are short

# Auto Scaling Group

- For scale in/out EC2 instances
- ASG tight coupled with Load Balancers
- For auto scaling the EC2 instances
- ASG launch template
    - AMI + instances types
    - Security configs
    - SSH
    - IAM roles
    - Network configs
    - etc…
- Scaling policy
    - ASG can scale based on cloud watch alarms
    - Based on alarms we can scale in or out.
    - Types of Scaling Policies
        - Dynamic
            - Target Tracking Scaling
                - Scale depend on CPU usage percentage
            - Simple/Step scaling
                - Scale depend on cloud watch alarm
                - Cloud watch alarm will be custom
            - Scheduled Actions
                - Scaling based on usage pattern
                - Schedule the scale in/out policy depends on time
        - Predictive Scaling
            - Forecast scaling depends on historic time series data
            - AI Powered
    - Good Metrics to Scale on
        - CPU Utilization
        - Request Counts per Target
        - Average Network In/Out
        - Any custom metrics
    - During cooldown ASG will not launch or terminate instances
    - Use ready to use AMI to reduce configuration time.