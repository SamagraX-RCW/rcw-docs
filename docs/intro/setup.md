---
title: Setup
---

<head>
  <title>Environment Setup | Node & NPM Environment for Ionic App Setup</title>
  <meta
    name="description"
    content="To get started with Ionic Framework, the only requirement is a Node & npm environment. Learn what environment setup is required for your Ionic apps."
  />
</head>

To get started with setting up the DevOps Pipeline.

### Pre-requisites:
- Jenkins Server for running pipelines
- Hashicorp Vault for storing secrets
- Docker Swarm (This could be your localhost as well)
- Private Docker Registry
- Ansible for deploying swarm nodes

### Setting up the Pipeline
- **Clone the repository**
```
git clone https://github.com/SamagraX-RCW/devops.git
```


- **Run the scripts to install Docker & docker-compose** 
```
chmod +x setup.sh
./setup.sh
```
<!-- - Get your SSL key from CA(Certified Authority) and paste it inside the ssl certificate(docker-registry.crt) -->

- **Now run the compose file to deploy Jenkins, registry & vault** 
```
docker compose up -d
```

- **Configure Vault token**
  - exec into the container for retrieving token that would be access SSL certs for nginx.
```
docker ps
```


- **Setup environment variables in Jenkins**
  - Go to **Dashboard** -> **Manage Jenkins** -> **System**
  - Add **VAULT_ADDR_DEV** and **VAULT_TOKEN_DEV** with corresponding values you got while setting up Hashicorp Vault
    - VAULT_ADDR_DEV = 

- **Add SSL Certificates in the Vault**

  - **Don't have SSL certificate**

- **Deploy Swarm Server**
  ```
  ansible-playbook -i ./ansible_workspace_dir/inventory/hosts ./ansible_workspace_dir/main.yml
  ```