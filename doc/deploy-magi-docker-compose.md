# Deploy magi using Docker Compose

This manual shows how to deploy magi, using Docker compose. We have created a Docker image with magi installed and we will use that image, to create a Spark cluster with 4 Docker containers, one running Spark master and the other three running Spark workers. 

The instructions have been tested on servers with a Debian 8.3 OS. 

0. git clone magi

	$ git clone https://github.com/NTAP/magi.git

1. Install Docker CE 

	$ sudo magi/scripts/install-docker-ce.sh

2. Install docker compose

	$ sudo magi/scripts/install-docker-compose.sh

3. Add S3/Azure credentials at the spark master node

	$ docker exec -it master bash
	$ vim /w/spark/conf/spark-defaults.conf

4. Submit a job from the Spark master container

	$ /w/spark/bin/spark-submit --master spark://master:7077 --class ListBucket /w/magi_2.11-1.2.jar --bucket atg-sync --prefix hex --summary


