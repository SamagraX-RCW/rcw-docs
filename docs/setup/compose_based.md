---
title: Compose Based Deployment
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
- Make cli for running Makefile

## Setting up the Pipeline
- ### **Clone the repository**
```
git clone https://github.com/SamagraX-RCW/devops.git
```


- ### **Run the scripts to install Docker Ansible and Vault Cli** 
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

![Jenkins init image](../assets/jenkins_init.png)

- ### **Install recommended plugins and restart Jenkins**
```
sudo systemctl restart jenkins
```
![Jenkins Restart image](../assets/jenkins_restart.png)

![Jenkins dashboard image](../assets/jenkins_dashboard.png)

- ### **Now run the compose file to deploy Registry, Nginx and Vault** 
```
docker compose up -d
```

![Docker Compose image](../assets/docker_compose_up.png)

- ### **Start RCW Services**

  - **Run the script to init the vault & generate unseal tokens**
  ```
  make start
  ```
