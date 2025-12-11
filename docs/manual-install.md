# Manual usage

To use these instructions, create a file valled `values.yaml` with the content found below.

## Add the Traefik helm repository
```bash
helm repo add traefik https://traefik.github.io/charts
helm repo update
```

## Install the chart
```bash
helm install traefik traefik/traefik -f values.yaml --wait --namespace traefik --create-namespace
```

## Update the chart
```bash
helm upgrade traefik traefik/traefik -f values.yaml --wait --namespace traefik 
```

## Uninstall the chart
```bash
helm uninstall traefik --namespace traefik --wait
```

## Values file

```yaml
ingressRoute:
  dashboard:
    enabled: true
providers:
  kubernetesGateway:
    enabled: true
gateway:
  listeners:
    web:
      namespacePolicy:
        from: All
service:
  spec:
    externalTrafficPolicy: Local # 👈 Preserve client source IP
  annotations:
    loadbalancer.openstack.org/proxy-protocol: "true" # 👈 Amphora LB Proxy Protocol einschalten
logs:
  access:
    enabled: true
ports:
  web:
    proxyProtocol:
      insecure: true
    redirections:
      entryPoint:
        to: websecure
        scheme: https
        permanent: true
  websecure:
    proxyProtocol:
      insecure: true

# Cert resovler specific configuration
## We should use cert-manager in the future
## https://doc.traefik.io/traefik/reference/install-configuration/tls/certificate-resolvers/acme/#using-letsencrypt-with-kubernetes
certificatesResolvers:
  letsencrypt:
    acme:
      email: "itkom@pbs.ch"
      storage: "/data/acme.json" # Path to store the certificate information.
      tlsChallenge: "true"
persistence:
  enabled: true
deployment:
  initContainers:
    # The "volume-permissions" init container is required if you run into permission issues. This seems to be the case for Infomaniak
    # Related issue: https://github.com/traefik/traefik-helm-chart/issues/396
    - name: volume-permissions
      image: busybox:1.36
      command:
        [
          "sh",
          "-c",
          "touch /data/acme.json; chmod -v 600 /data/acme.json; chown -R 65532:65532 /data/acme.json;",
        ]
      securityContext:
        runAsNonRoot: false
        runAsGroup: 0
        runAsUser: 0
      volumeMounts:
        - name: data
          mountPath: /data
```