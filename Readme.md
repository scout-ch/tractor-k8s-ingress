# Tractor k8s ingress

This a documentation repo for using [Traefik v3](https://doc.traefik.io/traefik/) as an ingress controller in a Kubernetes cluster on Infomaniak Kubernetes Service.

It is based on the [official Traefik helm chart](https://doc.traefik.io/traefik/getting-started/kubernetes/).

## Assumptions

This assumes, traefik is already installed in the cluster (eg. using FluxCD, see: https://github.com/scout-ch/tractor-k8s-shared/blob/main/infrastructure/traefik.yaml)

> [!TIP]  
> If you don't have traefik installed yet and want to do so manually, see [here](./docs/manual-install.md).

## Usage

The recommended approach is to use the Kubernetes Gateway API. See [examples](./examples/) for alls supported usaged. There is `whoami.yaml` whic illustrates a sample application.

However, we still support ingress resources and you can also use the traefik ingress crds.

> [!WARNING]  
> You need to set the DNS before you deploy your ingress, otherwise traefik will fail to get a certificate.

You need to create a CNAME DNS record pointing to the load balancer CNAME:

```bind
<your-domain> IN CNAME traefik.k8s.tractor.scout.ch.
```
