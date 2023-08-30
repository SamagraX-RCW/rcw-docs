---
title: Swarm Based Deployment
---

<head>
  <title>Environment Setup</title>
  <meta
    name="description"
    content="Here we'll deploy our pipeline in the Docker Container"
  />
</head>

Here we'll deploy our pipeline in the Docker Container using compose.

## Pre-requisites:
- Jenkins Server for running pipelines
- Hashicorp Vault for storing secrets
- Docker Swarm (This could be your localhost as well)
- Private Docker Registry
- Ansible for deploying swarm nodes

## Setting up the Pipeline
- ### **Clone the repository**
```
git clone https://github.com/SamagraX-RCW/devops.git
```


- ### **Run the scripts to install Docker and Ansible** 
```
chmod +x ./scripts/setup.sh
./scripts/setup.sh
```
<!-- - Get your SSL key from CA(Certified Authority) and paste it inside the ssl certificate(docker-registry.crt) -->

- ### **Install and Start Jenkins Service**
```
chmod +x ./scripts/jenkins_init.sh
./scripts/jenkins_init.sh
```

- ### **Configure Jenkins Credentials for Private Registry**
    - #### **Update the Registry Credentials in Jenkins:** 

        **Dashboard > RCW > deploy-staging > Credentials > docker-server**
        
        Update with **https://nginx-reverse-proxy:80**, also create new credentials for registry username and password**

  - #### **Add Vault Server Address and Token Secret**




- ### **Now run the compose file to deploy Registry, Nginx and Vault** 
```
docker compose up -d
```

- ### **Configure Vault**

  - **Run the script to init the vault & generate unseal tokens**
  ```
  chmod +x setup_vault_gha.sh
  ./setup_vault_gha.sh
  ```

  ***Note: This will store the registry username & password (admin/admin) inside the vault***

  - **Run the script to get registry username and password from vault**

  ```
  chmod +x ./scripts/get_secrets.sh
  ./scripts/get_secrets.sh
  ```

- ### **SSL Configuration for Nginx**(Optional)
  - **Copy the SSL certificates and paste it inside the *nginx_config/ssl* folder**

  - **Now run the script**
    ```
    chmod +x ./scripts/set_up_ssl.sh
    ./scripts/set_up_ssl.sh
    ```
  
  - ***This script will store the ssl certs content inside the Vault as KV(key value) and keep as environment variable inside the Nginx container***

- ### **Deploy Swarm and other Services**
  - **Copy the hostname and paste in inside the inventory/hosts folder**

  - **Copy this command the paste this inside the script section inside the jenkins **

  ```
  ansible-playbook -i ./ansible_workspace_dir/inventory/hosts ./ansible_workspace_dir/main.yml
  ```

  - **Now Jenkins will run ansible playbook after the build has been successful**


- ### **Deploy the RCW Services**
  - **Run the RCW Compose file**
  ```
  docker compose -f rcw-compose.yml up -d
  ```

## Adding Ansible Roles for Services

- ### **Run the Script**

```
chmod +x /scripts/roles.sh
./scripts/roles.sh
```

- **Give the name of the role**

    eg. monitoring


- **Now give the variables for that role**

    eg. no. of replicas : 1/2/3