## Using Docker and Docker-compose to run GitLab and GitLab CI/CD

### 1. Prerequisites
Install these packages locally:
- git
- docker
- docker-compose

Install instructions for docker: https://docs.docker.com/install/
                 docker-compose: https://docs.docker.com/compose/install/
                            git: https://www.linode.com/docs/development/version-control/how-to-install-git-on-linux-mac-and-windows/

### 2. Checkout repository
Execute the following command:
`git clone https://github.com/arjenwilts/gitlab-docker-compose.git`

This will create a copy of this github project on your laptop

### 3. Docker-compose 
Locate the docker-compose file and have a look at it using a terminal window or code editor.

### 4. Using docker-conmpose
In a terminal window, navigate to the project directory where the docker-compose.yml file is located and execute:
```docker compose up -d```