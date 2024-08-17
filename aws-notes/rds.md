# RDS

Owner: Khant
Tags: AWS

- Managed SQL database service by AWS
- Fully automated provisioning
    - OS patching
    - Monitoring dashboards
    - Storage backed by EBS (GP2 or io1)
    - Scaling capability
    - Multi AZ for DR
    - Point in time restore
- Storage auto scaling
    - Scale the database storage automatically
    - Had to set maximum storage threshold

# Read Replicas vs Multi AZ

- Read Replicas
    - Read replication instance for main database
    - Up to 15 read replicas
    - Within AZ or Cross AZ or even Cross region
    - Replicas are async with the main DB so reads are eventually consistent
    - Replicas can be promoted to their own DB
    - Application must update the connection string to leverage the read replicas
    - Read replicas support only “SELECT”
    - Use Cases:
        - Read heavy applications
        - Analytics system
        - Ability to read the data without stressing out the main DB
    - Network Cost:
        - Same region is free even for the different AZ
        - Cross region will have network fee
- Multi AZ
    - Sync replication
    - Automatic app failover to standby
    - One DNS name failover standby
    - HA
    - No manual approach from application side
    - No read/write just standby
    - Read replicas can be Multi AZ DR too
    - Zero downtime
    - Just one click setup “modify”

# RDS Custom

- Only support for oracle and MSSQL
- Can access OS via SSH
- Stay managed services
- De-active before custom access
- Not recommended

# Amazon Aurora

- Proprietary from AWS (not open sourced)
- Support PostgreSQL and MySQL
- Optimized by AWS 5x faster than MySQL and 3x faster than PostgreSQL
- Storage grow automatically up to 128TB
- support 15 replicas, faster than MySQL
- Instantaneous failover (30 seconds)
- 20% costly than MySQL and PostgreSQL
- Advanced monitoring
- Backtrack: restore data any point in time
- Industry compliance

## Aurora HA and Read Scaling

- 6 copies of data across 3AZ
    - 4 out of 6 for write
    - 3 out of 6 for read
    - Self healing
    - Storage striped across 100s of volumes
- One Aurora instance takes writes (master)
- Storage are replicated and shared across replicas
- Any replicas can become master in case of failover
- Support cross region
- DB Cluster interaction
    - Writer endpoint for write
    - Reader endpoint for read.
        - auto load balancing at connections level
        

## Replicas Auto Scaling

- Extended endpoint will appear in auto scaling
- Can have large custom endpoint for some replicas for some workload
    - Setup custom endpoint on large subset of replicas for heavy load
    - Analytical queries
    - Reader Endpoint is not use after custom endpoint
- Serverless
    - Per pay second
    - For unpredictable workloads
    - No planning
- Aurora Multi Master
    - Continuous write ability
    - Every can be read/write in case of write node failed
- Global Aurora
    - Cross Region
    - Super HA
    - 1 primary region
    - up to 5 secondary
    - 16 read replicas per region
    - Promote to another region RTO < 1 minute
    - Usual RTO less than 1 second
- Aurora ML
    - ML based predictions to app via SQL
    - Optimized for other ML services
    - Support with Sage Maker and Comprehend
    - Use case: Fraud detection, ads targeting, sentiment analysis, product recommendations

# RDS Backup and Monitoring

- Automated backup
    - Daily full backup during backup window
    - Transaction logs are backed-up by RDS every 5 min
    - Restore any point in time
- Manual Backup
    - Manually triggered by the user
    - Retention of backup as long as user want
- Tips and Tricks
    - Since RDS charge even in DB stopped time, instead of stopping create snapshot and restore the database instead.

## Aurora Backup

- Automated backup
    - 1 to 35 days (cannot be disabled)
    - Point in time recovery
- Manual
    - Same as RDS
    - Suitable for long term backup
- Restore a backup or snapshot create new database
- **Percona XtraBackup** is only thing that support for Aurora restoring
- Database cloning
    - Create a database from existing database
    - Faster than snapshot and restore
    - Uses copy-on-write protocol
    - Very fast and cost effective

# RDS & Aurora Security

- Encrypt at Rest
- If master is not encrypt, replicas can’t be encrypt
- In flight encryption
    - TLS-ready
- IAM Authentication supported
- Username and Password supported
- Security Group control traffic
- Expect RDS custom, instance can’t be SSH
- Can leverage Cloud Watch for logging

# RDS Proxy

- Fully managed database proxy
- Allow apps to pool and share DB connections
- To improve database efficiency and stress, minimize open connections
- Good for serverless apps
- Proxy is serverless and HA (Multi AZ)
- Reduced failover time by 66%
- Enforce IAM authentication for DB
- Store credentials securely via AWS Secrets Manager
- Only within VPC, no external access

**Tips:**

- IP Sec VPN connections for replicas on premise.
- Set the deletion policy on the RDS to snapshot and set s3 bucket to retain