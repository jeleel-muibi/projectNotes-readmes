# Create a README.md file summarizing Jeleel Muibi's hybrid homelab project
readme_content = """# 🌐 Jeleel Muibi's Hybrid Homelab Architecture

This document outlines the architecture, automation strategy, and monitoring setup for Jeleel Muibi's hybrid homelab project. It combines on-prem infrastructure with cloud-native scalability using Kubernetes, Prometheus, Jenkins, Terraform, and Azure.

---

## 🚀 Project Overview

A hybrid homelab that integrates:

- On-prem Kubernetes cluster hosted on Proxmox
- Autoscaling to Azure using Prometheus alerts and Jenkins-triggered Terraform
- VPN connectivity via automated CSR1000V provisioning
- Persistent storage using iSCSI targets
- Backup strategy using Dropbox
- CI/CD orchestration with Jenkins
- Monitoring and alerting with Prometheus + Grafana

---

## 🧱 Infrastructure Components

### 🏠 On-Prem Setup

- **Proxmox Cluster**: Hosts VMs for Kubernetes control plane, Jenkins, Prometheus, and other services.
- **EVE-NG**: Network emulation platform for routers, switches, and firewalls.
- **Windows/Linux Servers**: Managed via Ansible and monitored with Prometheus exporters.
- **iSCSI Storage**: Centralized storage served to VMs and Azure nodes via Ansible automation.

### ☁️ Cloud Setup (Azure)

- **Azure VMs**: Provisioned dynamically as Kubernetes worker nodes.
- **CSR1000V**: Cisco virtual router for VPN tunnel to on-prem.
- **Azure Arc**: Optional management of on-prem resources from Azure.
- **Terraform**: Infrastructure provisioning engine triggered by Jenkins.

---

## 🔄 Autoscaling Workflow

1. **Prometheus** monitors on-prem Kubernetes cluster.
2. **Alertmanager** detects unschedulable pods or high CPU/memory.
3. **Webhook triggers Jenkins**, which runs:
   - `terraform apply` to provision Azure VM
   - `cloud-init` or `Ansible` to configure and join VM to cluster
4. **CSR1000V VPN** is provisioned and configured automatically.
5. **VM connects to on-prem cluster** and begins handling workloads.

---

## 🔐 VPN Provisioning (CSR1000V)

- Terraform provisions CSR1000V in Azure.
- Configuration is applied via:
  - `cloud-init` startup script
  - `Ansible` playbook using `ios_config` module
- Tunnel connects to on-prem firewall/router using IPsec/IKEv2.

---

## 📦 Persistent Storage with iSCSI

- Ansible provisions iSCSI initiators on Azure VMs.
- Connects to on-prem iSCSI targets.
- Mounts and formats storage for persistent use.
- Ensures consistent data access across hybrid nodes.

---

## 🗃️ Backup Strategy

- Backups stored in **Dropbox** using:
  - `rclone` or Dropbox CLI
  - Scheduled sync via Jenkins or cron
- Optional encryption for secure cloud storage

---

## 🔧 CI/CD with Jenkins

- Jenkins runs locally and/or in Azure.
- Pulls code from GitHub (playbooks, Terraform modules).
- Executes multi-stage pipelines:
  - VLAN configuration via Ansible
  - VM provisioning via Terraform
  - Storage setup and backup sync

---

## 📊 Monitoring Stack

- **Prometheus**: Collects metrics from Kubernetes, servers, and network devices.
- **Grafana**: Visualizes metrics and logs.
- **Alertmanager**: Sends alerts to Jenkins or Slack/email.
- **Exporters**:
  - `node_exporter` for Linux
  - `windows_exporter` for Windows
  - `snmp_exporter` for switches/firewalls

---

## 🧠 Portfolio Impact

This project demonstrates:

- Hybrid cloud orchestration
- Infrastructure-as-code mastery
- Monitoring-driven autoscaling
- Secure network design
- CI/CD automation in restricted environments
- Disaster recovery planning

---

## 📁 Repository Structure (Suggested)
