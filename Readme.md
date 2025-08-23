# Tractor k8s ingress

This a helm char values file for deploying [Traefik v3](https://doc.traefik.io/traefik/) as an ingress controller in a Kubernetes cluster on Infomaniak Kubernetes Service.

It is based on the [official Traefik helm chart](https://doc.traefik.io/traefik/getting-started/kubernetes/).

## Usage

### Add the Traefik helm repository
```bash
helm repo add traefik https://traefik.github.io/charts
helm repo update
```

### Install the chart
```bash
helm install traefik traefik/traefik -f values.yaml --wait --namespace traefik --create-namespace
```

### Update the chart
```bash
helm upgrade traefik traefik/traefik -f values.yaml --wait --namespace traefik 
```

### Uninstall the chart
```bash
helm uninstall traefik --namespace traefik --wait
```

## Usage

To use the ingress controller, you can create a regular Kubernetes Ingress resource or a Traefik IngressRoute resource (see [examples](./examples/)).

You need to create a CNAME DNS record pointing to the load balancer CNAME:

```bind
<your-domain> IN CNAME traefik.k8s.tractor.scout.ch.
```
