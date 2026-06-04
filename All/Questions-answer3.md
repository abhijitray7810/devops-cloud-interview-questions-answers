# DevOps Interview Questions & Answers

## 1. What are the different ways to create a Jenkins pipeline?
- **Pipeline Script (Declarative Pipeline)** using `Jenkinsfile`.
- **Scripted Pipeline** using Groovy syntax.
- **Pipeline from SCM** (GitHub, GitLab, Bitbucket).
- **Multibranch Pipeline**.
- **Pipeline generated through Blue Ocean UI**.

---

## 2. What are the different sections in a Declarative Pipeline?
```groovy
pipeline {
    agent any
    parameters {}
    environment {}
    stages {}
    post {}
}
```
Main sections:
- `pipeline`
- `agent`
- `tools`
- `environment`
- `parameters`
- `stages`
- `steps`
- `post`

---

## 3. What are the different agents you used?
- `any`
- `none`
- `label`
- `node`
- `docker`
- `dockerfile`
- Kubernetes pod agents

---

## 4. What are the different parameter types you used?
- String Parameter
- Choice Parameter
- Boolean Parameter
- Text Parameter
- Password Parameter
- Active Choice Parameter
- Active Choice Reactive Parameter

---

## 5. How do you populate values dynamically for parameters?
Using the **Active Choices Plugin** with Groovy scripts that fetch values from:
- AWS
- Git branches
- APIs
- Databases

---

## 6. Which parameter is used to populate values dynamically?
- Active Choice Parameter
- Active Choice Reactive Parameter

---

## 7. Difference between Active Choice Reactive Parameter and Active Choice Reactive Reference Parameter?

### Active Choice Reactive Parameter
- Updates dynamically based on another parameter.
- User can select values.

### Active Choice Reactive Reference Parameter
- Read-only display.
- Used for showing additional information.

---

## 8. What is branching strategy?
A Git workflow defining how code is developed and released.

Common strategies:
- Git Flow
- GitHub Flow
- GitLab Flow
- Trunk-Based Development

---

## 9. In DevOps Lifecycle, where do CI and CD fit?
- **CI**: Build, Test, Integration.
- **CD**: Deployment and Release Automation.

Flow:
```
Plan → Code → Build → Test → Release → Deploy → Operate → Monitor
```

---

## 10. What AWS services have you used?
- EC2
- VPC
- IAM
- S3
- Route53
- ALB/NLB
- Auto Scaling
- CloudWatch
- EKS
- ECS
- Lambda
- RDS
- Secrets Manager
- ECR
- CodePipeline

---

## 11. In Route53, what is a Record and CNAME Record?

### Record
Maps a domain name to a resource.

### CNAME Record
Maps one domain name to another domain name.

Example:
```
app.example.com → alb.amazonaws.com
```

---

## 12. Difference between A Record and CNAME Record?

| A Record | CNAME |
|-----------|--------|
| Maps domain to IP | Maps domain to another domain |
| Faster lookup | Additional DNS lookup |
| Root domain supported | Root domain not supported |

---

## 13. If one Availability Zone goes down, how do you route traffic automatically?

- Deploy application across multiple AZs.
- Use Application Load Balancer.
- Configure Auto Scaling Groups.
- Enable Route53 Health Checks and Failover Routing.
- For DR, use Multi-Region deployment.

---

## 14. What are the limitations of Lambda?

- Maximum execution time: 15 minutes.
- Cold start latency.
- Temporary storage limit.
- Stateless execution.
- Memory and concurrency limits.

---

## 15. Write a Lambda function example.

```python
import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda')
    }
```

---

## 16. Difference between Public and Private Subnet?

| Public Subnet | Private Subnet |
|---------------|---------------|
| Internet access | No direct internet access |
| Uses Internet Gateway | Uses NAT Gateway |
| Web servers | Databases, internal services |

---

## 17. Where will you attach your NAT Gateway?
NAT Gateway is created inside a **Public Subnet** and attached to a route table that has access to an Internet Gateway.

---

## 18. How is traffic from Private Subnet to NAT Gateway configured?
Private subnet route table:

```
0.0.0.0/0 → NAT Gateway
```

NAT Gateway forwards traffic through Internet Gateway.

---

## 19. What is Blue-Green Deployment?

Two identical environments:

- Blue = Current production
- Green = New version

Traffic is switched from Blue to Green after validation.

Benefits:
- Near-zero downtime
- Easy rollback

---

## 20. How does Auto Scaling work in EKS?

### Pod Scaling
Horizontal Pod Autoscaler (HPA)
- CPU
- Memory
- Custom metrics

### Node Scaling
Cluster Autoscaler or Karpenter
- Adds nodes when pods are pending.
- Removes unused nodes.

---

## 21. What is PDB?

**Pod Disruption Budget**

Defines the minimum number of pods that must remain available during:
- Maintenance
- Node drain
- Upgrades

Example:
```yaml
minAvailable: 2
```

---

## 22. Steps to upgrade an EKS cluster?

1. Check compatibility.
2. Upgrade control plane.
3. Upgrade managed node groups.
4. Upgrade add-ons:
   - CoreDNS
   - kube-proxy
   - VPC CNI
5. Validate workloads.
6. Monitor cluster.

---

## 23. What is Ingress?

Ingress manages external HTTP/HTTPS access to Kubernetes services.

Features:
- Host-based routing
- Path-based routing
- SSL termination
- Load balancing

---

## 24. What are the components of ArgoCD?

- API Server
- Repository Server
- Application Controller
- Dex (Authentication)
- Redis
- UI

---

## 25. What are the methods to create an application in ArgoCD?

- Web UI
- CLI
- YAML Manifest
- GitOps Repository
- ApplicationSet

---

## 26. What is a Terraform State File?

`terraform.tfstate`

Stores:
- Resource IDs
- Metadata
- Infrastructure mapping

Used to determine current infrastructure state.

---

## 27. What is the use of Lifecycle Block in Terraform?

```hcl
lifecycle {
  create_before_destroy = true
  prevent_destroy = true
  ignore_changes = [tags]
}
```

Controls resource creation and destruction behavior.

---

## 28. Name some Terraform functions.

- element()
- lookup()
- length()
- join()
- split()
- format()
- upper()
- lower()
- merge()
- concat()
- contains()
- file()

---

## 29. What is the use of element()?

Returns an item from a list by index.

Example:
```hcl
element(["a","b","c"],1)
```

Output:
```
b
```

---

## 30. Difference between count and for_each?

| count | for_each |
|---------|----------|
| Uses numeric index | Uses keys |
| Best for identical resources | Best for unique resources |
| Less flexible | More flexible |

---

## 31. What is a Multi-stage Docker Image?

Uses multiple build stages.

Benefits:
- Smaller image size
- Better security
- Faster deployments

Example:
```dockerfile
FROM maven AS build
RUN mvn package

FROM openjdk:17
COPY --from=build app.jar .
```

---

## 32. Difference between ENTRYPOINT and CMD?

| ENTRYPOINT | CMD |
|------------|-----|
| Main executable | Default arguments |
| Hard to override | Easy to override |
| Mandatory process | Optional parameters |

Example:
```dockerfile
ENTRYPOINT ["java"]
CMD ["-jar","app.jar"]
```

---

## 33. How have you created dashboards in Grafana?

1. Configure datasource (Prometheus, CloudWatch, Loki).
2. Create dashboard.
3. Add panels.
4. Write PromQL queries.
5. Create variables.
6. Configure alerts.
7. Share dashboard.

Common Metrics:
- CPU
- Memory
- Disk
- Network
- Pod Health
- Application Latency
- Error Rate
