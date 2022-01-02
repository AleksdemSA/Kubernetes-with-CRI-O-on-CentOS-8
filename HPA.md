If you want the kubectl top command to work and you can use horizontal scaling, the cluster must have metric-server installed. For example: [https://github.com/kubernetes-sigs/metrics-server](https://github.com/kubernetes-sigs/metrics-server)

Check metric-server for nodes
```
kubectl get --raw "/apis/metrics.k8s.io/v1beta1/nodes"
```
or pods
```
kubectl get --raw "/apis/metrics.k8s.io/v1beta1/pods"
```

After installation and configuration (you may need to use the --kubelet-insecure-tls option), configure auto-scaling for ingress-nginx (example).

```
apiVersion: autoscaling/v2beta1
kind: HorizontalPodAutoscaler
metadata:
  name: ingress-nginx-controller
  namespace: ingress-nginx
  labels:
    app: ingress-nginx-controller
spec:
  scaleTargetRef:
    kind: Deployment
    name: ingress-nginx-controller
    apiVersion: apps/v1
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        targetAverageUtilization: 80
```

Using the ab utility, you can load an already configured ingress-service (example hostinfo) and see how HPA will work. Note that metric-server has a default timeout of 15s.