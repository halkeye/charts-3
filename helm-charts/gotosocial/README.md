# GoToSocial HELM Chart


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
helm repo add 0hlov3 http://schoenwald.me/helm-charts/
helm repo update
helm upgrade --install gotosocial 0hlov3/gotosocial --namespace gotosocial --create-namespace --set gotosocial.config.host='domain.tld' --set gotosocial.config.accountDomain='domain.tld'
```

### Create first User
```shell
kubectl exec -ti $CONTERNAR_ID -- /gotosocial/gotosocial --config-path /config/config.yaml admin account create --username $USERNAME --email $USER_EMAIL --password $USER_PASS
```

```shell
kubectl exec -ti $CONTERNAR_ID -- /gotosocial/gotosocial --config-path /config/config.yaml admin account confirm --username $USERNAME
```

```shell
kubectl exec -ti $CONTERNAR_ID -- /gotosocial/gotosocial --config-path /config/config.yaml admin account promote --username $USERNAME
```