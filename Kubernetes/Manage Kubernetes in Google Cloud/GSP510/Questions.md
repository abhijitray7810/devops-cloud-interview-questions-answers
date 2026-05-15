# Manage Kubernetes in Google Cloud: Challenge Lab (GSP510)

## Challenge Scenario

Cymbal Shops asked to demonstrate Kubernetes and GKE management skills by completing several DevOps and monitoring tasks inside Google Cloud. The objective was to deploy applications on GKE, configure monitoring with Managed Prometheus, create alerting policies, debug Kubernetes deployment issues, containerize an application, push images to Artifact Registry, and expose services externally.

---

# Task 1: Create a GKE Cluster

## Objective

Create a GKE cluster named `hello-world-msgx` with the following configuration:

| Setting         | Value         |
| --------------- | ------------- |
| Zone            | us-central1-b |
| Release channel | Regular       |
| Autoscaling     | Enabled       |
| Nodes           | 3             |
| Minimum nodes   | 2             |
| Maximum nodes   | 6             |

## Commands Used

```bash
gcloud container clusters create hello-world-msgx \
  --zone us-central1-b \
  --release-channel regular \
  --num-nodes 3 \
  --enable-autoscaling \
  --min-nodes 2 \
  --max-nodes 6
```

## Configure kubectl Access

```bash
gcloud container clusters get-credentials hello-world-msgx \
--zone us-central1-b
```

## Verification

```bash
kubectl get nodes
```

Result:

* Cluster created successfully
* Nodes were in Ready state

---

# Task 2: Enable Managed Prometheus on the GKE Cluster

## Objective

Enable Managed Prometheus collection and deploy a sample monitoring application.

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

## Update Configuration

Updated the `<todo>` values in `prometheus-app.yaml`:

```yaml
image: nilebox/prometheus-example-app:latest
name: prometheus-test
ports:
  - name: metrics
```

## Deploy Application

```bash
kubectl apply -f prometheus-app.yaml -n gmp-xy23
```

## Download Pod Monitoring File

```bash
gcloud storage cp gs://spls/gsp510/pod-monitoring.yaml .
```

## Configure pod-monitoring.yaml

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

## Apply Pod Monitoring

```bash
kubectl apply -f pod-monitoring.yaml -n gmp-xy23
```

## Verification

```bash
kubectl get podmonitoring -n gmp-xy23
```

Result:

* PodMonitoring resource created successfully
* Prometheus test pods running correctly

---

# Task 3: Deploy an Application onto the GKE Cluster

## Objective

Deploy a Kubernetes manifest and inspect deployment issues.

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

Result:

```text
InvalidImageName
Failed to apply default image tag "<todo>"
```

The deployment failed because the image name inside the manifest was invalid.

---

# Task 4: Create a Logs-Based Metric and Alerting Policy

## Objective

Create monitoring metrics and alerts for Kubernetes pod image errors.

## Logs Explorer Query

```text
resource.type="k8s_pod"
severity>=WARNING
```

## Errors Observed

```text
Error: InvalidImageName
Failed to apply default image tag "<todo>"
```

## Create Logs-Based Metric

Metric Details:

| Setting     | Value            |
| ----------- | ---------------- |
| Metric Type | Counter          |
| Metric Name | pod-image-errors |

## Create Alerting Policy

### Alert Configuration

| Setting                 | Value                    |
| ----------------------- | ------------------------ |
| Rolling Window          | 10 min                   |
| Rolling Function        | Count                    |
| Time Series Aggregation | Sum                      |
| Condition Type          | Threshold                |
| Threshold               | Above 0                  |
| Trigger                 | Any time series violates |
| Notification Channel    | Disabled                 |
| Policy Name             | Pod Error Alert          |

## Result

* Alert policy created successfully
* Logs-based monitoring enabled

---

# Task 5: Update and Re-deploy the Application

## Objective

Fix the invalid image reference and redeploy the application.

## Update Image in Manifest

Updated image value inside `helloweb-deployment.yaml`:

```yaml
image: us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
```

## Delete Old Deployment

```bash
kubectl delete deployment helloweb -n gmp-xy23
```

## Deploy Updated Manifest

```bash
kubectl apply -f hello-app/manifests/helloweb-deployment.yaml -n gmp-xy23
```

## Verification

```bash
kubectl get pods -n gmp-xy23
```

Result:

* Pods running successfully
* No InvalidImageName errors

---

# Task 6: Containerize Code and Deploy onto the Cluster

## Objective

Update application version, build Docker image, push to Artifact Registry, and deploy it.

## Update Application Version

Modified `main.go`:

```go
Version: 2.0.0
```

## Build Docker Image

```bash
docker build -t us-central1-docker.pkg.dev/qwiklabs-gcp-00-ab312fd553bc/demo-repo/hello-app:v2 .
```

## Configure Docker Authentication

```bash
gcloud auth configure-docker us-central1-docker.pkg.dev
```

## Push Image to Artifact Registry

```bash
docker push us-central1-docker.pkg.dev/qwiklabs-gcp-00-ab312fd553bc/demo-repo/hello-app:v2
```

## Update Kubernetes Deployment Image

```bash
kubectl set image deployment/helloweb \
helloweb=us-central1-docker.pkg.dev/qwiklabs-gcp-00-ab312fd553bc/demo-repo/hello-app:v2 \
-n gmp-xy23
```

## Expose Deployment

```bash
kubectl expose deployment helloweb \
--name=helloweb-service-jpug \
--type=LoadBalancer \
--port=8080 \
--target-port=8080 \
-n gmp-xy23
```

## Verify Service

```bash
kubectl get svc -n gmp-xy23
```

## Final Output

Opened External IP in browser:

```text
Hello, world!
Version: 2.0.0
Hostname: helloweb-xxxxxxxxxx
```

---

# Final Result

Successfully completed the following:

* Created and configured a GKE cluster
* Enabled Managed Prometheus
* Deployed monitoring workloads
* Debugged Kubernetes deployment errors
* Created logs-based metrics and alert policies
* Fixed deployment image issues
* Built and pushed Docker images to Artifact Registry
* Updated Kubernetes deployments
* Exposed application using LoadBalancer service

---

# Skills Demonstrated

* Google Kubernetes Engine (GKE)
* Managed Prometheus
* Kubernetes Deployments & Services
* Logs-based Metrics
* Google Cloud Monitoring
* Alert Policies
* Docker Containerization
* Artifact Registry
* Kubernetes Troubleshooting
* DevOps & Cloud Operations
