
# Manage Kubernetes in Google Cloud: Challenge Lab (GSP510)

## Questions Solved in This Lab

---

## Task 1: Create a GKE Cluster

### Question

Create a Google Kubernetes Engine (GKE) cluster with the following configuration:

| Setting         | Value              |
| --------------- | ------------------ |
| Cluster Name    | `hello-world-msgx` |
| Zone            | `us-central1-b`    |
| Release Channel | `Regular`          |
| Number of Nodes | `3`                |
| Autoscaling     | `Enabled`          |
| Minimum Nodes   | `2`                |
| Maximum Nodes   | `6`                |

### Objective

Demonstrate the ability to create and configure a production-ready GKE cluster with autoscaling enabled.

---

## Task 2: Enable Managed Prometheus on the GKE Cluster

### Question

Enable Managed Prometheus on the GKE cluster and deploy a sample monitoring application with PodMonitoring configuration.

### Requirements

* Enable Managed Prometheus collection.
* Create a namespace named `gmp-xy23`.
* Deploy the Prometheus example application.
* Configure PodMonitoring to scrape metrics every `30s`.

### Objective

Demonstrate knowledge of Kubernetes monitoring and Managed Prometheus integration in Google Cloud.

---

## Task 3: Deploy an Application onto the GKE Cluster

### Question

Deploy the `helloweb` Kubernetes application manifest to the cluster and inspect deployment issues.

### Requirements

* Download deployment manifests from Cloud Storage.
* Deploy the application to namespace `gmp-xy23`.
* Investigate deployment failures related to container images.

### Expected Error

```text
InvalidImageName
Failed to apply default image tag "<todo>"
```

### Objective

Demonstrate troubleshooting skills for Kubernetes deployment and container image issues.

---

## Task 4: Create a Logs-Based Metric and Alerting Policy

### Question

Create a logs-based metric and alerting policy to monitor Kubernetes pod image errors.

### Requirements

#### Logs-Based Metric

| Setting     | Value              |
| ----------- | ------------------ |
| Metric Type | Counter            |
| Metric Name | `pod-image-errors` |

#### Alert Policy

| Setting        | Value                      |
| -------------- | -------------------------- |
| Rolling Window | `10 min`                   |
| Function       | `Count`                    |
| Aggregation    | `Sum`                      |
| Condition      | `Threshold`                |
| Threshold      | `Above 0`                  |
| Trigger        | `Any time series violates` |
| Policy Name    | `Pod Error Alert`          |

### Objective

Demonstrate skills in Google Cloud Logging, Monitoring, metrics creation, and alert configuration.

---

## Task 5: Update and Re-deploy the Application

### Question

Fix the invalid image reference inside the Kubernetes deployment manifest and redeploy the application.

### Requirements

* Replace the invalid image reference with:

```yaml
us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
```

* Delete the broken deployment.
* Re-deploy the corrected manifest.
* Verify pods are running successfully.

### Objective

Demonstrate deployment recovery and Kubernetes application management skills.

---

## Task 6: Containerize Code and Deploy onto the Cluster

### Question

Update the application version, build a Docker image, push it to Artifact Registry, and deploy the updated application onto GKE.

### Requirements

* Update application version to:

```text
Version: 2.0.0
```

* Build Docker image with tag `v2`.
* Push image to Artifact Registry.
* Update Kubernetes deployment image.
* Expose application using a LoadBalancer service.

### Expected Output

```text
Hello, world!
Version: 2.0.0
Hostname: helloweb-xxxxxxxxxx
```

### Objective

Demonstrate containerization, Artifact Registry usage, Kubernetes deployment updates, and service exposure.

---

# Overall Skills Demonstrated

* Google Kubernetes Engine (GKE)
* Kubernetes Deployments and Services
* Managed Prometheus
* Google Cloud Monitoring
* Logs-Based Metrics
* Alert Policies
* Docker Containerization
* Artifact Registry
* Kubernetes Troubleshooting
* DevOps and Cloud Operations
* Application Deployment and Scaling
