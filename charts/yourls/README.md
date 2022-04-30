# YOURLS Chart

## Usage

### Create NameSpace
```
kubectl create namespace yourls
```

### Create existing Secret
```shell
kubectl create secret generic yourls-secret --from-literal=password=$(openssl rand -hex 12) -n yourls
```

### Create existing Secret for mySQL
```shell
kubectl create secret generic yourls-mysql-secret --from-literal=mysql-root-password=$(openssl rand -hex 12) --from-literal=mysql-replication-password=$(openssl rand -hex 12) --from-literal=mysql-password=$(openssl rand -hex 12) -n yourls
```

### Create a values.yaml
```yaml
# Example values.yaml
yourls:
  site: https://example.com

ingress:
  enabled: true
  className: "nginx"
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
    kubernetes.io/tls-acme: "true"
  hosts:
    - host: example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - secretName: tls-yourls-general
      hosts:
        - example.com

```
### Deploy yourls
```
helm upgrade --install yourls maxxblow/yourls -f values.yaml
```

## Values
