````markdown id="u5dbjw"
# Problems Faced During the Lab

## 1. Google Cloud Authentication Error

### Problem

While creating the GKE cluster, the following authentication error occurred:

```text
ERROR: (gcloud.container.clusters.create) You do not currently have an active account selected.
````

### Cause

Cloud Shell authentication session was not active for the lab account.

### Solution

Authenticated using:

```bash
gcloud auth login
```

Then selected the correct lab project and retried the cluster creation command.

---

# 2. PodMonitoring YAML Misconfiguration

## Problem

While editing `pod-monitoring.yaml`, the `<todo>` values were incorrectly replaced using the `sed` command.

Resulting incorrect configuration:

```yaml
name: 30s
app.kubernetes.io/name: 30s
app: 30s
```

### Cause

The `sed` replacement command replaced all matching values unintentionally.

### Solution

Recreated the entire `pod-monitoring.yaml` file manually with the correct configuration:

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

Applied the corrected file successfully.

---

# 3. InvalidImageName Error in Kubernetes Deployment

## Problem

The `helloweb` deployment failed with:

```text
InvalidImageName
```

### Cause

The deployment manifest contained an invalid container image reference placeholder (`<todo>`).

### Solution

Updated the image field in `helloweb-deployment.yaml`:

```yaml
image: us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
```

Deleted the old deployment and redeployed the application successfully.

---

# 4. Logs-Based Metric and Alert Policy UI Confusion

## Problem

While creating the alerting policy in Google Cloud Monitoring, some expected fields such as "Condition Type" were not visible in the updated UI.

### Cause

Google Cloud Monitoring UI had changed and automatically configured certain options.

### Solution

Used the available fields in the updated interface and configured:

* Rolling Window = 10 minutes
* Function = Count
* Aggregation = Sum
* Threshold = Above 0

Successfully created the `Pod Error Alert` policy.

---

# 5. Delay in LoadBalancer External IP Assignment

## Problem

After exposing the Kubernetes service, the external IP address was not immediately available.

### Cause

Google Cloud LoadBalancer provisioning requires several minutes.

### Solution

Waited a few minutes and verified the service using:

```bash
kubectl get svc -n gmp-xy23
```

After provisioning completed, the application became accessible through the external IP.

---

# 6. Docker Authentication for Artifact Registry

## Problem

Docker push operations required authentication with Artifact Registry.

### Solution

Configured Docker authentication using:

```bash
gcloud auth configure-docker us-central1-docker.pkg.dev
```

Successfully pushed the Docker image afterward.

---

# Final Outcome

All issues were identified, debugged, and resolved successfully. The Kubernetes applications, monitoring configuration, Docker image deployment, and LoadBalancer service worked correctly by the end of the lab.

```
```
