<h1 align="center">Install CRI-O and Kubernetes with Ansible</h1>


## Prebuild
This Ansible Playbook will help you install package and configs need to set up Kubernetes with CRI-O on OS CentOS8 Linux. Component versions can be changed in vars/main.yml. Create file hosts and run

```sh
ansible-playbook 00_prebuild.yml
```


## Examples

Pullign all images for your cluster

```sh
kubeadm config images pull
```


Create master-node (control-panel)

```sh
kubeadm init --pod-network-cidr=10.244.0.0/16
```

Copy kubeconfig in home directory

```sh
mkdir -p $HOME/.kube
yes | cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
```

Copy kubeconfig (run from desktop, change SERVER_NAME)

```sh
scp root@SERVER_NAME:/etc/kubernetes/admin.conf ~/.kube/config
```

Run network (calico)

```sh
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
kubectl get nodes -o wide
```

If you have one server, you can run pods on master-server

```sh
kubectl taint node $HOSTNAME node-role.kubernetes.io/control-plane:NoSchedule-
```

Run ingress with external IP (change YOUR_EXTERNAL_IP)

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
kubectl patch -n ingress-nginx svc ingress-nginx-controller -p '{"spec":{"externalIPs":["YOUR_EXTERNAL_IP"]}}'
kubectl get svc -A
sleep 60
curl -i YOUR_EXTERNAL_IP
```

Create testing application (change YOUR_DOMAIN)

```sh
kubectl create ns hostinfo
kubectl -n hostinfo create deployment hostinfo --image=aleksdem/hostinfo --port=8080 --replicas=10
kubectl -n hostinfo expose deployment hostinfo
kubectl -n hostinfo create ingress hostinfo --class=nginx --rule=hostinfo.YOUR_DOMAIN/*=hostinfo:8080
```


## Dynamic Volume Provisioning

* [Local Path Provisioner](https://github.com/rancher/local-path-provisioner) (accessModes: ReadWriteOnce)

Change default storage class. Get classes:

```sh
kubectl get storageclass
```

```sh
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```


## SSL and Kubernetes

* [Kubernetes and self-signed certificate](SSC_Kubernetes.md)
* [Kubernetes and Let's Encrypt certificate with cert-manager](SSL_Kubernetes.md)


## Check your cluster

* [Polaris](https://github.com/FairwindsOps/polaris)


## Need HPA (Horizontal Pod Autoscaling)?

* [HPA with metric-server](HPA.md)
