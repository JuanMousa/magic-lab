# VM Setup

## Purpose

This Ubuntu virtual machine hosts my Linux and DevOps learning lab.
Configurations and exercises are stored in the magic-lab repository.

## Virtual Machine

| Component | Verified value |
|---|---|
| Virtualisation | VirtualBox |
| Operating system | Ubuntu 26.04.1 LTS (Resolute Raccoon) |
| Available logical CPUs | 6 |
| RAM reported by Linux | 5.3 GiB |
| Swap | 4.0 GiB |
| Root filesystem | /dev/sda2, mounted at / |
| Root filesystem capacity | 59G, as reported by df -h |

## Installed Tools

| Tool | Verified version |
|---|---|
| Docker client | 29.8.2 |
| Minikube | v1.39.0 |
| kubectl client | v1.37.1 |
| Bundled Kustomize | v5.8.1 |

## Kubernetes Environment

| Component | Verified value |
|---|---|
| Minikube profile | minikube |
| Driver | docker |
| Container runtime | containerd |
| Kubernetes version | v1.37.0 |
| Number of nodes | 1 |

The cluster was started, its context became active, and communication
was checked with kubectl get nodes.

## Verification Commands

Run these commands inside the VM:

```bash
cat /etc/os-release
nproc
free -h
df -h .
docker --version
minikube version
kubectl version --client
kubectl config current-context
minikube profile list
kubectl get nodes