## Installation Jenkins Process

### AWS EC2 Instance
* Launch EC2 Instance 

![image](https://user-images.githubusercontent.com/22033287/230765756-80a66283-bb4c-40bc-9d81-35c690ba08a1.png)


### Pre-Requisites:
* Java (JDK)

Install Java
```
sudo apt update
sudo apt install openjdk-11-jre
```
Verify Java is Installed
```
java -version
```

Now, you can proceed with installing Jenkins
```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

EC2 > Instances > Click on
In the bottom tabs -> Click on Security
Security groups
Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed All traffic).

![image](https://user-images.githubusercontent.com/22033287/230765926-a228840d-c409-4459-b9ea-d3ed99398db3.png)

## Login to Jenkins using the below URL:
http://:8080 [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing All Traffic to your EC2 instance 1. Delete the inbound traffic rule for your instance 2. Edit the inbound traffic rule to only allow custom TCP port 8080

After you login to Jenkins, - Run the command to copy the Jenkins Admin Password - sudo cat /var/lib/jenkins/secrets/initialAdminPassword - Enter the Administrator password

![image](https://user-images.githubusercontent.com/22033287/230765986-0b329cb8-9413-40fe-89a2-7e6e386f16c3.png)

### Click on Install suggested plugins
![image](https://user-images.githubusercontent.com/22033287/230766015-6eb52e9b-482b-414b-903a-3b1dd4f75f7c.png)

### Wait for the Jenkins to Install suggested plugins
![image](https://user-images.githubusercontent.com/22033287/230766022-0a633c88-4f25-47a3-8f72-680839b29eee.png)


Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]
![image](https://user-images.githubusercontent.com/22033287/230766030-3ad09434-0edc-4674-94bc-027bd47e3def.png)

Jenkins Installation is Successful. You can now starting using the Jenkins
![image](https://user-images.githubusercontent.com/22033287/230766038-11cf4e1c-9f8e-4c47-a20d-71c09af21495.png)

  
### Install the Docker Pipeline plugin in Jenkins:
* Log in to Jenkins.
* Go to Manage Jenkins > Manage Plugins.
* In the Available tab, search for "Docker Pipeline".
* Select the plugin and click the Install button.
* Restart Jenkins after the plugin is installed.

![image](https://user-images.githubusercontent.com/22033287/230767072-e865d3cb-0a92-48eb-a33c-762a49977ca1.png)


Wait for the Jenkins to be restarted.

#### Docker Slave Configuration
Run the below command to Install Docker
```
sudo apt update
sudo apt install docker.io
```

#### Grant Jenkins user and Ubuntu user permission to docker deamon.
```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```
Once you are done with the above steps, it is better to restart Jenkins.
```
http://<ec2-instance-public-ip>:8080/restart
```
The docker agent configuration is now successful.
