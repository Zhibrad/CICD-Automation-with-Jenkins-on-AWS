# 🚀 CI/CD Automation with Jenkins on AWS

### Installing, Configuring and Validating a Jenkins CI/CD Server on Amazon EC2

![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red?logo=jenkins)
![Debian](https://img.shields.io/badge/Linux-Debian%20%2F%20Ubuntu-red?logo=debian)
![Java](https://img.shields.io/badge/Java-21%20%2F%2017-orange?logo=openjdk)
![Maven](https://img.shields.io/badge/Maven-3.9.9-C71A36?logo=apachemaven)
![Git](https://img.shields.io/badge/Git-Version%20Control-F05032?logo=git)
![DevOps](https://img.shields.io/badge/Domain-DevOps-purple)

> **A practical CI/CD infrastructure project demonstrating how to provision, configure and validate Jenkins on an AWS EC2 Linux server, prepare Java and Maven build tools, and execute a first automated build job.**

---

# 📌 Project Overview

Continuous Integration and Continuous Delivery/Deployment (CI/CD) are fundamental practices in modern DevOps.

Instead of manually connecting to a server, compiling applications, running tests and deploying software, Jenkins can automate these activities through repeatable jobs and pipelines.

This project demonstrates the foundational deployment of Jenkins on an AWS EC2 Linux server.

The implementation covers:

```text
AWS EC2
   ↓
Linux Server
   ↓
Jenkins
   ↓
Java
   ↓
Maven
   ↓
Git
   ↓
Build Job
   ↓
Console Output
```

The project starts with a manually configured Jenkins server and establishes the foundation for more advanced CI/CD automation such as:

- GitHub integration
- Maven builds
- Automated testing
- SonarQube quality analysis
- Docker image builds
- Amazon ECR
- Amazon ECS
- AWS deployment automation

---

# 🎯 Project Objectives

The objectives of this project are to:

- Provision an AWS EC2 Linux server.
- Configure secure SSH administration.
- Install Jenkins using the official Jenkins LTS package repository.
- Configure Jenkins to run as a system service.
- Access Jenkins through its web interface.
- Complete the Jenkins initial setup process.
- Configure a stable Jenkins URL.
- Configure Java development environments.
- Configure Maven 3.9.9.
- Verify Git availability.
- Create and execute a first Jenkins Freestyle project.
- Validate Jenkins execution through console output.
- Establish a foundation for future CI/CD pipelines.

---

# 📚 Table of Contents

1. [Project Overview](#-project-overview)
2. [Project Objectives](#-project-objectives)
3. [Business Scenario](#-business-scenario)
4. [Solution Architecture](#-solution-architecture)
5. [Technology Stack](#-technology-stack)
6. [Construction Rules](#-construction-rules)
7. [Implementation Flow](#-implementation-flow)
8. [Prerequisites](#-prerequisites)
9. [Step 1 — Create the EC2 Instance](#1--create-the-ec2-instance)
10. [Step 2 — Create the EC2 Key Pair](#2--create-the-ec2-key-pair)
11. [Step 3 — Configure the Security Group](#3--configure-the-security-group)
12. [Step 4 — Install Jenkins on Linux](#4--install-jenkins-on-linux)
13. [Step 5 — Launch the EC2 Instance](#5--launch-the-ec2-instance)
14. [Step 6 — SSH into the EC2 Server](#6--ssh-into-the-ec2-server)
15. [Step 7 — Verify Jenkins](#7--verify-jenkins)
16. [Step 8 — Access Jenkins](#8--access-jenkins)
17. [Step 9 — Retrieve the Initial Administrator Password](#9--retrieve-the-initial-administrator-password)
18. [Step 10 — Complete Jenkins Setup](#10--complete-jenkins-setup)
19. [Step 11 — Configure Jenkins URL](#11--configure-jenkins-url)
20. [Step 12 — Configure Maven](#12--configure-maven)
21. [Step 13 — Configure Java](#13--configure-java)
22. [Step 14 — Verify Java on the Server](#14--verify-java-on-the-server)
23. [Step 15 — Verify Git](#15--verify-git)
24. [Step 16 — Create the First Jenkins Job](#16--create-the-first-jenkins-job)
25. [Step 17 — Execute the First Build](#17--execute-the-first-build)
26. [CI/CD Execution Flow](#-cicd-execution-flow)
27. [Security Architecture](#-security-architecture)
28. [Verification Checklist](#-verification-checklist)
29. [Troubleshooting](#-troubleshooting)
30. [Cost Considerations](#-cost-considerations)
31. [Production Improvements](#-production-improvements)
32. [Skills Demonstrated](#-skills-demonstrated)
33. [Project Structure](#-project-structure)
34. [Lessons Learned](#-lessons-learned)
35. [Future Improvements](#-future-improvements)
36. [Official Documentation](#-official-documentation)
37. [Project Author](#-project-author)

---

# 💼 Business Scenario

Imagine a development team working on a Java application.

Without CI/CD, a developer might manually:

```text
Write Code
   ↓
Build Application
   ↓
Run Tests
   ↓
Package Application
   ↓
Copy Files to Server
   ↓
Deploy
```

This process can become slow, inconsistent and difficult to audit.

Jenkins introduces automation:

```text
Developer
    ↓
Git Repository
    ↓
Jenkins
    ↓
Build
    ↓
Test
    ↓
Package
    ↓
Deploy
```

The objective of this project is to establish the Jenkins platform that will eventually automate this workflow.

---

# 🏗️ Solution Architecture

```text
                         DEVELOPER
                             |
                             v
                         GitHub
                             |
                             v
                    +----------------+
                    |   Jenkins      |
                    |   Controller   |
                    +-------+--------+
                            |
                    +-------+--------+
                    |                |
                    v                v
                 Maven            Java/JDK
                    |                |
                    +-------+--------+
                            |
                            v
                       Build Job
                            |
                            v
                       Console Output
```

---

# 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Amazon EC2 | Jenkins hosting infrastructure |
| Debian / Ubuntu | Linux server |
| Jenkins LTS | CI/CD automation server |
| Java 21 | Jenkins controller runtime |
| JDK 17 | Optional build JDK for Java 17 applications |
| Maven 3.9.9 | Java build automation |
| Git | Source-code version control |
| SSH | Secure server administration |
| AWS Security Group | Network access control |

---

# 📐 Construction Rules

This implementation follows the following infrastructure rules.

## Rule 1 — Use Jenkins LTS

The deployment uses the Jenkins Long Term Support release rather than the weekly release for greater stability.

Jenkins publishes the LTS package through the Debian-stable repository.

---

## Rule 2 — Use a Supported Java Runtime

The Jenkins controller must run on a supported Java version.

For current Jenkins LTS releases:

```text
Jenkins Controller
        ↓
Java 21+
```

If a project requires Java 17, Java 17 can be installed separately as a build tool:

```text
Jenkins Controller
        ↓
Java 21

Application Build
        ↓
JDK 17
```

This separation is important.

---

## Rule 3 — Restrict Administrative Access

SSH should be restricted to the administrator's IP address:

```text
TCP 22
Source: YOUR_PUBLIC_IP/32
```

Jenkins port 8080 should also be restricted during this lab:

```text
TCP 8080
Source: YOUR_PUBLIC_IP/32
```

For production, Jenkins should preferably be placed behind HTTPS and an appropriate reverse proxy/load-balancing architecture.

---

## Rule 4 — Use a Stable Jenkins URL

An EC2 public IP can change when an instance is stopped and started.

Therefore, production-oriented deployments should use:

```text
Elastic IP
     OR
DNS hostname
```

Example:

```text
jenkins.example.com
```

Changing the Jenkins URL in Jenkins configuration does not itself prevent an EC2 public IP from changing.

---

## Rule 5 — Verify Every Dependency

Before creating the first build:

```text
EC2
 ↓
SSH
 ↓
Java
 ↓
Jenkins
 ↓
Maven
 ↓
Git
 ↓
Build Job
```

Each layer must be verified independently.

---

# 🔄 Implementation Flow

```text
Create EC2
    ↓
Create Key Pair
    ↓
Create Security Group
    ↓
Configure SSH + Jenkins 8080
    ↓
Install Java
    ↓
Install Jenkins LTS
    ↓
Launch EC2
    ↓
SSH to Server
    ↓
Verify Jenkins Service
    ↓
Open Jenkins :8080
    ↓
Retrieve Initial Password
    ↓
Install Plugins
    ↓
Create Admin Account
    ↓
Configure Jenkins URL
    ↓
Configure Maven
    ↓
Configure JDK
    ↓
Verify Git
    ↓
Create Freestyle Job
    ↓
Execute Shell
    ↓
Build Now
    ↓
Inspect Console Output
```

---

# ✅ Prerequisites

Before starting, ensure you have:

- AWS account
- EC2 access
- AWS key pair
- Local terminal
- SSH client
- Internet access
- Your current public IP address
- Basic Linux command-line knowledge

---

# 1 — Create the EC2 Instance

Open:

```text
AWS Console
   ↓
EC2
   ↓
Instances
   ↓
Launch Instance
```

Configure:

```text
Name:
Jenkins-CICD

AMI:
Debian Linux
```

You may also use Ubuntu if preferred; Jenkins provides Debian/Ubuntu installation instructions.

Select an instance type appropriate for Jenkins.

For a small learning environment, start with a small general-purpose instance and monitor memory, CPU and disk usage.

Configure storage:

```text
Root EBS:
20 GB or more
```

---

# 2 — Create the EC2 Key Pair

During instance creation:

```text
Key pair
   ↓
Create new key pair
```

Example:

```text
Name:
jenkins-key
```

Download the private key.

For Windows PowerShell, keep the `.pem` file somewhere secure.

Example:

```text
C:\Users\YourName\.ssh\jenkins-key.pem
```

Never commit this file to GitHub.

---

# 3 — Configure the Security Group

Create a new Security Group.

## SSH

```text
Type:
SSH

Protocol:
TCP

Port:
22

Source:
My IP
```

## Jenkins

```text
Type:
Custom TCP

Protocol:
TCP

Port:
8080

Source:
My IP
```

The initial learning environment therefore exposes:

```text
TCP 22
TCP 8080
```

and both are restricted to your current public IP.

---

# 4 — Install Jenkins on Linux

Before installation, consult the official Jenkins Linux installation instructions:

https://www.jenkins.io/doc/book/installing/linux/

## Install Java for the Jenkins Controller

For a current Jenkins LTS installation:

```bash
sudo apt update
sudo apt install -y fontconfig openjdk-21-jre
```

Verify:

```bash
java -version
```

If you specifically require a complete JDK for build tools:

```bash
sudo apt install -y openjdk-21-jdk
```

### Why Java 21?

Current Jenkins LTS releases require a supported Java runtime, and current LTS documentation specifies Java 21 or later for the controller.

---

## Add Jenkins LTS Repository

Create the keyring:

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```

Add the repository:

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Update:

```bash
sudo apt update
```

Install Jenkins:

```bash
sudo apt install -y jenkins
```

These commands follow the current Jenkins Debian/Ubuntu LTS installation method.

---

# 5 — Launch the EC2 Instance

After configuring:

- Instance type
- Key pair
- Storage
- Security Group

launch the instance.

Wait until:

```text
Instance State:
Running
```

and:

```text
Status Checks:
2/2 checks passed
```

Copy the EC2 public IPv4 address.

Example:

```text
98.84.130.158
```

---

# 6 — SSH into the EC2 Server

From your local terminal:

```bash
ssh -i "jenkins-key.pem" admin@<EC2-PUBLIC-IP>
```

The username depends on the selected Debian/Ubuntu image.

For example:

```bash
ssh -i "jenkins-key.pem" admin@98.84.130.158
```

For Ubuntu:

```bash
ssh -i "jenkins-key.pem" ubuntu@98.84.130.158
```

---

# 7 — Verify Jenkins

Check the Jenkins service:

```bash
sudo systemctl status jenkins
```

Expected:

```text
Active: active (running)
```

If Jenkins is not running:

```bash
sudo systemctl start jenkins
```

Enable Jenkins at boot:

```bash
sudo systemctl enable jenkins
```

---

# 8 — Access Jenkins

Open a browser:

```text
http://<EC2-PUBLIC-IP>:8080
```

Example:

```text
http://98.84.130.158:8080
```

You should see the Jenkins unlock screen.

---

# 9 — Retrieve the Initial Administrator Password

On the EC2 server:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the generated password.

Paste it into the Jenkins setup page.

> **Security:** Never publish this password in screenshots, documentation or GitHub.

---

# 10 — Complete Jenkins Setup

After entering the initial administrator password:

```text
Customize Jenkins
```

Choose:

```text
Install suggested plugins
```

If you want a minimal lab and do not require Ant, you can deselect the Ant-related plugin when the installation interface allows customization.

Jenkins will install the selected plugins.

---

# Create the Administrator Account

Provide:

```text
Username
Password
Full Name
Email Address
```

Use a strong password.

Do not reuse AWS credentials.

---

# 11 — Configure Jenkins URL

Navigate to:

```text
Manage Jenkins
   ↓
System
   ↓
Jenkins URL
```

For a temporary lab:

```text
http://<EC2-PUBLIC-IP>:8080/
```

For a more stable setup, use:

```text
http://<Elastic-IP>:8080/
```

or preferably:

```text
https://jenkins.example.com/
```

through a production-appropriate HTTPS/reverse-proxy architecture.

### Why?

An EC2 dynamic public IPv4 address can change after a stop/start cycle.

A stable Elastic IP or DNS hostname prevents users, integrations and webhooks from depending on a changing address.

---

# 12 — Configure Maven

Navigate to:

```text
Manage Jenkins
   ↓
Tools
```

Find:

```text
Maven installations
```

Add Maven.

Use:

```text
Name:
MAVEN3.9
```

Select the required Maven version:

```text
3.9.9
```

Save the configuration.

Jenkins can then expose the configured Maven installation to jobs and pipelines.

---

# 13 — Configure Java

Go to:

```text
Manage Jenkins
   ↓
Tools
   ↓
JDK installations
```

For an application that specifically requires Java 17, configure:

```text
Name:
JDK17
```

You can either use Jenkins automatic installation or provide the existing Java installation on the EC2 server.

---

# 14 — Verify Java on the Server

SSH into the EC2 server.

Run:

```bash
java -version
```

Then:

```bash
ls /usr/lib/jvm/
```

You may see directories similar to:

```text
java-17-openjdk-amd64
java-21-openjdk-amd64
```

The exact directory name depends on the Debian/Ubuntu release and package installation.

---

# Install JDK 17 if Required

If JDK 17 is not already installed:

```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
```

Then verify:

```bash
ls /usr/lib/jvm/
```

and:

```bash
java -version
```

You should now see the installed Java versions.

---

# Configure JAVA_HOME

In:

```text
Manage Jenkins
   ↓
Tools
   ↓
JDK installations
```

For:

```text
JDK17
```

provide the path reported by:

```bash
ls /usr/lib/jvm/
```

For example:

```text
/usr/lib/jvm/java-17-openjdk-amd64
```

> **Important:** Do not assume `/usr/lib/jvm/openjdk-17-jdk-amd64` exists. Always use the exact directory returned by your server.

This allows Jenkins jobs to use Java 17 while the Jenkins controller itself can continue using a supported Java 21 runtime.

---

# 15 — Verify Git

Git is required for pulling source code from repositories.

Check:

```bash
git --version
```

Expected:

```text
git version 2.x.x
```

If Git is missing:

```bash
sudo apt update
sudo apt install -y git
```

Verify again:

```bash
git --version
```

---

# 16 — Create the First Jenkins Job

Go to:

```text
Jenkins Dashboard
   ↓
New Item
```

Enter:

```text
Name:
Jenkins-First-Build
```

Select:

```text
Freestyle project
```

Click:

```text
OK
```

---

# Add a Description

Example:

```text
This is my first Jenkins CI/CD validation job running on an AWS EC2 Linux server.
The job validates the Jenkins execution environment and prints system information.
```

---

# Add Build Step

Scroll to:

```text
Build Steps
```

Select:

```text
Execute shell
```

Add:

```bash
whoami
pwd
w
id
```

---

# Why These Commands?

### `whoami`

Displays the user executing the Jenkins build.

Example:

```text
jenkins
```

### `pwd`

Displays the current Jenkins workspace.

Example:

```text
/var/lib/jenkins/workspace/Jenkins-First-Build
```

### `w`

Displays logged-in users and system activity.

### `id`

Displays the UID, GID and group membership of the build user.

---

# 17 — Execute the First Build

Click:

```text
Save
```

Then:

```text
Build Now
```

Jenkins will create a build number:

```text
#1
```

Click:

```text
#1
   ↓
Console Output
```

You should see output similar to:

```text
Started by user admin

Building in workspace
/var/lib/jenkins/workspace/Jenkins-First-Build

+ whoami
jenkins

+ pwd
/var/lib/jenkins/workspace/Jenkins-First-Build

+ w
...

+ id
uid=...
gid=...
groups=...

Finished: SUCCESS
```

The exact output will vary according to your server.

---

# 🔄 CI/CD Execution Flow

The basic Jenkins workflow now looks like:

```text
Developer
    |
    v
Git Repository
    |
    v
Jenkins
    |
    v
Build Job
    |
    +---- Java
    |
    +---- Maven
    |
    +---- Git
    |
    v
Console Output
    |
    v
Build Result
```

---

# 🚀 Future CI/CD Architecture

This foundational Jenkins installation can evolve into:

```text
                   DEVELOPER
                       |
                       v
                    GitHub
                       |
                       v
                   Jenkins
                       |
              +--------+---------+
              |                  |
              v                  v
           Maven              Git
              |
              v
          Unit Tests
              |
              v
         SonarQube
              |
              v
         Quality Gate
              |
              v
          Docker Build
              |
              v
             ECR
              |
              v
             ECS
              |
              v
           Production
```

This architecture transforms the initial Jenkins installation into a complete AWS CI/CD platform.

---

# 🔐 Security Architecture

The initial implementation uses:

```text
                 INTERNET
                    |
             Restricted IP
                    |
            +-------+-------+
            |               |
        TCP 22           TCP 8080
            |               |
            v               v
          SSH            Jenkins
            |               |
            +-------+-------+
                    |
                  EC2
```

### Security controls

- SSH restricted to administrator IP
- Jenkins 8080 restricted during setup
- EC2 key pair authentication
- Jenkins administrator credentials
- No private keys stored in GitHub
- Linux service isolation
- Least-privilege build execution
- Stable Jenkins URL planned for production

---

# 🧪 Verification Checklist

```text
[✓] AWS EC2 instance created

[✓] EC2 key pair created

[✓] SSH security rule configured

[✓] Jenkins 8080 security rule configured

[✓] Java installed

[✓] Jenkins LTS repository configured

[✓] Jenkins installed

[✓] Jenkins service running

[✓] Jenkins web interface accessible

[✓] Initial administrator password retrieved

[✓] Jenkins administrator created

[✓] Plugins installed

[✓] Jenkins URL configured

[✓] Maven 3.9.9 configured

[✓] JDK 17 configured for build compatibility

[✓] Java installation verified

[✓] Git verified

[✓] Freestyle job created

[✓] Shell commands executed

[✓] Console output verified

[✓] Build completed successfully
```

---

# 🚨 Troubleshooting

## Jenkins Service Is Not Running

Check:

```bash
sudo systemctl status jenkins
```

View recent logs:

```bash
sudo journalctl -u jenkins -n 100 --no-pager
```

---

## Jenkins Cannot Start Because Java Is Missing

Check:

```bash
java -version
```

Install the supported runtime:

```bash
sudo apt update
sudo apt install -y fontconfig openjdk-21-jre
```

Then:

```bash
sudo systemctl restart jenkins
```

---

## Cannot Open Jenkins on Port 8080

Check Jenkins:

```bash
sudo systemctl status jenkins
```

Check listening ports:

```bash
sudo ss -lntp | grep 8080
```

Check the AWS Security Group.

Confirm:

```text
TCP 8080
Source: Your Public IP
```

---

## Initial Password Not Found

Use the correct path:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## Maven Cannot Be Found

Verify Jenkins:

```text
Manage Jenkins
   ↓
Tools
   ↓
Maven installations
```

Confirm the Maven name used by your jobs matches the configured installation.

---

## JDK 17 Path Does Not Work

Do not guess the path.

Run:

```bash
ls /usr/lib/jvm/
```

Then use the exact JDK 17 directory returned.

Example:

```text
/usr/lib/jvm/java-17-openjdk-amd64
```

---

## Git Is Missing

Install:

```bash
sudo apt update
sudo apt install -y git
```

Verify:

```bash
git --version
```

---

# 💰 Cost Considerations

This project uses AWS EC2 as the Jenkins hosting platform.

Potential AWS costs include:

```text
EC2
+
EBS
+
Public IPv4
+
Data Transfer
```

The actual cost depends on:

- AWS Region
- Instance type
- Running time
- Storage
- Network traffic
- Number of builds
- Build workload

For a learning environment, Jenkins can be hosted on a small instance, but resource usage should be monitored.

Jenkins builds involving Maven, Docker, SonarQube or parallel jobs may require significantly more memory and CPU than a simple test job.

---

# 📈 Production Improvements

The current implementation is intentionally designed as a foundational CI/CD server.

A production implementation should consider:

## 1. Stable DNS

```text
jenkins.example.com
```

instead of relying directly on a dynamic IP.

---

## 2. HTTPS

Instead of:

```text
http://jenkins.example.com:8080
```

use:

```text
https://jenkins.example.com
```

through an appropriate reverse-proxy/load-balancing design.

---

## 3. Restricted Access

Limit administrative access through:

- Security Groups
- VPN/ZTNA
- Reverse proxy
- Identity provider
- SSO
- MFA

---

## 4. Jenkins Controller / Agent Separation

Instead of compiling everything on the controller:

```text
Jenkins Controller
       |
       v
Build Agent
       |
       v
Maven / Docker / Testing
```

---

## 5. Infrastructure as Code

Terraform can provision:

```text
VPC
EC2
Security Group
IAM
EBS
Elastic IP
Route 53
CloudWatch
```

---

## 6. CI/CD Pipeline

The next stage is:

```text
GitHub
   ↓
Jenkins
   ↓
Build
   ↓
Test
   ↓
SonarQube
   ↓
Quality Gate
   ↓
Docker
   ↓
Amazon ECR
   ↓
Amazon ECS
```

---

# 🧠 Skills Demonstrated

## AWS

- Amazon EC2
- Security Groups
- Key Pairs
- EBS
- Public IP addressing
- AWS cost awareness

## Linux

- Debian / Ubuntu
- SSH
- APT
- systemd
- service management
- file system navigation
- process and user inspection

## Jenkins

- Jenkins LTS installation
- Initial configuration
- Plugins
- Tools configuration
- Freestyle projects
- Build execution
- Console output
- Jenkins workspace

## Java

- Java 21 Jenkins runtime
- Java 17 application build environment
- JAVA_HOME
- Multiple JDK management

## Maven

- Maven 3.9.9
- Jenkins Maven tool configuration
- Build environment preparation

## Git

- Git installation
- Git version control preparation
- Source-control integration foundation

## DevOps

- CI/CD
- Infrastructure provisioning
- Build automation
- Environment configuration
- Security
- Troubleshooting
- Cloud cost awareness

---

# 📂 Project Structure

```text
jenkins-cicd-automation/
│
├── README.md
│
├── docs/
│   ├── architecture.md
│   ├── installation.md
│   ├── security.md
│   └── troubleshooting.md
│
├── screenshots/
│   ├── ec2-instance.png
│   ├── security-group.png
│   ├── jenkins-dashboard.png
│   ├── jenkins-tools.png
│   ├── jenkins-job.png
│   └── console-output.png
│
└── architecture/
    └── jenkins-cicd-architecture.png
```

---

# 🔐 GitHub Security Rules

Never commit:

```text
*.pem
*.key
credentials
passwords
Jenkins secrets
AWS access keys
private SSH keys
```

Recommended `.gitignore`:

```gitignore
# AWS / SSH private keys
*.pem
*.key
id_rsa
id_ed25519

# Credentials
.env
.env.*

# Jenkins secrets
secrets/
initialAdminPassword

# Logs
*.log

# IDE
.vscode/
.idea/
```

---

# 📚 Lessons Learned

### 1. Jenkins Is an Application Running on Infrastructure

Installing Jenkins is only the beginning.

The complete platform requires:

```text
Cloud Infrastructure
+
Linux
+
Java
+
Jenkins
+
Build Tools
+
Source Control
```

---

### 2. The Controller Runtime and Build JDK Can Be Different

A current Jenkins LTS controller should run on a supported Java runtime such as Java 21.

A Java 17 JDK can still be configured separately for projects that require Java 17.

This creates:

```text
Jenkins Controller → Java 21

Application Build → JDK 17
```

This separation is important when maintaining multiple application versions.

---

### 3. Network Configuration Is Part of CI/CD

Jenkins is only useful when developers and integrations can reach it securely.

Therefore:

```text
Security Group
+
SSH
+
Port 8080
+
Stable DNS
+
HTTPS
```

are part of the CI/CD infrastructure.

---

### 4. Tool Configuration Must Be Reproducible

Maven, JDK and Git should be explicitly configured and verified before automated builds are introduced.

---

# 🔮 Future Improvements

## Phase 1 — Source Control Integration

```text
GitHub
   ↓
Jenkins
```

## Phase 2 — Maven CI

```text
GitHub
   ↓
Jenkins
   ↓
Maven Build
```

## Phase 3 — Automated Testing

```text
Maven
   ↓
Unit Tests
```

## Phase 4 — Code Quality

```text
Maven
   ↓
SonarQube
   ↓
Quality Gate
```

## Phase 5 — Containerization

```text
Application
   ↓
Docker Image
```

## Phase 6 — AWS Container Deployment

```text
Docker
   ↓
Amazon ECR
   ↓
Amazon ECS
```

## Final CI/CD Platform

```text
                DEVELOPER
                    |
                    v
                 GitHub
                    |
                    v
                 Jenkins
                    |
          +---------+----------+
          |                    |
          v                    v
        Maven                Tests
          |                    |
          +---------+----------+
                    |
                    v
                SonarQube
                    |
                    v
               Quality Gate
                    |
                    v
              Docker Build
                    |
                    v
                  ECR
                    |
                    v
                  ECS
                    |
                    v
               Production
```

---

# 🏆 Portfolio Value

This project demonstrates the ability to build a CI/CD platform from the infrastructure layer upward.

Rather than simply installing Jenkins, the project covers:

```text
Cloud Provisioning
        ↓
Linux Administration
        ↓
Jenkins Installation
        ↓
Java Environment
        ↓
Maven Configuration
        ↓
Git Integration
        ↓
Build Automation
        ↓
Security
        ↓
CI/CD Architecture
```

This establishes the foundation for a production-oriented DevOps pipeline.

---

# 📌 Project Summary

### Project

**CI/CD Automation with Jenkins on AWS**

### Infrastructure

```text
AWS EC2
+
Debian / Ubuntu
+
Jenkins LTS
+
Java 21
+
JDK 17
+
Maven 3.9.9
+
Git
```

### Core DevOps Concepts

```text
Continuous Integration
Continuous Delivery
Build Automation
Cloud Infrastructure
Linux Administration
Source Control
Java Build Management
Security
Infrastructure Management
```

---

# 📖 Official Documentation

### Jenkins

- https://www.jenkins.io/doc/book/installing/linux/
- https://www.jenkins.io/doc/
- https://www.jenkins.io/doc/book/pipeline/
- https://www.jenkins.io/doc/book/managing/tools/

### Jenkins LTS Packages

- https://pkg.jenkins.io/debian-stable/

### Maven

- https://maven.apache.org/

### Git

- https://git-scm.com/docs

### AWS EC2

- https://docs.aws.amazon.com/ec2/

### AWS Security Groups

- https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html

---

# 👨‍💻 Project Author

## Zhibrad

**Cloud & DevOps Portfolio**

Areas of focus:

```text
AWS
Docker
Linux
Jenkins
CI/CD
Cloud Security
Infrastructure
DevOps
Automation
```

---

# ⭐ Final Architecture

```text
                         DEVELOPER
                             |
                             v
                          GitHub
                             |
                             v
                    +----------------+
                    |    Jenkins     |
                    |   Controller   |
                    +-------+--------+
                            |
                  +---------+---------+
                  |                   |
                  v                   v
                Maven              Java/JDK
                  |                   |
                  +---------+---------+
                            |
                            v
                       Build Job
                            |
                            v
                     Test / Package
                            |
                            v
                       Build Result
```

> **Built to demonstrate real-world DevOps engineering: cloud infrastructure, Linux administration, Jenkins automation, build-tool configuration, security-conscious networking, and a foundation for end-to-end CI/CD.**
