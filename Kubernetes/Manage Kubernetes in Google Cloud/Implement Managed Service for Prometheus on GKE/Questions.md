You can write the solved task/question like this:

---

## Implement Managed Service for Prometheus on GKE

### Tasks Completed

1. Enabled the required Google Cloud APIs and verified organization policy constraints.

2. Created a GKE cluster with Managed Prometheus enabled:

```bash id="n4t6mf"
gcloud beta container clusters create gmp-cluster \
--num-nodes=1 \
--zone=us-west1-b \
--enable-managed-prometheus
```

3. Connected to the Kubernetes cluster:

```bash id="l3xg7m"
gcloud container clusters get-credentials gmp-cluster --zone=us-west1-b
```

4. Created the namespace for testing:

```bash id="u1w5qe"
kubectl create ns gmp-test
```

5. Downloaded and configured Prometheus.

6. Downloaded and started Node Exporter on port `9100`.

7. Created `config.yaml` with scrape configuration:

```yaml id="7w8zfk"
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: node
    static_configs:
      - targets: ['localhost:9100']
```

8. Uploaded the configuration file to Cloud Storage and made it publicly readable.

9. Started Prometheus with Managed Service for Prometheus exporter labels:

```bash id="xv4u3s"
./prometheus \
--config.file=config.yaml \
--export.label.project-id=$PROJECT \
--export.label.location=$ZONE
```

10. Verified metrics collection successfully:

```bash id="uv6r9b"
curl localhost:9090/api/v1/targets
```

Result:

```json id="w4r2cn"
"health":"up"
```

11. Verified GKE Managed Prometheus components:

```bash id="z9q2ta"
kubectl get pods -n gmp-system
```

Result:

* collector pod running
* gmp-operator running

---

This is a proper lab-answer/documentation format.
