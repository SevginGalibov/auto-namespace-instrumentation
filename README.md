# Auto Namespace Instrumentation Injector

This repository contains a Kubernetes controller (written in Go) and a Helm chart that deploys it.

## 📁 Structure

```
.
├── app/                # Go source code (controller logic)
│   ├── main.go
│   ├── go.mod
│   ├── go.sum
│   └── Dockerfile
├── chart/              # Helm chart
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│       ├── deployment.yaml
│       └── rbac.yaml
└── README.md
```

## 🛠️ Features

- Watches for new Kubernetes namespaces
- Applies OpenTelemetry `Instrumentation` resource automatically
- Respects an ignore list of namespaces
- Configurable collector endpoint and auth headers

## 🚀 Usage

### 1. Build & Push Docker Image

```bash
cd app
docker build -t ghcr.io/<username>/auto-namespace-intrumentation:latest .
docker push ghcr.io/<username>/auto-namespace-intrumentation:latest
```

### 2. Update `values.yaml`

Edit `chart/values.yaml` and set:
- `.image.repository`: your pushed image path
- `.collector.endpoint`: your OTEL collector
- `.otelHeader`: any auth headers
- `.ignoreNamespaces`: namespaces to skip

### 3. Install with Helm

```bash
helm install namespace-injector ./chart --namespace otel-system --create-namespace
```

### 4. Test

```bash
kubectl create ns test-ns
kubectl get instrumentation -n test-ns
```

You should see `auto-instrumentation` created automatically.

## 🧹 Uninstall

```bash
helm uninstall namespace-injector -n otel-system
```

---

MIT License
