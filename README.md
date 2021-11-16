# Create Wordpress using Docker containers

This is a simple documentation to create a Wordpress website using Docker containers. In this, I am using 2 docker containers, one for database and one for website.

### Prerequisites
First, I am going to install Docker service in my host server. My host server is an AWS EC2. 
So, I am using *amazon-linux-extras* package manager to install the Docker service.

>Below commands install, restart and enable docker service in the host server.

```bash
# amazon-linux-extras install docker -y
# systemctl restart docker
# systemctl enable docker
```
Then, we need to create a custom Bridge in the docker. [Bridge](https://docs.docker.com/network/bridge/) is a virtual network device that enables communication between containers. There will be a default bridge in the host server which is named as **Bridge**.

*Default Bridge in the docker does not support communication between containers using container names.  It supports communication only through private IPs of the contianers and it will be dificult for us to configure and manage containers using private IPs.*

Custom Bridge in docker supports communication between containers through container names. So, I am going to create a custom Bidge here.

#### Creating custom Bridge

>I am creating a custom Bridge named **wpntwrk**.

```bash
# docker network create wpntwrk
```
Then, I am going to create 2 containers under this Bridge, one for databaase and one for website.

#### Creating Database container

>- Used latest docker image of MySQL from docker hub.
>- name of container is **database**
>- Container belongs to the network of custom bridge **wpntwrk**
>- provided ***root password, wordpress database name, user*** and ***password*** through envronment variables.
>- Started container in detatch mode

```bash
# docker container run \
--name database \
--network wpntwrk \
--restart always \
-d \
-e MYSQL_ROOT_PASSWORD=admin123 \
-e MYSQL_DATABASE=wpdb \
-e MYSQL_USER=wp \
-e MYSQL_PASSWORD=wp123 \
mysql:latest
```

#### Creating Website container

>- Used latest docker image of wordpress from docker hub.
>- name of container is **wordpress**
>- Container belongs to the network of custom bridge **wpntwrk**
>- provided ***wordpress db host, db user, db password *** and ***db name*** through envronment variables.
>- published port 80 of container and mapped to port 80 of host server.
>- Started container in detatch mode

```bash
# docker container run \
--name wordpress \
-d \
--restart always \
--network wpntwrk \
-p 80:80 \
-e WORDPRESS_DB_HOST=database \
-e WORDPRESS_DB_USER=wp \
-e WORDPRESS_DB_PASSWORD=wp123 \
-e WORDPRESS_DB_NAME=wpdb \
wordpress:latest
```
Now, the Wordpress website is ready to use and you can access the website through port 80 of the docker host server.

#### Sample screenshots

![](https://i.ibb.co/F47LjjP/t1.png)
![](https://i.ibb.co/zmqxWgW/t2.png)
![](https://i.ibb.co/0fypc1b/t3.png)
![](https://i.ibb.co/tcDMCT8/wp.png)

x------------------x---------------------x---------------------x-------------------x--------------------x---------------------x-------------------x
### ⚙️ Connect with Me 

<p align="center">
<a href="mailto:dilshad.lalu@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/dilshadkp/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 
<a href="https://www.instagram.com/dilshad_a.k.a_lalu/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://wa.me/%2B919567344212?text=This%20message%20from%20GitHub."><img src="https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white"/></a><br />
