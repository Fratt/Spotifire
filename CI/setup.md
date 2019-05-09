Spotifire CI setup
==================

## Docker
* [Install docker](https://docs.docker.com/install/linux/docker-ce/fedora/)
* Create a docker network using `docker network create spotifire`

## MariaDB
* Pull MariaDB image using `docker pull mariadb`
* Create and run a new container using `docker run --network spotifire --name mariadb-spotifire -e MYSQL_ROOT_PASSWORD=a-secret-password -d mariadb`
* Connect to it using `docker run -it --network spotifire --rm mariadb mysql -hmariadb-spotifire -uroot -p`
* Create a user for spotifire:
`CREATE OR REPLACE USER spotifire;`
* Create a database for spotifire
`CREATE OR REPLACE DATABASE spotifire; USE spotifire;`