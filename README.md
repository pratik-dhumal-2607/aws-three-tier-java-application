# 🚀 AWS 3-Tier Web Application Deployment

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazonaws)
![EC2](https://img.shields.io/badge/Amazon-EC2-red)
![RDS](https://img.shields.io/badge/Amazon-RDS-blue)
![VPC](https://img.shields.io/badge/AWS-VPC-success)
![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-green?logo=nginx)
![Tomcat](https://img.shields.io/badge/Apache-Tomcat-yellow?logo=apachetomcat)
![Java](https://img.shields.io/badge/Java-Application-orange?logo=openjdk)
![Linux](https://img.shields.io/badge/Linux-Amazon%20Linux-black?logo=linux)
![GitHub](https://img.shields.io/badge/GitHub-Portfolio-black?logo=github)

---

## 📌 Project Overview

This project demonstrates the deployment of a **production-style 3-Tier Java Web Application** on **Amazon Web Services (AWS)** following cloud networking and security best practices.

The application is deployed using a custom AWS Virtual Private Cloud (VPC) with isolated networking. The infrastructure is divided into three independent layers:

- **Presentation Layer** – Nginx Reverse Proxy running on a Jump Server in public subnet
- **Application Layer** – Apache Tomcat hosting the Java web application in private subnet
- **Database Layer** – Amazon RDS (MariaDB) deployed in a private subnet

The architecture ensures that only the Jump Server is publicly accessible while the Application Server and Database remain protected inside private subnets.

---

## 🎯 Project Objectives

- Design a secure AWS network using a custom VPC.
- Implement public and private subnet architecture.
- Deploy a Java web application using Apache Tomcat.
- Configure Amazon RDS (MariaDB) for persistent data storage.
- Implement Nginx as a Reverse Proxy.
- Secure SSH access through a Jump Server.
- Apply Security Groups to restrict network access.
- Demonstrate real-world cloud deployment practices.

---

# 🏗️ Architecture Overview

```
                            Internet
                                │
                                ▼
                      Internet Gateway (IGW)
                                │
                                ▼
                     Public Route Table (Route IGW & associate public subnet)
                                │
                                ▼
        ┌─────────────────────────────────────┐
        │ Public Subnet                       │
        │                                     │
        │    Jump Server                      │
        │      Amazon Linux                   │
        │      Nginx Reverse Proxy            │
        └───────────────┬─────────────────────┘
                        │ SSH
                        │ HTTP
                        ▼
             Private Route Table (Route NAT Gateway & associate private subnets)
                        │
        ┌───────────────┴────────────────┐
        │                                │
        ▼                                ▼
┌──────────────────────┐        ┌────────────────────────┐
│ Private App Subnet   │        │ Private DB Subnet      │
│                      │        │                        │
│ Application Server   │        │ Database Server        │
│   Apache Tomcat      │◄──────►│   MariaDB              │
│   Java Student App   │        │   Database             │
└──────────────────────┘        └────────────────────────┘
```

---

# 🌐 Technology Stack

| Category | Technology |
|----------|------------|
| Cloud Provider | Amazon Web Services (AWS) |
| Compute | Amazon EC2 |
| Networking | VPC, Public & Private Subnets |
| Routing | Internet Gateway, NAT Gateway, Route Tables |
| Security | Security Groups |
| Web Server | Nginx |
| Application Server | Apache Tomcat 9 |
| Programming Language | Java |
| Database | Amazon RDS (MariaDB) |
| Operating System | Amazon Linux |
| Version Control | Git & GitHub |

---

# ☁️ AWS Services Used

| AWS Service | Purpose |
|-------------|---------|
| Amazon VPC | Isolated cloud network |
| EC2 | Compute instances |
| Public Subnet | Hosts Jump Server |
| Private Application Subnet | Hosts Tomcat Server |
| Private Database Subnet | Hosts Amazon RDS |
| Internet Gateway | Public Internet connectivity |
| NAT Gateway | Internet access for private resources |
| Elastic IP | Static IP for NAT Gateway |
| Route Tables | Network traffic routing |
| Security Groups | Instance-level firewall |
| Amazon RDS | Managed MariaDB database |

---

# ✨ Key Features

- Custom AWS VPC
- Multi-tier architecture
- Public and Private Subnets
- Bastion Host for secure administration
- Reverse Proxy using Nginx
- Apache Tomcat application deployment
- Amazon RDS (MariaDB)
- Secure Security Group configuration
- Private networking
- Internet Gateway and NAT Gateway
- JDBC database connectivity
- Production-inspired infrastructure design

---


# 🏛️ Infrastructure Components

## 🌐 Virtual Private Cloud (VPC)

A custom VPC was created to isolate all cloud resources and provide complete control over networking.

| Resource | Configuration |
|----------|---------------|
| VPC | Custom |
| Availability Zones | Multiple |
| Public Subnets | 1 |
| Private Application Subnets | 1 |
| Private Database Subnets | 1 |

---

## 🖥️ EC2 Instances

### Jump Server

The Jump Server is the only publicly accessible EC2 instance.

**Responsibilities**

- Secure SSH access to private instances
- Runs Nginx Reverse Proxy
- Receives incoming HTTP requests
- Forwards traffic to Apache Tomcat

---

### Application Server

The Application Server resides in a private subnet.

**Responsibilities**

- Runs Apache Tomcat
- Hosts the Java Student Application
- Connects securely to Amazon RDS
- Accessible only through the Jump Server

---

### Database Server

The Database server is in a private subnet.

**Responsibilities**

- Stores student records
- Accessible only from the Application Server
- No public internet access

---

# 🔐 Security Architecture

Security was implemented using AWS networking best practices.

### Jump Server

- Publicly Accessible
- SSH (Port 22)
- HTTP (Port 80)

### Application Server

- Private Subnet
- No Public IP
- SSH allowed only from Jump Server
- Tomcat Port (8080) accessible only from Jump Server

### Database Server

- Private Subnet
- No Public IP
- MariaDB Port (3306) accessible only from the Application Server

---

# 🛡️ Security Group Rules

## Jump Server

| Protocol | Port | Source |
|----------|------|--------|
| SSH | 22 | My IP |
| HTTP | 80 | Anywhere (0.0.0.0/0) |

---

## Application Server

| Protocol | Port | Source |
|----------|------|--------|
| SSH | 22 | Jump Server Security Group |
| TCP | 8080 | Jump Server Security Group |

---

## Database Server

| Protocol | Port | Source |
|----------|------|--------|
| MySQL | 3306 | Application Server Security Group |

---

# 🔄 Application Request Flow

When a user accesses the application, the request follows this path:

```text
User Browser
      │
      ▼
Public IP of Jump Server
      │
      ▼
Nginx Reverse Proxy
      │
      ▼
Apache Tomcat
      │
      ▼
Java Web Application
      │
      ▼
Database (MariaDB)
      │
      ▼
Response returned to the User
```

---

# 🚀 Deployment Workflow

1. Create a Custom VPC.
2. Create Public and Private Subnets.
3. Configure Route Tables.
4. Attach the Internet Gateway.
5. Create and configure the NAT Gateway.
6. Launch the Jump Server.
7. Launch the Application Server.
8. Create the Amazon RDS instance.
9. Configure Security Groups.
10. Install Java on the Application Server.
11. Install Apache Tomcat.
12. Deploy the Student WAR application.
13. Configure JDBC connectivity with Amazon RDS.
14. Install and configure Nginx on the Jump Server.
15. Verify the application using the Jump Server's public IP.

---

# 📊 Skills Demonstrated

## AWS

- Amazon EC2
- Amazon VPC
- Internet Gateway
- NAT Gateway
- Route Tables
- Public & Private Subnets
- Amazon RDS
- Security Groups

---

## Linux

- User Management
- File Permissions
- Package Management
- System Services
- SSH
- Process Management

---

## Web Technologies

- Apache Tomcat
- Nginx Reverse Proxy
- Java Web Applications
- JDBC Configuration

---

## Networking

- Private Networking
- Jump Server
- Reverse Proxy
- Secure Database Connectivity

---

# 🎯 Learning Outcomes

Through this project, I gained practical experience in:

- Designing secure AWS cloud architectures
- Deploying applications using a multi-tier architecture
- Configuring public and private networking
- Managing Linux servers
- Installing and configuring Apache Tomcat
- Deploying Java web applications
- Configuring Nginx as a Reverse Proxy
- Connecting applications to Amazon RDS
- Applying AWS security best practices

---

# 🔮 Future Improvements

This project can be enhanced by implementing:

- Docker containerization
- Terraform Infrastructure as Code
- Jenkins CI/CD pipeline
- HTTPS using AWS Certificate Manager
- Application Load Balancer (ALB)
- Auto Scaling Group



