# Deploy magi using Docker Compose

This manual shows how to deploy magi, using Docker compose. We have created a Docker image with magi installed and we will use that image, to create a Spark cluster with 4 Docker containers, one running Spark master and the other three running Spark workers. 

The instructions have been tested on servers with a Debian 8.3 OS. 

0. git clone magi

	$ git clone https://github.com/NTAP/magi.git

1. Install Docker CE 

	$ sudo magi/scripts/install-docker-ce.sh

2. Install docker compose

	$ sudo magi/scripts/install-docker-compose.sh

3. Bring up the Docker containers

	$ sudo docker-compose -f magi/scripts/docker-compose.yml up -d

4. List running containers

	$ sudo docker ps 

5. Attach to the master container and add S3/Azure credentials in file /w/spark/conf/spark-defaults.conf

	$ sudo docker exec -it master bash
	root@master:/# vim /w/spark/conf/spark-defaults.conf

Fill in spark.cloud.account/spark.cloud.secretkey, if you want to create/list/delete objects. If you want to copy objects between two object stores, fill in spark.sb.origin.account/spark.sb.origin.secretkey/spark.sb.dest.account/spark.sb.dest.secretkey. If you are not using AWS S3, change the cloud to others and set other properties according to instructions in README.

6. Submit a job from the Spark master container

	root@master:/# /w/spark/bin/spark-submit --master spark://master:7077 --class ListBucket /w/magi_2.11-1.2.jar --bucket atg-sync --prefix hex --summary


