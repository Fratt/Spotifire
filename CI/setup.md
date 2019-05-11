Spotifire CI setup
==================

## Linux
**Disclaimer**: This guide was written for a Linux Fedora 27. Some commands might differ on a different distribution/version.

## Docker
* [Install docker](https://docs.docker.com/install/linux/docker-ce/fedora/)
* Create a docker network using `docker network create spotifire`

## MariaDB
* Pull MariaDB image using `docker pull mariadb`
* Create and run a new container using
`docker run --name mariadb-spotifire -d \
	--network spotifire  \
	-v mariadb:/var/lib/mysql \
	-e MYSQL_ROOT_PASSWORD=a-secret-password \
	mariadb`
* Connect to it using `docker run -it --network spotifire --rm mariadb mysql -hmariadb-spotifire -uroot -p`
* Create a user for spotifire:
`CREATE OR REPLACE USER spotifire;`
* Create a database for spotifire
`CREATE OR REPLACE DATABASE spotifire; USE spotifire;`

## Jenkins
* Pull Jenkins image using `docker pull jenkins`
* Create and run a new container with
`docker run --name jenkins-spotifire -d \
	-p 8080:8080 \
	-p 50000:50000 \
	-v jenkins_home:/var/jenkins_home \
	jenkins/jenkins:lts`
* Open the port 8080 by running `iptables -A INPUT -p tcp --dport 8080 -j ACCEPT`
* Navigate to http://your_box:8080/
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

## YouTrack
* Find the latest version number of YouTrack on [their DockerHub page](https://hub.docker.com/r/jetbrains/youtrack/tags)
* Pull JIRA image using `docker pull jetbrains/youtrack:2019.1.51932`
* Create and run a new container with
`docker run --name youtrack-spotifire -d \
	-p 8081:8080 \
    -v youtrack_data:/opt/youtrack/data \
    -v youtrack_conf:/opt/youtrack/conf \
    -v youtrack_logs:/opt/youtrack/logs \
    -v youtrack_backups:/opt/youtrack/backups \
	jetbrains/youtrack:2019.1.51932`
* Navigate to http://your_box:8081/
* Find the token with `docker exec -it youtrack-spotifire cat /opt/youtrack/conf/internal/services/configurationWizard/wizard_token.txt`

Spotifire CI administration
===========================

## Docker
* List all running containers with `docker ps`
* Start a container with `docker start mariadb-spotifire`
* Stop a container with `docker stop mariadb-spotifire`
* Delete a container with `docker rm mariadb-spotifire`
* Read the logs of a container with `docker logs mariadb-spotifire`
* List all volumes with `docker volume ls`
* Remove all volumes that are not used by any container anymore with `docker volume prune`

## MariaDB
* To administrate it, run `docker run -it --network spotifire --rm mariadb mysql -hmariadb-spotifire -uroot -p`

Spotifire CI backup
===================
## Run a backup of everything
`
TODO
`