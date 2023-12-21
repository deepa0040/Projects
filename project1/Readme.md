# Deploy web application using Git, Jenkins, Ansible, Docker in your AWS Cloud

Step1: Create a github Repo and upload your code.

![image](https://github.com/deepa0040/Projects/assets/59830481/683553c4-46d2-4937-8fef-83907644a149)

Step2: Launch Virtual Machines (ec2)

![image](https://github.com/deepa0040/Projects/assets/59830481/dea54673-556d-425d-9522-0c41c2b27abd)

Name one Jenkins server and other Docker server

![image](https://github.com/deepa0040/Projects/assets/59830481/d8a9d4c3-9adb-490b-a739-47418d12ad63)


Step3: Login in Jenkins Server and run below commands.

```
sudo hostnamectl set-hostname jenkins
/bin/bash
sudo apt update
sudo apt install openjdk-17-jre (or required version)
```
# Install Docker
  ```
  sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update
  sudo apt-get install jenkins
```
```
sudo systemctl status jenkins.service
```
Step4: Edit Security group and open 22 and 8080 port

![image](https://github.com/deepa0040/Projects/assets/59830481/49cf0052-c4dd-48de-b385-88b3ba54b100)

Step5 Setup jenkins.

> Search <public_ip>:8080
> for example: http://13.232.180.60:8080/

You will get this page.

![image](https://github.com/deepa0040/Projects/assets/59830481/24e6d632-f9ec-4018-993d-7e693db64cb4)

In jenkins ec2 check jenkins status and copy unlock key from there.

```
sudo systemctl status jenkins.service
```
OR
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![image](https://github.com/deepa0040/Projects/assets/59830481/231394bb-77c4-472e-bdc3-f10b03026eb0)

Next select Install suggested plugin
It will take time to install plugins in docker, till then you can install docker in docker server.

Step5: Install docker in docker server.

```
sudo apt update
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
```
sudo systemctl status docker.service
```
Next run command <code>docker ps </code>
You will get below error , this is becoz , ubuntu user doesn't have the permission to connect to docker deomen and run any docker command
![image](https://github.com/deepa0040/Projects/assets/59830481/2eeb8635-afb8-4dc1-b1c9-d2a246de2ee6)
To resolve this we have to add the current user to user group.
```
sudo usermod -aG docker ubuntu
newgrp docker
docker ps
```
![image](https://github.com/deepa0040/Projects/assets/59830481/4512ddff-a652-4a79-b47f-1cc5d58904a7)

Step6:Next Install Ansible in Jenkins server
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```
Next we need to install docker dependency in jenkins server, for this run below command
```
sudo apt install python3-pip
pip install docker
```



