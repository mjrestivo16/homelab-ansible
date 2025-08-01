# 🏠 Homelab Ansible Automation

This repository contains a full Ansible automation suite to deploy and manage a secure and feature-rich homelab environment including:

- Proxmox & TrueNAS integration
- Docker, Pi-hole, UPS monitor
- VLANs, Firewall, Node Exporter
- Tailscale VPN
- Prometheus + Grafana stack
- TrueNAS snapshot automation

---

## 📁 Structure

```bash
homelab-ansible/
├── ansible.cfg
├── inventories/
│   └── production/
│       ├── hosts.yml
│       └── group_vars/
├── playbooks/
├── roles/
│   └── [common, docker, pihole, ...]
├── unifi_config/
│   ├── vlans.json
│   └── firewall_rules.json
```

---

## 🚀 Usage

### Prerequisites

- Control node: Ubuntu with Ansible installed
- SSH key-based access to all nodes
- TrueNAS API key (if using `truenas_api`)
- Tailscale auth key (if using `vpn_tailscale`)

### 🔧 Inventory Setup

Edit `inventories/production/hosts.yml` to match your setup.

```yaml
all:
  children:
    proxmox:
      hosts:
        proxmox.local:
    truenas:
      hosts:
        truenas.local:
    ubuntu_vms:
      hosts:
        dockerhost.local:
        pihole.local:
        upsmon.local:
        grafana.local:
```

### 🔐 Secrets with Vault (optional)

To store secrets like API keys:

```bash
ansible-vault create group_vars/all/vault.yml
```

Add:
```yaml
tailscale_authkey: "tskey-xyz"
truenas_api_key: "your-api-key"
```

Use in playbooks by referencing `{{ vault_variable_name }}`.

### 🔑 Deploy SSH Keys

```bash
ansible-playbook -i inventories/production playbooks/00-ssh-key-deploy.yml
```

### 📜 Running Playbooks

ansible-playbook -i inventories/production playbooks/11-nginx-proxy.yml
ansible-playbook -i inventories/production playbooks/12-backup.yml
ansible-playbook -i inventories/production playbooks/13-grafana-dashboards.yml
ansible-playbook -i inventories/production playbooks/14-uptime-kuma.yml
ansible-playbook -i inventories/production playbooks/15-logging-loki.yml
ansible-playbook -i inventories/production playbooks/16-update-rpi.yml
ansible-playbook -i inventories/production playbooks/17-monitor-rpi.yml
ansible-playbook -i inventories/production playbooks/18-pihole-sync.yml
ansible-playbook -i inventories/production playbooks/19-dns-ha.yml
ansible-playbook -i inventories/production playbooks/20-prometheus-alerts.yml
ansible-playbook -i inventories/production playbooks/21-home-assistant.yml
```bash
ansible-playbook -i inventories/production playbooks/01-bootstrap.yml
ansible-playbook -i inventories/production playbooks/02-docker.yml
...
ansible-playbook -i inventories/production playbooks/10-vpn.yml
```

---

## 🌐 UniFi VLAN & Firewall Setup

If using UniFi Cloud Gateway:

- Use `unifi_config/vlans.json` and `firewall_rules.json` to manually import or via UniFi API.
- Or script this via `unifi` role (not included yet).

---

## 🧪 CI/CD Integration

You can automate playbook execution with GitHub Actions or GitLab CI. Let me know if you want an example pipeline.

---

## 🤝 Contributions & Extending

Add more roles like:

- NGINX Reverse Proxy
- Home Assistant
- Backup/Restore Routines
- ZFS Scrub & Report

Happy automating!
