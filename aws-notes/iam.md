# IAM

Owner: Khant
Tags: AWS

# AWS IAM Deep Dive

- IAM Users are account users with specific permission.
- IAM Users can be grouped into IAM Groups. Users can belong to more than one group.
- **Least privilege principle is a must.**
- IAM are written in JSON.

# IAM MFA

- Password you know + security device you own
- SMS or App auth
- U2F key (Physical key)

IAM Policies in depth

- Apply at group, inheritance by user
- Inheritance by multiple group
- Id is identifier
- Sid is id of statement, optional
- Resource is optional
- Condition

I AM role

- mostly for services
- call service on our behalf
- EC2
- Lambda
- Cloud Formation

IAM Tools

- IAM Credentials Report (Account Level)
- IAM Access Advisor (User Level)

Best Practices

- Assign Users to Groups
- Strong password policy
- Use MFA
- Least privileges
- User toolings
- Never share