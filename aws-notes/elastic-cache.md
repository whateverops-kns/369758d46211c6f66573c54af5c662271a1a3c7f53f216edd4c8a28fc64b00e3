# Elastic Cache

Owner: Khant
Tags: AWS

- Managed Redis or Memcached service
- In memory database
- High performance and Low Latency
- Fully managed

# Redis vs Memcached

| Redis | Memcached |
| --- | --- |
| Multi AZ | Multi Node |
| Read Replicas | No HA |
| Data durability using AOF persistence | Non presistent |
| Supports backup and restore | No backup or restore |
| Supports Sets and Sorted Sets | Mult threading architecture |
| Hybrid Cache and In Memory store database | Pure cache |

# Security

- Support IAM auth for Redis
- IAM is only for AWS API level security
- Redis Auth
    - Username/Password
    - Support SSL
- Memcached support SASL auth

# Use case

- Make application stateless
- Helps reduce load of database
- Best for combo with RDS
- Session store
- Lazy loading
- Gaming leaderboards
- Countdown
- Uniqueness via sorted sets