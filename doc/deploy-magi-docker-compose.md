# Deploy magi using Docker Compose

This manual shows how to deploy magi, using Docker compose. We have created a Docker image with magi installed and we will use that image, to create a Spark cluster with 4 Docker containers, one running Spark master and the other three running Spark workers. 

The instructions have been tested on servers with a debian 8 or debian 9 OS. 

0. git clone magi
	```console
	$ git clone https://github.com/NTAP/magi.git
	```
1. Install Docker CE 
	```console
	$ sudo magi/scripts/install-docker-ce.sh
	```
If you want to be able to run docker commands without using sudo, you can add your user to the docker group. Log out and log back in for the change to take effect. 
	```console
	$ sudo usermod -aG docker $USER
	```  

2. Install docker compose
	```console
	$ sudo magi/scripts/install-docker-compose.sh
	```
3. Bring up the Docker containers
	```console
	$ sudo docker-compose -f magi/scripts/docker-compose.yml up -d
	```
4. List running containers
	```console
	$ sudo docker ps 
	```
5. Attach to the master container and add S3/Azure credentials in file /w/spark/conf/spark-defaults.conf
	```console
	$ sudo docker exec -it master bash  
	root@master:/#    vim /w/spark/conf/spark-defaults.conf
	```
Fill in spark.cloud.account/spark.cloud.secretkey, if you want to create/list/delete objects. If you want to copy objects between two object stores, fill in spark.sb.origin.account/ spark.sb.origin.secretkey/ spark.sb.dest.account/ spark.sb.dest.secretkey. If you are not using AWS S3, change the cloud to others and set other properties according to instructions in [README](../README.md).

6. Submit a job from the Spark master container
	```console
	root@master:/#    /w/spark/bin/spark-submit --master spark://master:7077 --class ListBucket /w/magi_2.11-1.2.jar --bucket atg-sync --prefix hex --summary
	```
Run `exit' to exit from the master container

7. Terminate docker containers
	```console
	$ cd magi/scripts/; sudo docker-compose down
	```
