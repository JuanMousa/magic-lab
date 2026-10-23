# Magic Lab Linux & DevOps Home Lab

A growing Linux and DevOps lab built on an Ubuntu virtual machine.
I use this project to practise administration, run applications,
troubleshoot problems, and document what I learn.

## Goal

Build one connected lab where Linux, Docker, Kubernetes, Terraform,
Ansible, and automation work together.

The lab follows professional practices as it develops, including
version control, reviewed changes, and reproducible configurations.

## Current Environment

- Ubuntu 24.04 LTS running in VirtualBox
- Docker Engine
- Minikube with the Docker driver and containerd runtime
- kubectl for managing Kubernetes
- GitHub access through SSH

## Practice So Far

- Created an Nginx Deployment and scaled it to three replicas
- Created a ClusterIP Service for Nginx
- Practised port forwarding, reading logs, and Pod replacement
- Created this repository and its directory structure locally
- Used branches, commits, pull requests, and merges
- Added Git ignore rules for temporary files and local secrets

Existing lab exercises will be documented and added progressively.
The repository does not yet contain all configurations from the VM.

## Repository Structure

| Directory | Purpose |
|---|---|
| linux/ | Administration, networking, and troubleshooting |
| docker/ | Images, containers, and volumes |
| kubernetes/ | Deployments, Services, networking, and troubleshooting |
| terraform/ | Future infrastructure definitions |
| ansible/ | Future configuration management and playbooks |
| scripts/ | Reusable Bash and Python automation |
| docs/ | Environment setup, architecture, and learning notes |

Git does not track empty directories. Directories will appear on GitHub
as their first files are added.

## Planned Work

- Document the VM setup and completed exercises
- Add Kubernetes manifests for the Nginx application
- Develop reusable administration scripts
- Introduce Ansible and Terraform through practical exercises
- Add CI/CD as the lab develops

## Working Method

Each task uses a separate branch. Changes are committed, pushed,
reviewed through a pull request, and merged into main.

After merging, I update my local main before starting the next task.

## Credentials

Real passwords, tokens, private SSH keys, and cluster credentials
must stay out of the repository.

Example configuration files contain placeholders only.
