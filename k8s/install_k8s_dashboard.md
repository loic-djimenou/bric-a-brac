# Install K8s dashboard locally

## Before running the commands bellow

The default way of doing this require some sort of authentication

Check out this blog on [how to install the dashboard wed ui](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

## Dashboard on local k8s without security

### Install the dashboard pods and k8s objects

Run

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.3.1/aio/deploy/recommended.yaml

```

> This example is using the version `v2.3.1` of the template.

### Patch the dashboard to allow skipping login

Run

```
kubectl patch deployment kubernetes-dashboard -n kubernetes-dashboard --type 'json' -p '[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value": "--enable-skip-login"}]'
```

### Proxy to k8s

Run

```
kubectl proxy

or

kubectl proxy --port=6666
```

Navigate to `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/`

> This example is using the port `8001` which is the default port unless one is specified.

## Add metrics server

### Install Metrics Server

Run

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.4.4/components.yaml
```

> This example is using the version `v0.4.4` of the template.

### Patch the metrics server to work with insecure TLS

Run

```
kubectl patch deployment metrics-server -n kube-system --type 'json' -p '[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value": "--kubelet-insecure-tls"}]'
```
