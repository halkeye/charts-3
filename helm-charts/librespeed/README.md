# LibreSpeed

[LibreSpeed](https://github.com/librespeed/speedtest)

```yaml
ingress:
  enabled: true
  className: "nginx"
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
    kubernetes.io/tls-acme: "true"
  hosts:
    - host: librespeed.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - secretName: tls-librespeed-general
      hosts:
        - librespeed.example.com

resources:
  limits:
    cpu: 200m
    memory: 256Mi
  requests:
    cpu: 200m
    memory: 256Mi
```