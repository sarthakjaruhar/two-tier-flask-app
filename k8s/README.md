# How to setup two-tier application deployment on kubernetes cluster

### Pre-requisites
  - Ubuntu OS (Xenial or later)
  - sudo privileges
  - AWS- t2.medium instance type or higher
  - Internet access

## First setup kubernetes kubeadm cluster

  ```bash
  # using 'sudo su' is not a good practice.
sudo apt update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo apt install docker.io -y

sudo systemctl enable --now docker # enable and start in single command.

# Adding GPG keys.
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Add the repository to the sourcelist.
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update 
sudo apt install -y kubeadm=1.28.1-1.1 kubelet=1.28.1-1.1 kubectl=1.28.1-1.1 -y
```

1. **Use the following commands on the below one by one**

   ```bash
   git clone https://github.com/mdazfar2/two-tier-flask-app.git
   ```

   ```bash
   cd two-tier-flask-app/k8s
   ```
   - Now, execute below commands one by one
  
  ```bash
  kubectl apply -f twotier-deployment.yml
  ```

  ```bash
  kubectl apply -f twotier-deployment-svc.yml
  ```

  ```bash
  kubectl apply -f mysql-deployment.yml
  ```

  ```bash
  kubectl apply -f mysql-deployment-svc.yml
  ```

  ```bash
  kubectl apply -f persistent-volume.yml
  ```

  ```bash
  kubectl apply -f persistent-volume-claim.yml
  ```

2. **Now use the Ip and add the port to :30004**

3. 4. **Then execute the command for MYSQL-**

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

4. **Show your messages in you MYSQL Database**

   ```bash
   select * from messages;
   ```

5. **If you want to delete all the history from your database**

   ```bash
   DELETE FROM messages;
   ```
