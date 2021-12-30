# Install CRI-O and Kubernetes with Ansible

This Ansible Playbook will help you install package and configs need to set up Kubernetes with CRI-O on OS CentOS8 Linux. Component versions can be changed in vars/main.yml. Create file hosts and run

```
ansible-playbook 00_prebuild.yml
```