# Java-Maven Artifact On Nexus
Run Nexus on Droplet DigitalOcean and publish Java-Maven Artifact to Nexus

## Technologies Used:
Nexus, DigitalOcean, Linux, Java, Gradle

## Project Description:
1-Create a cloud server on Droplet DigitalOcean and configure firewall with SSH port 22.

2-Install and configure Java 8 and Nexus from scratch on a cloud server.

3-Create a new Linux user with the ownership of the Nexus folders and run Nexus.

4-Create a new user on Nexus with the relevant permissions to developers team to upload and fetch artifacts on Nexus.

5-Build Jar file from Java Maven and publish it to Nexus.

6-Configure the Cleanup policy and Tasks for relevant repositories.

## Instructions:

### Step 1:

1- Sign up / sign in an account on the DigitalOcean and configure SSH keys.

2- Create a Droplet for Java Maven projects with Linux Ubuntu distribution in San Francisco Region by configuring the authentication method with SSH Key instead of Password.

3- Turn on the firewall for the server and configure the SSH with port 22 and specific ip address for Inbound traffics.

### Step 2:Prepare the server and install Java and Nexus

###### ssh into newly created server by root user
```
ssh -i .ssh/id_rsa root@24.199.124.10
```
###### Update Apt
```
apt update
```
###### Check Java installation status, install Java 8 and check java version
```
java
```
```
apt install openjdk-8-jre-headless
```
```
java -version
```
###### Install Nexus under /opt folder
```
wget https://download.sonatype.com/nexus/3/nexus-3.45.1-01-unix.tar.gz
```
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%204.28.04%20pm.png)
###### Unpack the downloaded Nexus file
```
tar -zxvf nexus-3.45.1.-01-unix.tar.gz
```
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%204.30.54%20pm.png)

### Step 3 :Add a Linux User to manage and run Nexus
###### Add a Linux User
```
adduser nexus
```
###### Check the ownership of the Nexus files
```
ls -l
```
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%204.32.53%20pm.png)
###### Change the ownership of Nexus files to newly-added Linux User
```
chown -R nexus:nexus nexus-3.45.1-01 
```
```
chown -R nexus:nexus sonatype-work
```
###### Check the ownership of Nexus files
```
ls -l
```
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%204.38.31%20pm.png)
### Step 4: Assign Newly-added user with the permission to run the Nexus
###### Update the nexus.rc file in /opt/nexus-3.45.1-01/bin folder
```
cd /opt
```
```
vim /nexus-3.45.1-01/bin/nexus.rc
```
###### update the 'run_as_user' is equal to 'nexus' in to nexus.rc file
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%204.43.43%20pm.png)
### Step 5: Start Nexus and update the firewall with listening port number
###### Switch to newly-added Linux user
```
su - nexus
```
###### Start Nexus by nexus user
```
/opt/nexus-3.45.1-01/bin/nexus start
```
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%204.48.18%20pm.png)
###### Update the firewall rule with listening port number 8081
```
netstat -lpnt
```
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%204.52.30%20pm.png)

![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%204.55.16%20pm.png)
### Login Nexus
###### Get username admin and password to login
```
ls /opt/sonatype-work/nexus3
```
```
cat /opt/sonatype-work/nexus3/admin.password
```
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%206.04.33%20pm.png)
###### Nexus UI
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-27%20at%208.46.21%20pm.png)
###### Create a new user of Nexus with relevant privilege role for developer team to upload and fetch artifact
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-28%20at%207.20.28%20pm.png)

![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-28%20at%207.20.44%20pm.png)
### Step 6 : Configure Java-Maven file for building and publishing
###### Add configuration into pom.xml file
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Maven-Nexus-DigitalOcrean/blob/master/image/Screenshot%202023-01-28%20at%2012.02.53%20am.png)
###### Add new nexus username and password into settings.xml under the .m2 file of current user.
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Maven-Nexus-DigitalOcrean/blob/master/image/Screenshot%202023-01-27%20at%207.21.54%20pm.png)

![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Maven-Nexus-DigitalOcrean/blob/master/image/Screenshot%202023-01-27%20at%207.21.46%20pm.png)
### Step 7: Build and Publish
###### build jar file in Java-maven-app root folder
```
mvn package
```
###### deploy the artifact to Nexus
```
mvn deploy
```
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Maven-Nexus-DigitalOcrean/blob/master/image/Screenshot%202023-01-28%20at%2012.43.47%20am.png)
### Step 8: Configure Cleanup policy and Task
###### Configure Cleanup policy with making deletion mark for all maven type repositories
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-28%20at%203.46.26%20pm.png)
###### Associate the cleanup policy with relevant repository
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-28%20at%203.47.03%20pm.png)
###### Create Task for empty storage daily
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-28%20at%203.48.51%20pm.png)

###### Outcomes after running cleanup policy and task for empty storage manually
![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-28%20at%203.49.54%20pm.png)

![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-28%20at%203.50.10%20pm.png)

![image](https://github.com/GLC-coder/DevOps-Artifact-Java-Gradle-Nexus-DigitalOcean/blob/master/image/Screenshot%202023-01-28%20at%203.50.39%20pm.png)
