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

# Architecture

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

# AWS Services Used
Amazon VPC
Amazon EC2
Amazon RDS MySQL
Internet Gateway
Route Tables
Security Groups
S3 Gateway Endpoint

# VPC Configuration
VPC CIDR: 10.0.0.0/16
Availability Zones: 2
Public subnets: 2
Private subnets: 2
DNS resolution: Enabled
DNS hostnames: Enabled
S3 Gateway Endpoint: Enabled
NAT Gateway: Not used

# EC2 Web Server

EC2 configuration:
Instance name: cloud-support-web-server
Operating system: Amazon Linux 2023
Instance type: t3.micro
Public IPv4 address: Enabled
Web server: Apache (httpd)
Application layer: PHP
Apache was installed and configured using:
- sudo dnf install httpd -y
- sudo systemctl start httpd
- sudo systemctl enable httpd
Apache was verified to be running successfully.

# RDS MySQL Database

- RDS configuration:
  DB identifier: cloud-support-db
  Engine: MySQL
  Database: cloudapp
  Public access: No
  Port: 3306
• DB subnet group: Private subnets
  Security group: cloud-support-db-sg
- The RDS database was intentionally kept         private and was not directly exposed to the     internet

# Security Group Configuration
EC2 Security Group
cloud-support-web-sg
Inbound access:
- SSH 22 — allowed from My IP
- HTTP 80 — allowed from Anywhere IPv4

# RDS Security Group

cloud-support-db-sg
• Inbound access:
  MySQL/Aurora 3306
  Source: cloud-support-web-sg
- This allows the EC2 web server to communicate with the database while preventing direct internet access to MySQL.

# Database Connectivity Test

• The MariaDB/MySQL client was installed on the   EC2 instance and used to test the RDS           connection.
  Example:
mysql -h <RDS-ENDPOINT> -P 3306 -u admin -p
- The connection was successful and the       cloudapp database was accessed.

# A support_tickets table was created:

- CREATE TABLE support_tickets (
    id INT AUTO_INCREMENT PRIMARY KEY,
    issue VARCHAR(100),
    status VARCHAR(20)
);

- A test support ticket was inserted:
INSERT INTO support_tickets (issue, status)
VALUES ('Web server not responding', 'Open');

- The record was verified using:
SELECT * FROM support_tickets;

Application Integration

• PHP and the MySQL connector were installed on   the EC2 web server.

• A simple PHP test application was created to    connect to RDS MySQL and retrieve records       from the support_tickets table.

# The application displayed:

• Cloud Support Ticket
  Ticket ID: 1
  Issue: Web server not responding
  Status: Open
- This verified the complete application path:
  Browser

  # application flow

  Browser
   |
EC2
   |
Apache
   |
PHP
   |
RDS MySQL
   |
support_tickets table

# Connectivity Troubleshooting

- A controlled connectivity failure was created   to practice troubleshooting an EC2-to-RDS       connection problem.

Symptom
- The PHP application stopped responding          because the EC2 instance could no longer        connect to the RDS database.

Initial Network Test
TCP connectivity to the RDS MySQL port was tested from EC2 using 
• Netcat:nc -vz -w 5 <RDS-ENDPOINT> 3306

The result was:
Ncat: TIMEOUT
- This indicated that TCP port 3306 was not reachable from the EC2 instance.

# Investigation
- The RDS security group was checked and the inbound rule allowing the web-server security group to access MySQL was missing.

# Root Cause
The RDS security group did not have an inbound rule allowing:
- cloud-support-web-sg → TCP 3306

# Resolution
The correct inbound rule was restored:
- Type: MySQL/Aurora
  Protocol: TCP
  Port: 3306
  Source: cloud-support-web-sg

# Verification
The TCP connectivity test was run again: nc -vz -w 5 <RDS-ENDPOINT> 3306
- The connection was successful:
Ncat: Connected to <private-RDS-IP>:3306
- The PHP application was then tested again and successfully retrieved the database record.

# Troubleshooting workflow 

Application timeout
        |
        v
Test TCP connectivity
        |
        v
Port 3306 timed out
        |
        v
Check RDS security group
        |
        v
Inbound rule missing
        |
        v
Allow EC2 security group
on TCP 3306
        |
        v
Test connectivity again
        |
        v
Connection successful
        |
        v
Verify application
        |
        v
Database record displayed

# Key Skills Demonstrated
- AWS VPC
- Public and private subnets
- Route tables
- Internet Gateway
- EC2 administration
- Amazon RDS MySQL
- Security groups
- TCP/IP connectivity troubleshooting
- Apache web server
- PHP and MySQL connectivity
- Linux command-line administration
- Network troubleshooting
- Root-cause identification
- Incident troubleshooting
- Technical documentation

# What I Learned

This project provided practical experience with:
- Designing a basic multi-tier AWS architecture
- Separating web and database tiers
- Keeping a database private
- Controlling traffic using security groups
- Testing network connectivity between AWS resources.
- Diagnosing security-group-related connectivity failures
- Verifying application-to-database communication
- Following a structured troubleshooting process




