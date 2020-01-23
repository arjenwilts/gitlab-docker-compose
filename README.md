## Introduction to Docker, Docker-compose, GitLab and GitLab CI/CD to run a webserver locally

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

### 4. Using docker-compose
In a terminal window, navigate to the project directory where the docker-compose.yml file is located and execute:

`docker-compose up -d`

This will run a gitlab instance first. After the gitlab instance is running a gitlab-runner will be launched. A gitlab-runner is a "workhorse" that can run the build jobs we are creating in this lab.

### 5. Wait
Wait until the GitLab instance is running for approximately 5 minutes.

### 6. GitLab
Navigate in your browser to http://localhost

First you will be asked to enter a root password twice(8 characters at least)

In advance, login:

```
username: root
password: The password you just created 
```

### 7. GitLab runners
Locate the ![alt text](./ringsleutel.png "logo") logo, to navigate to the admin area.
In the admin area, navigate to Overview --> Runners and copy the registration token.

### 8. Register the GitLab runner
Open a terminal window and enter the gitlab runner docker container:

```docker exec -it git_gitlab-runner_1 bash```

In the gitlab runner container, execute the following script and replace the REGISTRATION_TOKEN with the copied token:

```
export REGISTRATION_TOKEN="<your gitlab_runner token>"
export URL="http://localhost/"

gitlab-runner register -n \
  --url $URL \
  --registration-token $REGISTRATION_TOKEN \
  --executor docker \
  --description "My Docker Runner" \
  --docker-image "docker:latest" \
  --docker-volumes /var/run/docker.sock:/var/run/docker.sock \
  --docker-network-mode "host"
  ```

You should get a message like:
```
Registering runner... succeeded                     runner=rgcnHnHx
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```

### 9. Create a project
Now navigate back to the home screen(http://localhost) and create a project bij clicking "New Project" (pick a name yourself :) )

### 10. Initalize your created repository
Initialze your created project using GIT by cloning it locally. These steps are shown in the page after you created a project and contain some lines like:

```
Create a new repository
git clone http://localhost/root/test.git
cd test
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

### 11. Gilab-ci
Locate the .gitlab-ci.yml and Dockerfile files from this project and copy those to your local cloned git repository from previous step. After copying perform a git push to your project:

```
git add .gitlab-ci.yml Dockerfile
git commit -m "ci pipeline" 
git push
```

### 12. Pipelines  
Navigate to CI/CD --> Pipelines in your project. A pipeline job will run with two stages: build_image and push_image.

### 13. Registry
In the previous step, a image is build and pushed to the gitlab registry. 
Now navigate to registry in the left column and click the `>` sign. You will see a image with the tag "latest" here. Click on the copy sign.

![alt text](./docker-image.png "logo")

### 14. Running the image
Now run the created image locally!
Open a terminal window and execute the following command:

```docker run -d -p 8080:80 localhost:4567/root/test:latest```

Next, navigate to http://localhost:8080 and you will see a the home screen of a running nginx webserver instance!







