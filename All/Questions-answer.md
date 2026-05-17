# DevOps Interview Preparation - AWS, Terraform, Incident Handling

---

# 1. Self Introduction

Hello, my name is Abhijit Ray.  
I am a DevOps Engineer with experience in AWS Cloud, Terraform, Linux, CI/CD, and Infrastructure Automation.

I have worked on:
- AWS infrastructure provisioning
- Terraform automation
- Linux server administration
- CI/CD pipelines using Jenkins/GitHub Actions
- Monitoring and troubleshooting production environments
- Auto Scaling and Load Balancer management
- Incident handling and root cause analysis

Recently, I have also been learning and working on:
- Kubernetes
- Docker
- DevOps architecture design
- Cloud cost optimization

I enjoy automating infrastructure, improving system reliability, and solving production issues quickly.

---

# 2. P1 Incident Handling Process (Application Down)

## Question
Client raised a P1 incident because the application is down intermittently. What process do you follow?

## Answer

For a P1 incident, my priority is to restore the service as quickly as possible while maintaining proper communication.

## Step-by-Step Process

### 1. Acknowledge the Incident
- Accept the ticket immediately
- Join bridge call / war room
- Understand:
  - Impact
  - Affected users
  - Error behavior
  - Timeline

### 2. Check Monitoring & Alerts
I verify:
- CloudWatch metrics
- CPU/Memory spikes
- Load balancer health
- Disk usage
- Application monitoring dashboards
- Recent deployments

### 3. Identify Scope
Check whether issue is:
- Application issue
- Infrastructure issue
- Database issue
- Network issue
- DNS issue
- Auto Scaling issue

### 4. Initial Troubleshooting
I check:
- EC2 instance health
- Application logs
- System logs
- Nginx/Apache logs
- Container logs
- Database connectivity
- Security groups/NACLs

Commands:
```bash
top
df -h
free -m
systemctl status <service>
journalctl -xe
````

### 5. Mitigation / Recovery

Depending on issue:

* Restart service
* Replace unhealthy instances
* Rollback deployment
* Scale infrastructure
* Restart pods/containers
* Failover to standby

### 6. Communication

Continuously update:

* Incident manager
* Client
* Stakeholders

Example:

* Root cause identified
* Mitigation in progress
* ETA shared

### 7. Root Cause Analysis (RCA)

After recovery:

* Collect logs
* Timeline analysis
* Identify root cause
* Prevent recurrence

### 8. Post Incident Actions

* Documentation
* Monitoring improvement
* Add alerts
* Automation fixes

---

# 3. AWS Cost Optimization Techniques

## Answer

I optimize AWS cost using multiple strategies:

## Compute Optimization

* Use Reserved Instances for steady workloads
* Use Spot Instances for non-critical workloads
* Right-size EC2 instances
* Use Auto Scaling

## Storage Optimization

* Delete unused EBS volumes
* Use lifecycle policies in S3
* Move old data to Glacier
* Remove unused snapshots

## Database Optimization

* Right-size RDS
* Stop non-production DBs after office hours
* Use Aurora Serverless where applicable

## Monitoring & Governance

* AWS Cost Explorer
* AWS Budgets
* Trusted Advisor recommendations
* Tagging strategy

## Networking Optimization

* Reduce unnecessary NAT Gateway usage
* Use VPC endpoints where possible

## Automation

* Schedule shutdown of dev/staging environments using Lambda/EventBridge

---

# 4. Creating EC2 Instance via Terraform

## Answer

## Step 1: Create Provider Configuration

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

## Step 2: Create EC2 Resource

```hcl
resource "aws_instance" "web" {
  ami           = "ami-xxxxxxxx"
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-EC2"
  }
}
```

## Step 3: Initialize Terraform

```bash
terraform init
```

## Step 4: Validate

```bash
terraform validate
```

## Step 5: Plan

```bash
terraform plan
```

## Step 6: Apply

```bash
terraform apply
```

Terraform will provision the EC2 instance automatically.

---

# 5. Managing AWS Resources Created Outside Terraform

## Answer

If resources are created manually outside Terraform, I import them into Terraform state.

## Process

### 1. Write Terraform Configuration

```hcl
resource "aws_instance" "example" {
}
```

### 2. Import Resource

```bash
terraform import aws_instance.example i-1234567890
```

### 3. Run Plan

```bash
terraform plan
```

### 4. Update Terraform Code

Add actual configuration to avoid drift.

This helps Terraform manage existing infrastructure properly.

---

# 6. Terraform for Multiple AWS Regions

## Answer

I use multiple provider blocks with aliases.

## Example

```hcl
provider "aws" {
  region = "ap-south-1"
}

provider "aws" {
  alias  = "us"
  region = "us-east-1"
}
```

## Resource in India Region

```hcl
resource "aws_instance" "india" {
  ami           = "ami-xxxx"
  instance_type = "t2.micro"
}
```

## Resource in US Region

```hcl
resource "aws_instance" "usa" {
  provider      = aws.us
  ami           = "ami-yyyy"
  instance_type = "t2.micro"
}
```

This allows deployment across multiple regions from a single codebase.

---

# 7. Core AWS Services Worked On

## Answer

The core AWS services I have worked with are:

## Compute

* EC2
* Auto Scaling Group
* Lambda

## Storage

* S3
* EBS
* EFS

## Networking

* VPC
* Route 53
* Load Balancers
* Security Groups
* NAT Gateway

## Database

* RDS
* DynamoDB

## Monitoring

* CloudWatch
* CloudTrail

## DevOps

* IAM
* CodePipeline
* Systems Manager

## Infrastructure as Code

* Terraform
* CloudFormation (basic)

---

# 8. Terraform Management or Architecture Planning?

## Answer

I have experience in both:

* Terraform implementation
* Infrastructure architecture planning

## My Responsibilities Included

* Writing reusable Terraform modules
* Designing VPC architecture
* Multi-environment setup
* State management using S3 + DynamoDB
* CI/CD integration
* IAM and security best practices
* Infrastructure scalability planning

I also participate in:

* Architecture discussions
* Cost optimization
* Disaster recovery planning
* High availability design

---

# 9. How to Unmanage a Resource from Terraform

## Answer

If I want Terraform to stop managing a resource but keep the resource in AWS, I remove it from Terraform state.

## Command

```bash
terraform state rm aws_instance.example
```

This:

* Removes resource from state file
* Does NOT delete actual resource

Terraform will no longer manage it.

---

# 10. Terraform Drift After Removing State

## Question

After removing resource from Terraform state, Terraform tries to recreate it during next plan. How do you fix this?

## Answer

This happens because:

* Resource exists in Terraform code
* But not in state file

Terraform thinks resource is missing and wants to create it.

## Fix Options

## Option 1: Remove Resource from Code

If resource should not be managed anymore:

* Remove resource block from Terraform code

OR

## Option 2: Use Import

If resource should still exist under Terraform:

```bash
terraform import
```

Example:

```bash
terraform import aws_instance.example i-123456
```

## Best Practice

Always keep:

* Terraform code
* Terraform state
* Actual infrastructure

in sync to avoid drift.

---

# 11. EC2 in Auto Scaling Keeps Terminating Due to Health Check Failure

## Question

Instances launched from Auto Scaling are failing health checks and terminating every 5 minutes. How do you investigate and retain logs?

## Answer

## Step 1: Identify Health Check Type

Check whether:

* EC2 health check
* ELB health check
* Application health check

is failing.

## Step 2: Suspend Auto Scaling Process Temporarily

I suspend termination process temporarily.

```bash
aws autoscaling suspend-processes \
--auto-scaling-group-name my-asg \
--scaling-processes Terminate
```

This prevents instance termination for investigation.

## Step 3: Access Failed Instance

Connect via:

* SSH
* Session Manager

## Step 4: Check Logs

### System Logs

```bash
journalctl -xe
dmesg
```

### Application Logs

```bash
/var/log/messages
/var/log/nginx/error.log
```

### Cloud-init Logs

```bash
/var/log/cloud-init.log
```

## Step 5: Verify Common Issues

### Health Check Path

Check:

* Wrong endpoint
* HTTP 500 errors
* Timeout issues

### Application Startup

Maybe:

* App startup taking too long
* Dependencies failing

### Security Group

Check:

* Load balancer can reach instance

### Disk/Memory

Check:

```bash
df -h
free -m
```

## Step 6: Enable Log Retention

### Option 1: CloudWatch Agent

Send logs to CloudWatch Logs.

### Option 2: S3 Log Backup

Use lifecycle hook + Lambda to upload logs before termination.

### Option 3: Auto Scaling Lifecycle Hook

Create lifecycle hook:

```bash
aws autoscaling put-lifecycle-hook
```

This pauses termination temporarily.

## Step 7: Root Cause Analysis

Common root causes:

* Application crash
* Wrong health check endpoint
* Bad deployment
* Dependency issue
* Port mismatch
* High startup time

## Step 8: Fix & Resume

After fix:

```bash
aws autoscaling resume-processes \
--auto-scaling-group-name my-asg
```

---

# Additional Tips for Interview

## Important Things to Show

* Structured troubleshooting approach
* Communication skills
* Ownership mindset
* Automation thinking
* AWS best practices
* Terraform state understanding

## Avoid Saying

* "I don't know"

Instead say:

* "I haven't worked directly on that, but this is how I would approach it."

---

```
```
