---
title: Deploying on Server
---

<head>
  <title>Environment Setup</title>
  <meta
    name="description"
    content="Here we'll deploy our pipeline in the Docker Container"
  />
</head>

To get started with setting up the DevOps Pipeline.

## Pre-requisites:
- Jenkins Server for running pipelines
- Hashicorp Vault for storing secrets
- Private Docker Registry
- Ansible for deploying swarm nodes

## Setting up the Pipeline
- ### **Clone the repository**
```
git clone https://github.com/SamagraX-RCW/devops.git
```


- #### **Copy the host ip address and paste it inside ansible_workspace_dir/inventory/hosts**
 


- ### **Run the scripts to install Ansible and start services on admin nodes** 

```
chmod +x ./scripts/ansible_init.sh
./scripts/ansible_init.sh
```


- ### **Configure Jenkins Credentials for Private Registry**
  **Go to** 

  Dashboard > RCW > deploy-staging > Credentials > docker-server > Update with **http://localhost:80**

  - Also add ***username and password*** inside Jenkins credentials


- ### **Initialize Docker Swarm server**

  ```
  ansible-playbook -i ./ansible_workspace_dir/inventory/hosts ./ansible_workspace_dir/main.yml
  ```

- ### **Initialize RCW services**
```
ansible-playbook -i ./ansible_workspace_dir/inventory/hosts ./rcw-playboook.yml
```

<!-- - ### **SSL Configuration for Nginx**(Optional)
  - **Copy the SSL certificates and paste it inside the *nginx_config/ssl* folder**

  - **Now run the script**
    ```
    chmod +x ./scripts/set_up_ssl.sh
    ./scripts/set_up_ssl.sh
    ```
  
  - ***This script will store the ssl certs content inside the Vault as KV(key value) and keep as environment variable inside the Nginx container***


  - **Now Jenkins will run ansible playbook after the build has been successful** -->


<!-- ## Adding Ansible Roles for Services

- ### **Run the Script**

```
chmod +x /scripts/roles.sh
./scripts/roles.sh
```

- **Give the name of the role**

    eg. monitoring


- **Now give the variables for that role**

    eg. no. of replicas : 1/2/3 -->