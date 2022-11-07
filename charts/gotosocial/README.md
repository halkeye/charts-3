# GoToSocial HELM Chart

## Deployment
### Create Namespace
```shell
kubectl create ns gts-test
```
### Create a PostgreSQL-Secret
```shell
kubectl create secret generic gts-postgresql-secret --from-literal="password=$(openssl rand -hex 32)" --from-literal="postgres-password=$(openssl rand -hex 32)" -n gts-test
```
### Deploy Helm Chart
```shell
helm repo add maxxblow https://maxxblow.de/charts
helm repo update
helm upgrade --install gotosocial maxxblow/gotosocial --namespace gotosocial --create-namespace --set gotosocial.config.host='domain.tld' --set gotosocial.config.accountDomain='domain.tld'
```
## Create first User
```shell
kubectl exec -ti $CONTAINER_ID -- /gotosocial/gotosocial --config-path /config/config.yaml admin account create --username $USERNAME --email $USER_EMAIL --password $USER_PASS
```

```shell
kubectl exec -ti $CONTAINER_ID -- /gotosocial/gotosocial --config-path /config/config.yaml admin account confirm --username $USERNAME
```

```shell
kubectl exec -ti $CONTAINER_ID -- /gotosocial/gotosocial --config-path /config/config.yaml admin account promote --username $USERNAME
```

## Example values.yaml
```yaml
gotosocial:
  persistence:
    enabled: true
    accessMode: "ReadWriteOnce"
    size: "5Gi"
  config:
    host: "gts.example.com"
    accountDomain: "gts.example.com"

postgresql:
  enabled: true
  auth:
    username: gotosocial
    # password:
    database: gotosocial
    existingSecret: gts-postgresql-secret
  primary:
    persistence:
      enabled: true
      size: 2Gi

ingress:
  enabled: true
  className: "nginx"
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
    kubernetes.io/tls-acme: "true"
  hosts:
    - host: gts.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - secretName: tls-gotosocial-general
      hosts:
        - gts.example.com

resources:
  limits:
    cpu: 250m
    memory: 512Mi
  requests:
    cpu: 250m
    memory: 512Mi
```
