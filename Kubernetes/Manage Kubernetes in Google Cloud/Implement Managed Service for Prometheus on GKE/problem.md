````markdown id="4m8qzn"
# Problems Faced During Implementation

## 1. Organization Policy API Not Enabled

### Error
```text
API [orgpolicy.googleapis.com] not enabled on project
````

### Solution

Enabled the API when prompted by Google Cloud CLI.

---

## 2. Prometheus Export Errors

### Error

```text
rpc error: code = Internal desc = Internal error encountered
```

### Cause

Temporary Google Cloud Monitoring export issue in Qwiklabs environment.

### Solution

Waited and continued running Prometheus. Metrics scraping still worked correctly.

---

## 3. Node Exporter Port Already in Use

### Error

```text
listen tcp :9100: bind: address already in use
```

### Cause

Node Exporter was already running in another terminal/session.

### Solution

Did not restart Node Exporter and verified metrics using:

```bash
curl localhost:9100/metrics
```

---

## 4. Metrics Output Too Large

### Error

```text
curl: (23) Failure writing output to destination
```

### Cause

Large metrics output while using `curl`.

### Solution

Used:

```bash
curl localhost:9090/metrics | head
```

and

```bash
curl localhost:9100/metrics | head
```

to limit displayed output.

---

## 5. metrics-server Pod Not Fully Ready

### Observation

```text
metrics-server-v1.35.1 0/1
```

### Solution

Ignored because it did not affect Managed Prometheus functionality or lab completion.

---

## 6. Rule Evaluator Deployment Showing 0/0

### Observation

```text
rule-evaluator 0/0
```

### Solution

This is normal in many Qwiklabs labs and did not affect monitoring setup.

---

# Final Verification

Verified successful metrics scraping with:

```bash
curl localhost:9090/api/v1/targets
```

Result:

```json
"health":"up"
```

```
```
