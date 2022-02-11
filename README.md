# helm-charts
0hlov3s Public Helm Charts

## Add Charts
```shell
helm repo add 0hlov3 http://schoenwald.me/helm-charts/
helm repo update
```

## Development
### Lint the chart
```shell
helm lint helm-charts/*
```

### Create the Helm chart package
```shell
helm package helm-charts/*
```

### Create the Helm chart repository index
```shell
helm repo index --url http://schoenwald.me/helm-charts/ .
```