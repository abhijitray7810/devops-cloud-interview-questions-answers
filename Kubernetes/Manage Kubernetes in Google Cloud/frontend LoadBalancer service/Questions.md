# Implement Continuous Delivery with Gemini

## Lab Overview

In this lab, the objective was to use Gemini in Google Cloud to configure Kubernetes infrastructure, deploy microservices, analyze logs, and create a private build environment using Cloud Build and Artifact Registry.

The lab focused on:

* Configuring Gemini permissions
* Creating and managing a GKE cluster
* Deploying microservices with Kubernetes
* Exploring logs using Gemini assistance
* Creating private container repositories
* Understanding Cloud Build private worker pools

---

# Task 1: Configure Environment and Gemini Access

## Objective

Configure the Google Cloud project and enable Gemini access for the lab environment.

---

## Set Project Variables

```bash
PROJECT_ID=$(gcloud config get-value project)
REGION=us-west1

echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"
```

---

## Store Active User

```bash
USER=$(gcloud config get-value account 2> /dev/null)

echo "USER=${USER}"
```

---

## Enable Gemini API

```bash
gcloud services enable cloudaicompanion.googleapis.com --project ${PROJECT_ID}
```

---

## Grant IAM Roles

```bash
gcloud projects add-iam-policy-binding ${PROJECT_ID} \
  --member user:${USER} \
  --role=roles/cloudaicompanion.user
```

```bash
gcloud projects add-iam-policy-binding ${PROJECT_ID} \
  --member user:${USER} \
  --role=roles/serviceusage.serviceUsageViewer
```

---

## Verification

Result:

* Gemini API enabled successfully
* IAM permissions added successfully

---

# Task 2: Configure Google Kubernetes Engine (GKE)

## Objective

Enable Kubernetes Engine API and create a zonal GKE cluster.

---

## Enable Kubernetes Engine API

```bash
gcloud services enable container.googleapis.com --project ${PROJECT_ID}
```

---

## Grant Container Admin Role

```bash
gcloud projects add-iam-policy-binding ${PROJECT_ID} \
  --member user:${USER} \
  --role=roles/container.admin
```

---

## Create GKE Cluster

```bash
gcloud container clusters create test \
    --project=${PROJECT_ID} \
    --zone=us-west1-b \
    --num-nodes=3 \
    --machine-type=e2-standard-4
```

---

## Verification

```bash
kubectl get nodes
```

Result:

* GKE cluster created successfully
* All nodes were in `Ready` state

---

# Task 3: Deploy Microservices to GKE

## Objective

Deploy the Online Boutique microservices application into the Kubernetes cluster.

---

## Clone Repository

```bash
git clone --depth=1 https://github.com/GoogleCloudPlatform/microservices-demo
```

---

## Deploy Kubernetes Manifests

```bash
cd ~/microservices-demo

kubectl apply -f ./release/kubernetes-manifests.yaml
```

---

## Verify Deployments

```bash
kubectl get deployments
```

Result:

```text
NAME                    READY   UP-TO-DATE   AVAILABLE
adservice               1/1     1            1
cartservice             1/1     1            1
checkoutservice         1/1     1            1
currencyservice         1/1     1            1
emailservice            1/1     1            1
frontend                1/1     1            1
loadgenerator           1/1     1            1
paymentservice          1/1     1            1
productcatalogservice   1/1     1            1
recommendationservice   1/1     1            1
redis-cart              1/1     1            1
shippingservice         1/1     1            1
```

---

## Get External Application URL

```bash
echo "http://$(kubectl get service frontend-external \
-o=jsonpath='{.status.loadBalancer.ingress[0].ip}')"
```

Example Output:

```text
http://8.229.183.212
```

---

## Verification

Result:

* Online Boutique application deployed successfully
* External LoadBalancer IP accessible from browser

---

# Task 4: Use Gemini to Understand Logs in GKE

## Objective

Use Gemini assistance to analyze Kubernetes workload logs.

---

## Logs Explorer Query

```text
resource.type="k8s_container"
resource.labels.cluster_name="test"
resource.labels.namespace_name="default"
```

---

## Gemini Log Analysis

Gemini explained log entries such as:

```text
GET /product/0PUK6V6EV0
```

Gemini identified:

* Kubernetes container source
* Pod name
* Namespace
* Cluster information
* HTTP request details
* Response status and latency

---

## Result

* Successfully filtered Kubernetes logs
* Gemini explained workload logs clearly

---

# Task 5: Create a Private Build Environment

## Objective

Understand Cloud Build private worker pools and create a private Artifact Registry repository.

---

## Attempt to Create Private Worker Pool

```bash
gcloud builds worker-pools create pool-test \
  --project=${PROJECT_ID} \
  --region=us-west1 \
  --no-public-egress
```

---

## Observed Error

```text
ERROR: (gcloud.builds.worker-pools.create)
FAILED_PRECONDITION:
project is unable to use private pools
```

---

## Explanation

The lab environment does not support private worker pools.

This error was expected and could be ignored during the lab.

---

# Task 6: Create Artifact Registry Repository

## Objective

Create a private Docker repository for container images.

---

## Create Docker Repository

```bash
gcloud artifacts repositories create my-repo \
  --repository-format=docker \
  --location=us-west1 \
  --description="My private Docker repository"
```

---

## Verification

Result:

* Artifact Registry repository created successfully
* Private Docker repository available for container storage

---

# Problems Faced

## 1. Project ID Initially Unset

### Error

```text
project (unset)
```

### Solution

Configured the project manually:

```bash
gcloud config set project qwiklabs-gcp-02-3e2774324577
```

---

## 2. External IP Not Immediately Available

### Issue

The frontend LoadBalancer service initially returned an empty IP.

### Solution

Waited a few minutes and reran:

```bash
kubectl get service frontend-external
```

---

## 3. Private Worker Pool Creation Failed

### Error

```text
FAILED_PRECONDITION:
project is unable to use private pools
```

### Solution

The lab instructions confirmed that private pools are disabled in the lab environment, so the error was ignored.

---

# Final Result

Successfully completed the following:

* Configured Gemini access and IAM permissions
* Created a Google Kubernetes Engine cluster
* Deployed microservices to Kubernetes
* Accessed the application externally
* Used Gemini to analyze Kubernetes logs
* Explored Cloud Build private worker pools
* Created a private Docker Artifact Registry repository

---

# Skills Demonstrated

* Google Kubernetes Engine (GKE)
* Kubernetes Deployments & Services
* Gemini for Google Cloud
* Cloud Logging & Logs Explorer
* Cloud Build
* Artifact Registry
* IAM & Google Cloud Permissions
* DevOps Operations
* Kubernetes Troubleshooting
* Containerized Application Deployment
