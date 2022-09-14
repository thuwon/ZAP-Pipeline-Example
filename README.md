For the purposes of demonstrating a possible integration case using ZAP, a Jenkins and Docker in Docker (DinD) approach has been chosen. This environment has been set up using an Apple MacBook Pro 2019 version running macOS Montery Version 12.4 with Docker and Java 15.0.2 installed. Using docker as a basis for setting up the environment should ease reproducing the following results on different systems. Commands utilized may differ though, as it is the case for windows systems. But in general this guide should be applicable to all systems after setting up Jenkins and DinD.


In this set up, Jenkins will be used in a containerized form with the DinD working as a slave node. All necessary services will be ran inside the DinD container using docker as well. As a remark - this set up is potentially hazardous. To run docker inside a docker container itself, the container needs to run in a privileged mode, meaning the container has access to the host systems resources. This can for instance be abused to gain root access to the host's environment. Caution is advised.


To set up Jenkins and DinD, the [official guide](https://www.jenkins.io/doc/book/installing/docker/) is used with a slight variation. First, the DinD is started using the following shell command.
```
  docker run --name jenkins-docker --rm --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind --storage-driver overlay2
```

Please follow the [official guide](https://www.jenkins.io/doc/book/installing/docker/) to complete setting up Jenkins locally.

Furthermore, please set up your Jenkins instance to pull the Jenkinsfile from a remote repository as this.
![JenkinsConfExample!](/assets/JenkinsConfExample.png)
