# Networking

Owner: Khant
Tags: AWS

# VPC

- CIDR: Classless Inter-Domain Routing - Method for allocating IP address
    - Base IP

# NAT instance

- EC2 instance use for NAT gateway
- Old service, not need for exam (I think)
- Must disable EC2 source/destination check
- Must have elastic IP address
- Must be in public subnet and configured to route traffic from private subnet via route table
- NAT instance are not HA

# NAT Gateway

- AWS managed NAT
- Higher bandwidth, HA, zero administration
- NATGW is created in a specific AZ
- Use elastic IP
- Require IGW
- No security groups are required
- Must be in public subnet and configured to route traffic from private subnet via route table
- Deployed NAT gateway in multiple AZ for fault tolerance
- No need for manual failover.
- 100GBPs bandwidth

# NACL and Security Groups

- NACL: subnet level firewalls
- One NACL per one subnet
- Rules has number which are priority, highest priority rule will assumed first
- Great for blocking specific IP address at subnet level
- NACL is stateless, SGs are stateful
- NACL need to defined both inbound and outbound rules for reply but for SGs, that isn’t required.
- Default NACL will allow/deny everything.
- Create new one instead of modifying existing one
- Need to open ephemeral ports on NACL since client defined ephemeral ports randomly to accept response.
- Return traffic are not automatically allowed on NACL.

# VPC Peering

- Connect two VPC privately via AWS network
- All VPC in peering need to enabled peering to connect each other. (three ways)
- Must not have overlapping CIDR
- Must update route tables in each VPC subnets to ensure the communications.
- Use Case:
    - Can happen between cross accounts/cross regions.
    - Can reference other VPC SGs in current SG

# VPC Endpoints

- Services inside VPC access other AWS services via AWS network without going through internet.
- Redundant and scale horizontally
- In case of issues check DNS settings :3
- Types of Endpoints
    - Interface endpoints: (only on premises, Site to Site VPN, another VPC, Direct Connect)
        - Powered by PrivateLink
        - Provision as ENI - private IP address as an entry point
        - Supports multiple AWS Services
        - Costly ($ per hour + $ per GB)
    - Gateway endpoints (Prefer in exam)
        - Free
        - Provision a gateway via target in route table
            - Traditional VPC way
        - Support only S3 and DynamoDB
        

# AWS Site to Site VPN

- Virtual Private Gateway (VGW)
    - VPN concentrator on the AWS side of the VPN connection
    - VGW is attach to the VPC to create the Site-to-Site VPN
    - possibility to customize the ASN
- Customer Gateway (CGW)
    - Software application or physical device on customer side of the VPN connection
    - Which IP address to use
        - Public: Directly connect
        - Private: Behind NAT device
- Enable route propagation for the VPG in route table to ensure the site-to-site VPN connection to connect the AWS VPC
- AWS VPN Cloud Hub
    - Allow multiple on-premise to connected via Site-to-Site VPN connection
    - Low cost
    - Enable dynamic routing and configure route table
    - Goes over public internet

# AWS Direct Connect

-