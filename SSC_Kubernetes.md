# Kubernetes and self-signed certificate

Generate certificate (change CERT_NAME and DOMAIN)

```
openssl genrsa -out CERT_NAME.key 2048
openssl req -new -x509 -key CERT_NAME.key -out CERT_NAME.cert -days 3600 -subj /CN=DOMAIN
```

Create secret (change CERT_NAME once again)

```
kubectl create secret tls CERT_NAME --cert=CERT_NAME.cert --key=CERT_NAME.key -n CERT_NAME
```

Example ingress config (see spec: tls)

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  labels:
    app: CERT_NAME
  name: CERT_NAME
  namespace: default
spec:
  tls:
  - hosts:
    - DOMAIN
    CERT_NAME: CERT_NAME
  rules:
  - host: DOMAIN
    http:
      paths:
      - backend:
          service:
            name: CERT_NAME
            port:
              number: 8080
        path: /
        pathType: ImplementationSpecific
