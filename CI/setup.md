Spotifire CI setup
==================

## Linux
**Disclaimer**: This guide was written for a Linux Fedora 27. Some commands might differ on a different distribution/version.

## Docker
* [Install docker](https://docs.docker.com/install/linux/docker-ce/fedora/)
* Create a docker network using `docker network create spotifire`

## MariaDB
* Pull MariaDB image using `docker pull mariadb`
* Create and run a new container using `docker run --network spotifire --name mariadb-spotifire -e MYSQL_ROOT_PASSWORD=a-secret-password -d mariadb`
* Connect to it using `docker run -it --network spotifire --rm mariadb-spotifire mysql -hmariadb-spotifire -uroot -p`
* Create a user for spotifire:
`CREATE OR REPLACE USER spotifire;`
* Create a database for spotifire
`CREATE OR REPLACE DATABASE spotifire; USE spotifire;`

## Jenkins
* Pull Jenkins image using `docker pull jenkins`
* Create and run a new container with `docker run --name jenkins-spotifire -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -d jenkins/jenkins:lts`
* Open the port 8080 by running `iptables -A INPUT -p tcp --dport 8080 -j ACCEPT`
* Navigate to http://your_box:8080/.
* Find the initial password with `docker exec -it jenkins-spotifire cat /var/jenkins_home/secrets/initialAdminPassword`
* Follow the instructions on the screen. I personnaly installed the following plugins:
  * Folders
  * OWASP Markup Formatter
  * Build Timeout
  * Timestamper
  * Workspace Cleanup
  * Pipeline
  * Pipeline: Stage View
  * Git
  * Email Extension
  * Mailer
  * Dashboard View
  * Credentials Binding
  * JUnit
  * GitHub Branch Source
  * Locale
* [More information](https://github.com/jenkinsci/docker/blob/master/README.md)


Spotifire CI administration
===========================

## Docker
* List all running containers with `docker ps`
* Start a container with `docker start mariadb-spotifire`
* Stop a container with `docker stop mariadb-spotifire`
* Delete a container with `docker rm mariadb-spotifire`
* Read the logs of a container with `docker logs mariadb-spotifire`

## MariaDB
* To administrate it, run `docker run -it --network spotifire --rm mariadb mysql -hmariadb-spotifire -uroot -p`

Spotifire CI backup
===================
TODO