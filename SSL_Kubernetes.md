# Kubernetes and Let's Encrypt certificate with cert-manager

Create NS for cert-manager
```
kubectl create ns cert-manager
```

Install cert-manager
```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.6.1/cert-manager.yaml
```

Create issuer [SSL_issuer.yml](SSL_ymls/SSL_issuer.yml) and cert [SSL_cert.yml](SSL_ymls/SSL_cert.yml) and apply it

```
kubectl apply -f SSL_issuer.yml
kubectl apply -f SSL_cert.yml
```

Check it...
```
kubectl describe certificate CERT_NAME
```
We need "Created new CertificateRequest resource". Check status, you must see "Order completed successfully".
```
kubectl describe order "Name from previous command"
```

If all is well, create an ingress for the service as in the example [SSL_ingress.yml](SSL_ymls/SSL_ingress.yml).

```
kubectl apply -f SSL_ingress.yml
```