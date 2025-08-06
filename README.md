---

# 🐳 Containerization, Kubernetes, and Ansible – Homelab Setup Guide

## 🧰 Overview

This documentation describes how a homelab environment was configured using **Docker**, **Kubernetes (K8s)**, and **Ansible** to orchestrate and automate containerized infrastructure.

---

## 📦 1. Containerization with Docker

### 🔹 Purpose

* Isolate services in lightweight containers.
* Run applications reliably across environments.

### 🔹 Tools Used

* **Docker Engine**
* **Docker Compose**

### 🔹 Key Setup

```bash
# Install Docker (Debian/Ubuntu)
curl -fsSL https://get.docker.com | bash

# Run a container
docker run -d --name nginx-container -p 8080:80 nginx

# View running containers
docker ps
```

### 🔹 Docker Compose Example

```yaml
version: "3.9"
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

---

## ☸️ 2. Kubernetes (K8s) - Container Orchestration

### 🔹 Purpose

* Manage and scale container workloads across multiple nodes.
* Provide self-healing, load balancing, and rolling updates.

### 🔹 Tools Used

* **K3s** (Lightweight Kubernetes for homelab)
* **kubectl** (K8s CLI)

### 🔹 Install K3s (Single node)

```bash
curl -sfL https://get.k3s.io | sh -
```

### 🔹 Basic kubectl Commands

```bash
# Check nodes
kubectl get nodes

# Deploy a pod
kubectl run nginx --image=nginx --port=80

# Expose deployment
kubectl expose pod nginx --type=NodePort --port=80
```

---

## ⚙️ 3. Infrastructure Automation with Ansible

### 🔹 Purpose

* Automate Docker/K8s setup.
* Manage system configurations consistently.

### 🔹 Setup

```bash
# Install Ansible
sudo apt update && sudo apt install ansible -y

# Inventory example
echo "[docker-hosts]
192.168.1.100
" > inventory.ini
```

### 🔹 Ansible Playbook Example

```yaml
- name: Install Docker
  hosts: docker-hosts
  become: yes
  tasks:
    - name: Install Docker using script
      shell: curl -fsSL https://get.docker.com | bash
```

### 🔹 Run Playbook

```bash
ansible-playbook -i inventory.ini install-docker.yml
```

---

## 🧠 Benefits

* **Efficiency:** Automates provisioning and deployments.
* **Portability:** Docker ensures consistent environments.
* **Scalability:** Kubernetes allows handling more traffic and services.
* **Maintainability:** Ansible ensures easy replication and version-controlled infra.

---

## 📁 Directory Structure (Example)

```
homelab/
├── docker/
│   └── docker-compose.yml
├── k8s/
│   └── nginx-deployment.yml
├── ansible/
│   ├── inventory.ini
│   └── playbook.yml
└── README.md
```

---

## ✅ Future Enhancements

* Setup **K8s dashboard**
* Add **CI/CD** with GitLab or GitHub Actions
* Integrate **Monitoring** (Prometheus + Grafana)
* Use **Vault** for secrets management

---
