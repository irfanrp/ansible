# Ansible Project - Nginx Load Balancer & Backend Configuration

## 📂 Project Structure

- files/
  - index_node2.html
  - index_node3.html
- inventory/
  - hosts
- playbooks/
  - change_index_page.yml
  - install_nginx_backend.yml
  - install_nginx.yml
  - setup_loadbalancer.yml
- templates/
  - node2.conf.j2
  - node3.conf.j2
- README.md

---

## ✅ **Description**

This Ansible project automates:

1. Installation and configuration of **NGINX on backend servers (Node2, Node3)**.
2. Deployment of **custom index.html** on each backend server.
3. Setup of **NGINX Load Balancer** with upstream to backend nodes.

Each backend node has:

- Its own NGINX config template.
- A custom index.html page served from `/var/www/html/index.html`.

---

## 🖥️ **Inventory Example (`inventory/hosts`):**
[loadbalancer]
node1 ansible_host=198.19.249.24

[backend]
node2 ansible_host=198.19.249.90
node3 ansible_host=198.19.249.134

> Adjust `ansible_host` and `ansible_user` to match your environment.

---

## 🚀 **How to Use**

### 1️⃣ Install NGINX on backend nodes & configure:

```bash
ansible-playbook playbooks/install_nginx_backend.yml -i inventory/hosts

### Change index.html (content update only):
### If you only want to update index page without touching nginx config:
ansible-playbook playbooks/change_index_page.yml -i inventory/hosts

### Setup load balancer
ansible-playbook playbooks/setup_loadbalancer.yml -i inventory/hosts



🔐 SSL/TLS handled by Cloudflare
## 🛡️ Cloudflare Zero Trust

This deployment assumes Cloudflare is configured with **Zero Trust policies** to restrict access to the load balancer.

- HTTPS → Cloudflare → Load Balancer (HTTP port 80)
- Zero Trust access policy 
- No SSL cert required on NGINX (unless Cloudflare flexsible Strict is used)

Test via:

```bash
curl -H "Host: nginx.aboutdevops.my.id" https://nginx.aboutdevops.my.id