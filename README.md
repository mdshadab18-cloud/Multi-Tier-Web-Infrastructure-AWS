# Multi-Tier Web Infrastructure on AWS

## Project Objective

Built a hands-on multi-tier web infrastructure on AWS to understand how a web server communicates with a private database through AWS networking and security controls.

The project demonstrates:

- Custom VPC and subnet design
- Public and private subnets
- EC2 web server deployment
- Amazon RDS MySQL deployment
- Security group configuration
- EC2-to-RDS connectivity
- Basic Apache and PHP configuration
- Network connectivity troubleshooting

## Architecture

```text
Internet
    |
Internet Gateway
    |
Public Subnet
    |
EC2 Web Server
Apache + PHP
    |
TCP 3306
    |
Private Subnet
    |
Amazon RDS MySQL
