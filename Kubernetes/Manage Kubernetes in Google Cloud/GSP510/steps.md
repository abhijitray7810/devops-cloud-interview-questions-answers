
# Manage Kubernetes in Google Cloud: Challenge Lab (GSP510)

## Overview

This lab demonstrates how to create and manage a Google Kubernetes Engine (GKE) cluster, enable Managed Prometheus monitoring, deploy Kubernetes applications, create logs-based metrics and alerting policies, containerize applications using Docker, push images to Artifact Registry, and expose applications using LoadBalancer services.

---

# Task 1: Create a GKE Cluster

## Objective

Create a GKE cluster named `hello-world-msgx` with autoscaling enabled.

## Cluster Configuration

| Setting | Value |
|---|---|
| Zone | us-central1-b |
| Release Channel | Regular |
| Nodes | 3 |
| Min Nodes | 2 |
| Max Nodes | 6 |

## Create Cluster

```bash
gcloud container clusters create hello-world-msgx \
  --zone us-central1-b \
  --release-channel regular \
  --num-nodes 3 \
  --enable-autoscaling \
  --min-nodes 2 \
  --max-nodes 6
````

## Configure kubectl

```bash
gcloud container clusters get-credentials hello-world-msgx \
--zone us-central1-b
```

## Verify Cluster

```bash
kubectl get nodes
```

---

# Task 2: Enable Managed Prometheus on the GKE Cluster

## Enable Managed Prometheus

```bash
gcloud container clusters update hello-world-msgx \
  --zone us-central1-b \
  --enable-managed-prometheus
```

## Create Namespace

```bash
kubectl create namespace gmp-xy23
```

## Download Prometheus Application

```bash
gcloud storage cp gs://spls/gsp510/prometheus-app.yaml .
```

## Update prometheus-app.yaml

Replace the `<todo>` values with:

```yaml
image: nilebox/prometheus-example-app:latest
name: prometheus-test
ports:
  - name: metrics
```

## Deploy Prometheus Application

```bash
kubectl apply -f prometheus-app.yaml -n gmp-xy23
```

## Download Pod Monitoring File

```bash
gcloud storage cp gs://spls/gsp510/pod-monitoring.yaml .
```

## Update pod-monitoring.yaml

```yaml
apiVersion: monitoring.googleapis.com/v1
kind: PodMonitoring
metadata:
  name: prometheus-test
  labels:
    app.kubernetes.io/name: prometheus-test
spec:
  selector:
    matchLabels:
      app: prometheus-test
  endpoints:
  - port: metrics
    interval: 30s
```

## Apply Pod Monitoring Resource

```bash
kubectl apply -f pod-monitoring.yaml -n gmp-xy23
```

## Verify

```bash
kubectl get podmonitoring -n gmp-xy23
```

---

# Task 3: Deploy an Application onto the GKE Cluster

## Download Demo Application

```bash
gcloud storage cp -r gs://spls/gsp510/hello-app/ .
```

## Deploy Application

```bash
kubectl apply -f hello-app/manifests/helloweb-deployment.yaml -n gmp-xy23
```

## Verify Pods

```bash
kubectl get pods -n gmp-xy23
```

## Observed Error

```text
InvalidImageName
```

The deployment failed because of an invalid image reference.

---

# Task 4: Create a Logs-Based Metric and Alerting Policy

## Open Logs Explorer

Navigate to:

```text
Google Cloud Console → Logging → Logs Explorer
```

## Query Used

```text
resource.type="k8s_pod"
severity>=WARNING
```

## Errors Found

```text
Error: InvalidImageName
Failed to apply default image tag "<todo>"
```

---

## Create Logs-Based Metric

### Metric Configuration

| Setting     | Value            |
| ----------- | ---------------- |
| Metric Type | Counter          |
| Metric Name | pod-image-errors |

---

## Create Alert Policy

### Alert Configuration

| Setting              | Value                    |
| -------------------- | ------------------------ |
| Rolling Window       | 10 min                   |
| Function             | Count                    |
| Aggregation          | Sum                      |
| Condition Type       | Threshold                |
| Threshold            | Above 0                  |
| Trigger              | Any time series violates |
| Notification Channel | Disabled                 |
| Alert Policy Name    | Pod Error Alert          |

---

# Task 5: Update and Re-deploy the Application

## Update Image Reference

Edit `helloweb-deployment.yaml` and replace the invalid image with:

```yaml
image: us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
```

## Delete Existing Deployment

```bash
kubectl delete deployment helloweb -n gmp-xy23
```

## Re-deploy Updated Application

```bash
kubectl apply -f hello-app/manifests/helloweb-deployment.yaml -n gmp-xy23
```

## Verify Deployment

```bash
kubectl get pods -n gmp-xy23
```

Pods should now run successfully without errors.

---

# Task 6: Containerize Code and Deploy onto the Cluster

## Update Application Version

Edit `main.go`:

```go
Version: 2.0.0
```

---

## Configure Docker Authentication

```bash
gcloud auth configure-docker us-central1-docker.pkg.dev
```

---

## Build Docker Image

```bash
docker build -t us-central1-docker.pkg.dev/qwiklabs-gcp-00-ab312fd553bc/demo-repo/hello-app:v2 .
```

---

## Push Docker Image

```bash
docker push us-central1-docker.pkg.dev/qwiklabs-gcp-00-ab312fd553bc/demo-repo/hello-app:v2
```

---

## Update Deployment Image

```bash
kubectl set image deployment/helloweb \
helloweb=us-central1-docker.pkg.dev/qwiklabs-gcp-00-ab312fd553bc/demo-repo/hello-app:v2 \
-n gmp-xy23
```

---

## Expose Deployment as LoadBalancer

```bash
kubectl expose deployment helloweb \
--name=helloweb-service-jpug \
--type=LoadBalancer \
--port=8080 \
--target-port=8080 \
-n gmp-xy23
```

---

## Verify Service

```bash
kubectl get svc -n gmp-xy23
```

Open the external IP address in the browser.

## Expected Output

```text
Hello, world!
Version: 2.0.0
```

---

# Final Result

Successfully completed:

* GKE Cluster Creation
* Managed Prometheus Configuration
* Kubernetes Application Deployment
* Logs-Based Metrics
* Alerting Policies
* Kubernetes Troubleshooting
* Docker Image Build & Push
* Artifact Registry Integration
* LoadBalancer Service Exposure

---

# Skills Demonstrated

* Google Kubernetes Engine (GKE)
* Kubernetes Deployments & Services
* Managed Prometheus
* Google Cloud Monitoring
* Logs-Based Metrics
* Alert Policies
* Docker
* Artifact Registry
* DevOps Troubleshooting
* Cloud Operations

```
```
