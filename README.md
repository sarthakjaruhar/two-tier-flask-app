# Two Tier Flask App


## Overview- 
This project aims to deploy a scalable two-tier application running on Flask and MySQL, designed to handle 10,000 concurrent users while adhering to best DevOps practices. By leveraging Docker, Kubernetes, and AWS services, we ensure fault tolerance, scalability, and efficient deployment.

## Features
- **Scalability**: Designed to handle 10,000 concurrent users.
 
- **Fault Tolerance**: Utilizes Kubernetes on EKS for fault tolerance.

- **Containerization**: Dockerized application for portability and consistency.

- **Automated Deployment**: Automated Kubernetes cluster setup using HELM.

- **Continuous Integration/Continuous Deployment (CI/CD)**: Implemented DockerHub repository for versioning and continuous deployment.

- **Improved Downtime**: Reduced downtime by 60% with AWS Managed EKS.


# Step by Step guide for making this project-

1. **Execute the commands one by one, this is for Ubuntu**

    ```bash
    sudo apt install
    ```

   ```bash
   sudo apt update
   ```

   ```bash
   sudo apt install docker.io
   ```

   ```bash
   sudo chown $USER /var/run/docker.sock
   ```

   ```bash
   docker ps
   ```

2. **Now, execute below commands one by one**

   ```bash
   git clone https://github.com/mdazfar2/two-tier-flask-app.git
   ```

   ```bash
   cd two-tier-flask-app
   ```

   ```bash
   docker build . -t flaskapp
   ```

   ```bash
   docker images
   ```

   ```bash
   docker network create twotier
   ```

   ```bash
   docker run -d -p 5000:5000 --network=twotier -e MYSQL_HOST=mysql -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -e MYSQL_DB=myDB --name=flaskapp flaskapp:latest
   ```

   ```bash
   docker run -d -p 3306:3306 --network=twotier -e MYSQL_DATABASE=myDB -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -e MYSQL_ROOT_PASSWORD=admin --name=mysql mysql:5.7
   ```

3. **Now use the Ip and add the port to :5000**

4. **Then execute the command for MYSQL-**

   ```bash
   docker exec -it 634c54ae7bbb bash
   ```
> [!NOTE]
> Instead of 634c54ae7bbb use the `container Id` of MYSQL.

   ```bash
   mysql -u root -p
   ```
> [!NOTE]
> After run the up command then, Put the password `admin`.

  ```bash
  show databases;
  ```

  ```bash
  use myDB
  ```

  ```sql
  CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
  ```

5. **Show your messages in you MYSQL Database**

   ```bash
   select * from messages;
   ```

6. **If you want to delete all the history from your database**

   ```bash
   DELETE FROM messages;
   ```
