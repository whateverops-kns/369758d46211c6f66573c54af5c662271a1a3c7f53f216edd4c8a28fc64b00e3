# EC2

Owner: Khant
Tags: AWS

# Basics

- Core service
- Rent VM
- Store virtual devices EBS
- IaaS
- Capability
    - Renting VM
    - Storing data (EBS)
    - Load balancing (ELB)
    - Scaling (ASG)
- Supported OS
    - Window
    - Linux
    - Mac
- Support storage
    - EBS
    - EFS
- User data for bootstrapping the dependencies and configurations
    - Only run at first start
- EC2 has root volume which is (EBS) and can add more EBS volume
- Private IP are static but public IP are change on stop and start instance
- Types of instances
    - General Purpose
        - Diversity of workload
        - Balance
    - Compute Optimized (For CPU intensive)
        - Batch processing
        - Media Transcoding
        - High Performance webserver
        - Gaming Server
        - Machine Learning
    - Memory Optimized (For high memory)
        - In memory databases
        - BI
        - Data processing
    - Accelerated Computing
    - Storage Optimized (For storage and data)
        - OLTP systems
        - Databases
        - Cache
        - Data warehousing
        - Distributed file systems
    - Instance Features
    - Measuring instance performance
- EC2 types naming conversion
    - m5.2xlarge
        - m: instance class
        - 5: generation
        - 2xlarge: size within the instance class

# Security Groups

- SG can point to another SG group
- Can allow or deny
- Has outbound an inbound
- Basically a firewall for EC2 (Outer layer)
- Can be attached to multiple instances
- Locked to one region or one VPC
- Good to have one separate security group for SSH access

# Purchasing Options

- On-Demand instances - default, short workload, predictable pricing
    - Pay for what you use
    - Billing per second (Linux and Windows)
    - Billing per hour (Other OS)
    - Hight cost/No upfront
    - No long term commitment
    - Recommended for **short-term** and **un-interrupted workloads**
    - **Unpredictable app deployment**
- Reserved instances
    - Up to 72% discount
    - Reserve specific instance
    - Partial Upfront/No upfront/ All Upfront
    - Can sell in marketplace
    - Reserved 1 to 3 years
    - Long workload
    - Predictable app deployment and database deployment
    - Convertible reserved instances: long workloads with flexible instances
        - 66% discount
- Saving Plans
    - 1 to 3 years
    - Commitment to an amount of usage, long workload
    - Instance can change overtime
    - Flexible instance types
- Spot Instances and Spot fleets
    - Discount 90%
    - Can defined max spot price
    - Spot instance can be block max to 6 hours
    - Most cost efficient
    - Short workloads
        - ETL
        - Batch jobs
        - Distributed jobs
    - Cheap
    - Can lose anytime
- Dedicated Plans
    - Dedicated host
        - Billing per hardware specs
        - On demand or reserved
        - Most expensive
        - Suitable for BYOL
        - Strong compliance need app
        - Lower level visibility
- Dedicated Instances
    - No other customers will share the hardware
    - May share hardware with other instances in same account
    - Not access to physical server
- Capacity Reservations
    - Reserve on-demand capacity in specific AZ for any duration
    - No discounts
    - Short term-uninterrupted workloads

# Placement Groups

- **Cluster** – Packs instances close together inside an Availability Zone. This strategy enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication that is typical of high-performance computing (HPC) applications.
- **Partition** – Spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions. This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka.
- **Spread** – Strictly places a small group of instances across distinct underlying hardware to reduce correlated failures.
- Use for control over how EC2 instance are going to place in AWS
- Types of Instance group
    - Cluster
        - Clusters instance into low-latency group in single AZ
        - High Performance/ High Risk
        - Same AZ
        - For big data job
        - For low latency app
        - Single point of failure
    - Spread
        - Spread instance across the AZ
        - Max 7 instances per AZ
        - Reduce single point of failure
        - For HA app
        - For critical apps
    - Partition
        - Spreads instance across many different partitions
        - Across multiple AZ
        - One partition can have multiple instances
        - Basically cluster in multi AZ
        - HA
        - 100s instance per partition
        - Each partition are isolated to each AZ
        - Big Data database
            - HDFS
            - HBase
            - Cassandra
    
    # ENI (Elastic Network Interface)
    
    - Logical component in VPC
    - Virtual network card
    - ENI can have following attributes
        - Primary private IPv4
        - One elastic IP
        - One public ipv4
        - One or more SG
        - A MAC address
    - Bound to single AZ
    
    # EC2 Hibernate
    
    - On stop EBS will persist
    - On terminate EBS will deleted
    - On hibernate
        - RAM state will written to EBS
        - The root EBS volume must be encrypted
    - Use cases
        - Long Running processing
        - Saving RAM state
        - Services that take time to initialize