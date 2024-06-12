# Gitlab CI/CD pipeline project

### In this project, we will see how to deploy a Nodejs application using Gitlab CI/CD.

### Pre-requisites to implement this project:
-  Create 1 virtual machine on AWS with 2 CPU, 4GB of RAM (t2.medium)
- Setup it as runner in Gitlab <a href="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Day-4/README.md">Runner</a>.
- Sudo permission
- Install docker

#
## Steps for CI/CD in Gitlab:

1) Become root user :
    ```bash
    sudo su
    ```
#
2) Provide Docker socket permission : 
    ```bash
    chmod 777 /var/run/docker.sock
    ```
#
3) Go to UI and create .gitlab-ci.yml file in the root directory of your repository
    ```bash
    vi .gitlab-ci.yml
    ```

#
4) Create two variables for docker login, to create variables <a href="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Day-3/README.md">Variables</a>

#
5) Paste the below code in .gitlab-ci file :
```bash
stages:
  - version-check
  - build-docker-file
  - push-docker-file
  - deploy

Version-Checks:
  stage: version-check
  tags:
    - linux
    - ubuntu
  script:
    - docker --version
    - npm --version

Docker-Build:
  stage: build-docker-file
  tags:
    - linux
    - ubuntu
  script:
    - docker build -t madhupdevops/nodetodoapp .
    - docker images

Push-Artifacts:
  stage: push-docker-file
  tags:
    - linux
    - ubuntu
  script:
    - docker login -u $DOCKER_USER -p $DOCKER_PASS
    - docker push madhupdevops/nodetodoapp:latest

Deploy:
  stage: deploy
  tags:
    - linux
    - ubuntu
  script:
    - docker run -itd --name nodeapp -p 8000:8000 madhupdevops/nodetodoapp:latest
```

#
6) Go to Build --> pipeline, to see the pipeline status.

#
7) Once all jobs are successfully, copy the public IP of your instance and access it on 
    ```bash
    <public-ip>:8000
    ```

#
