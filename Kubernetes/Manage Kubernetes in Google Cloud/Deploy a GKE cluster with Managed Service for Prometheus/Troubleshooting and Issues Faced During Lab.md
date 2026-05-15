# Troubleshooting and Issues Faced During Lab

## Lab

Collect Metrics from Exporters using the Managed Service for Prometheus

---

# Problems Faced and Solutions

## 1. node_exporter Binary Not Found

### Problem

While running:

```bash
./node_exporter
```

the following error appeared:

```bash
-bash: ./node_exporter: No such file or directory
```

### Cause

The command was executed from the wrong directory.

### Solution

Moved into the extracted Node Exporter directory:

```bash
cd node_exporter-1.3.1.linux-amd64
./node_exporter
```

---

## 2. Prometheus Binary Not Found

### Problem

While running:

```bash
./prometheus
```

the following error appeared:

```bash
-bash: ./prometheus: No such file or directory
```

### Cause

The command was executed inside the Node Exporter directory instead of the Prometheus directory.

### Solution

Moved into the Prometheus directory:

```bash
cd ~/prometheus
```

Then executed:

```bash
./prometheus
```

---

## 3. Prometheus Treated as Directory

### Problem

The following error occurred:

```bash
-bash: ./prometheus: Is a directory
```

### Cause

The command was executed from the home directory where `prometheus` existed as a folder.

### Solution

Entered the actual Prometheus directory first:

```bash
cd ~/prometheus
```

Then executed the binary:

```bash
./prometheus
```

---

## 4. Config File Not Found

### Problem

Prometheus failed with:

```bash
Error loading config
open ~/node_exporter-1.3.1.linux-amd64/config.yaml: no such file or directory
```

### Cause

The `~` shortcut did not expand correctly inside the Prometheus command argument.

### Solution

Used absolute file path:

```bash
--config.file=/home/$USER/node_exporter-1.3.1.linux-amd64/config.yaml
```

---

## 5. Port 9090 Already in Use

### Problem

Prometheus failed with:

```bash
Unable to start web listener
listen tcp 0.0.0.0:9090: bind: address already in use
```

### Cause

An old Prometheus process was already running on port 9090.

### Solution

Checked running process:

```bash
netstat -tulpn | grep 9090
```

and

```bash
ss -tulpn | grep 9090
```

Found existing Prometheus process and reused the running instance instead of launching another one.

---

## 6. lsof Command Not Available

### Problem

While checking port usage:

```bash
lsof -i :9090
```

the following error occurred:

```bash
-bash: lsof: command not found
```

### Cause

`lsof` package was not installed in Cloud Shell environment.

### Solution

Used alternative commands:

```bash
netstat -tulpn | grep 9090
```

and

```bash
ss -tulpn | grep 9090
```

---

# Final Outcome

Successfully:

* Created GKE cluster with Managed Prometheus
* Configured PodMonitoring
* Installed and ran Node Exporter
* Configured Prometheus scraping
* Collected exporter metrics
* Visualized metrics using Prometheus UI on port 9090
