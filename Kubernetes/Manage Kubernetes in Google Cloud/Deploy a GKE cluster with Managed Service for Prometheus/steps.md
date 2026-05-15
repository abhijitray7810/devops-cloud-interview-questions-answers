# README.md

# Collect Metrics from Exporters using Managed Service for Prometheus

## Overview

This project demonstrates how to:

* Deploy a GKE cluster with Managed Service for Prometheus
* Configure PodMonitoring
* Deploy an example metrics application
* Install and run Prometheus locally
* Install and run Node Exporter
* Scrape and visualize metrics in Prometheus UI

---

# Prerequisites

* Google Cloud account
* Google Cloud Shell
* GKE API enabled
* kubectl installed
* Internet access

---

# Step 1: Create GKE Cluster

Create a Kubernetes cluster with Managed Prometheus enabled.

```bash
gcloud beta container clusters create gmp-cluster \
--num-nodes=1 \
--zone us-east1-c \
--enable-managed-prometheus
```

Connect kubectl to the cluster:

```bash
gcloud container clusters get-credentials gmp-cluster --zone=us-east1-c
```

Verify nodes:

```bash
kubectl get nodes
```

---

# Step 2: Create Namespace

Create namespace for monitoring resources.

```bash
kubectl create ns gmp-test
```

Verify namespace:

```bash
kubectl get ns
```

Check Managed Prometheus pods:

```bash
kubectl get pods -n gmp-system
```

---

# Step 3: Deploy Example Application

Deploy sample application exposing Prometheus metrics.

```bash
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.2.3/examples/example-app.yaml
```

Verify pods:

```bash
kubectl get pods -n gmp-test
```

Expected state:

```text
Running
```

---

# Step 4: Configure PodMonitoring

Apply PodMonitoring resource.

```bash
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.2.3/examples/pod-monitoring.yaml
```

Verify:

```bash
kubectl get podmonitoring -A
```

Expected output:

```text
gmp-test   prom-example
```

---

# Step 5: Download Prometheus Binary

Clone repository:

```bash
git clone https://github.com/GoogleCloudPlatform/prometheus
```

Move into directory:

```bash
cd prometheus
```

Checkout required version:

```bash
git checkout v2.28.1-gmp.4
```

Download Prometheus binary:

```bash
wget https://storage.googleapis.com/kochasoft/gsp1026/prometheus
```

Make executable:

```bash
chmod +x prometheus
```

---

# Step 6: Run Prometheus

Set variables:

```bash
export PROJECT=$(gcloud config get-value project)
export ZONE=us-east1-c
```

Run Prometheus:

```bash
./prometheus \
--config.file=documentation/examples/prometheus.yml \
--export.label.project-id=$PROJECT \
--export.label.location=$ZONE
```

Expected output:

```text
Server is ready to receive web requests
```

---

# Step 7: Install Node Exporter

Open a new Cloud Shell tab.

Download exporter:

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```

Extract archive:

```bash
tar xvfz node_exporter-1.3.1.linux-amd64.tar.gz
```

Move into directory:

```bash
cd node_exporter-1.3.1.linux-amd64
```

Run exporter:

```bash
./node_exporter
```

Expected output:

```text
Listening on address=:9100
```

---

# Step 8: Create Prometheus Config File

Create config file:

```bash
vi config.yaml
```

Add configuration:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: node
    static_configs:
      - targets: ['localhost:9100']
```

Save and exit.

---

# Step 9: Upload Config to Google Cloud Storage

Set project variable:

```bash
export PROJECT=$(gcloud config get-value project)
```

Create bucket:

```bash
gsutil mb -p $PROJECT gs://$PROJECT
```

Upload config:

```bash
gsutil cp config.yaml gs://$PROJECT
```

Set public access:

```bash
gsutil -m acl set -R -a public-read gs://$PROJECT
```

---

# Step 10: Run Prometheus with Config File

Go to Prometheus directory:

```bash
cd ~/prometheus
```

Run Prometheus using absolute config path:

```bash
./prometheus \
--config.file=/home/$USER/node_exporter-1.3.1.linux-amd64/config.yaml \
--export.label.project-id=$PROJECT \
--export.label.location=us-east1-c
```

---

# Step 11: Resolve Port Conflict

If error appears:

```text
bind: address already in use
```

Check running process:

```bash
netstat -tulpn | grep 9090
```

Or:

```bash
ss -tulpn | grep 9090
```

Kill process if needed:

```bash
pkill prometheus
```

Restart Prometheus.

---

# Step 12: Access Prometheus UI

1. Click Web Preview
2. Select Preview on port 9090

Run queries:

```promql
node_cpu_seconds_total
```

Other queries:

```promql
up
```

```promql
node_memory_MemAvailable_bytes
```

---

# Problems Faced

## Problem 1

```text
./node_exporter: No such file or directory
```

### Solution

Moved into correct directory.

---

## Problem 2

```text
./prometheus: No such file or directory
```

### Solution

Moved into Prometheus directory.

---

## Problem 3

```text
./prometheus: Is a directory
```

### Solution

Executed binary from inside correct folder.

---

## Problem 4

```text
config.yaml: no such file or directory
```

### Solution

Used absolute file path.

---

## Problem 5

```text
bind: address already in use
```

### Solution

Checked port usage and stopped old Prometheus process.

---

# Final Result

Successfully:

* Deployed GKE cluster
* Configured Managed Prometheus
* Installed Node Exporter
* Configured Prometheus scraping
* Collected metrics
* Visualized metrics in Prometheus UI
