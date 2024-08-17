# Storage Volumes and AMI

Owner: Khant
Tags: AWS

# EBS

- EBS volume: network device attach to the ec2 instances
- Restrict to one AZ
- Mount to one instance at a time
- Allows data to persist even after their termination
- Network device
- Connect ec2 via network
- Can be detached from one instance to another instance
- Provisioned capacity
    - GB or IOPS (I/O operation)
- EC2 to EBS is one to many
- Root EBS volume is deleted by termination by default
- Only EBS snapshot can be transfer to another AZ
    - Archive tier is much cheaper
    - There is recycle bin for snapshot
    - Recommended to detach EBS volume before the snapshot but it’s not required
    - FSR is fast snapshot restore service
- Volume type
    - gp2/gp3 (SSD) general purpose SSD price to performance balance (boot)
        - Cost effective
        - 1GB to 16TiB
        - Boot volume, virtual desktops, development and test env
        - GP3 can independently increase IOPs up to 16000 and throughput up to 1000 MiB
        - GP2 cannot independently increase IOPs, it’s linked to volume. Max IOPS is 160000 and 3IOPS per GB
    - io1/io2 (SSD) High performance, for mission critical low latency or high throughput workloads (boot)
        - 4GiB to 16TiB
        - For databases and read/write intensive apps
        - Max PIOPS is 64000 for Nitro EC2 and 32000 for others
        - Can scale IOPS independently
        - io2 has more durability and more IOPS per GiB with the same price as IO1
        - io2 block express
            - More space and more power
        - Support multi attach
    - st1(HDD) Low cost HDD, designed for frequently accessed, throughput intensive workloads (non boot)
        - Big Data, Data warehouses, Log Processing
        - High throughput
    - sc1 (HDD) Lowest cost HDD, less frequently accessed workloads (non boot)
        - Cold Storage apps

## EBS Multi-Attach

- Only available in IO1 or IO2 family
- Attach to multiple EC2
- For HA application
- Application must manage  concurrent read
- Attach up to 16 instances
- Must use a file system that’s cluster aware

## EBS Encryption

- Data at rest is encrypted inside volume
- All snapshots are encrypted behind the scene
- Can leverage KMS key to encrypt
- All data in flight
- Steps
    - Create EBS snapshot
    - Encrypt EBS snapshot using copy
    - Create a new EBS volume from the encrypted snapshot
    

# AMI

- Amazon Machine Image: basically OS image (Linux or Window or Mac)
- Can be create a new AMI from existing one with pre pack config and software's

# Instance Store

- Better I/O performance
- Ephemeral, deleted on stopped
- Good for buffer, cache, temporary content
- High risk of data lose
- Hardware device
- Backup and Replication are customer responsibilities
- Very high IOPS **(Highest)**
- Exam: high performance think instance store

# Amazon EFS

- Manged Network file system
- HA
- Multi AZ service
- Highly scalable
- Pay per use
- Use case:
    - Content sharing
    - Web serving
    - Data sharing
    - WordPress
- Uses NFSv4.1 protocol
- Use security group to control access to EFS
- Only compatible with Linux based AMI
- POSIX file system that has standard API
- Scales automatically, almost serverless
- EFS storage classes
    - EFS Scale
        - Auto scale
        - Grow to petabyte scale
    - Performance Mode
        - General purpose (default) - Latency sensitive use cases
        - Max I/O - High latency, parallel (big data, media processing)
    - Throughput Mode
        - Bursting
            - 1TB = 50MiB + burst of up to 100MiB/s
        - Provisioned
            - Set the throughput regardless of storage size
        - Elastic
            - Automatically scale
            - For unpredictable workloads
- EFS storage tier
    - Standard Tier: Frequent access
    - Infrequent access: cold tier
- EFS HA
    - Standard: Multi AZ
    - One Zone: One zone
        - For cold storage
        - Cheap
- General purpose is enough for most use cases

# EFS vs EBS

| EFS | EBS |
| --- | --- |
| HA | Usually attach to one instance expect for io1/2 storage classes |
| Attach to multiple instances across multiple AZs | Locked to one AZ |
| Only for Linux | Only snapshot can transfer to another AZ |
| Costly | Root EBS are deleted on terminated by default |
| Can leverage EFS-IA for cost saving |  |
| AWS EFS mount helper to encrypt data during transit |  |