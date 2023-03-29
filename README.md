
# Project-08
## :ledger: Index

- [About The Project](#beginner-about-the-project)
- [Problem that this project solves ](#question-problem-that-this-project-solves)
- [Solution to the problem.](#key-solution-to-the-problem)
- [Tools and Services](#hammer_and_wrench-tools-and-services)
- [Architecture of this project](#house-architecture-of-this-project)
- [Steps to execute the project](#zap-steps-to-execute-the-project)
  - [Login to AWS Account ](#key-login-to-aws-account )
  - [Create Key Pairs](#closed_lock_with_key-create-key-pairs)
  - [Create Security groups](#lock-create-security-groups)
    - [Jenkins](#lock-jenkins)
    - [Nexus](#lock-nexus)
    - [SonarQube](#lock-sonarqube)
  - [Launch Instances with user data](#bulb-launch-instances-with-user-data )
    - [Jenkins](#lock-jenkins)
    - [Nexus](#lock-nexus)
    - [SonarQube](#lock-sonarqube)
  - [Jenkins Post Installation](#earth_africa-jenkins-post-installation)
  - [Build Job](#hammer_and_wrench-build-job)
  - [Setup Slack Notification  ](#rocket-setup-slack-notification )
  - [Checkstyle code analysis Job](#package-checkstyle-code-analysis-job)
  - [Setup Sonar integration ](#lock-setup-sonar-integration)
  - [Sonar code analysis job](#earth_africa-sonar-code-analysis-job)
  - [Artifact upload job](#hammer_and_wrench-artifact-upload-job)
  - [Connect-all-jobs-together-with-Build-Pipeline](#hammer_and_wrench-connect-all-jobs-together-with-build-pipeline)
  - [Set Automatic build trigger](#hammer_and_wrench-set-automatic-build-trigger)
  - [Make Code change](#hammer_and_wrench-make-code-change)
  - [Change Branch](#package-change-branch)
  - [Create SG](#package-create-sg)
    - [Windows server](#package-windows-server)
    - [Tomcat and backend servers](#package-tomcat-and-backend-servers)
  - [Setup tomcat and backend server on ec2 with user data](#package-setup-tomcat-and-backend-server-on-ec2-with-user-data)
  - [Create Jenkins Job to Deploy artifact to Staging](#package-create-jenkins-job-to-deploy-artifact-to-staging)
  - [Add windows node as slave to Jenkins](#package-add-windows-node-as-slave-to-jenkins)
  - [Create Job to run Software Tests](#package-create-job-to-run-software-tests)
  - [Deploy artifact to Production tomcat server](#package-deploy-artifact-to-production-tomcat-server)
  - [Connect all the jobs with Build Pipeline](#package-connect-all-the-jobs-with-build-pipeline)
  - [Test it by committing code to github](#package-test-it-by-committing-code-to-github)
 - [Verify from browser](#earth_africa-verify-from-browser) 
 - [Resources](#page_facing_up-resources)
 - [Credit/Acknowledgment](#star2-creditacknowledgment)



## :beginner: About The Project

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

## :question: Problem that this project solves 

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

## :key: Solution to the problem.

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

## :hammer_and_wrench: Tools and Services
- visual studio code or an IDE
- AWS Account
- Nexus
- SonarQube
- Slack
- Maven
- JDK8
- Github Account

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>


## :house: Architecture of this project.

![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

## :zap: Steps to Execute the project. 

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :key: Login to AWS Account
- Login to your AWS management console. Create an AWS account if you don't have one. 

 ```sh
Account ID (12 digits) or account alias: <Enter your credentials>
IAM user name: <Your IAM user name>
Password: <Your password>
   ```
   
<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :closed_lock_with_key: Create Key Pairs

- On the console, search for `EC2` scrol down and select `key pair`-> `create key pair`.
- Create the  key pair that you use to login to your AWS services and name it as follows. This key will be used for all our instances.

 ```sh
Name: ci-vprofile-key
   ```

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :lock: Create Security groups

- Create three security group for the Jenkins,Nexux and Sonaqube Instancse.

#### :lock: Jenkins

- On your console under `EC2`->`security group` click `create security group`.
- Create a security group for the `Jenkins server` with the following details.

 ```sh
Name: vprofile-jenkins-sg
Description: vprofile-jenkins-sg
Tag: vprofile-jenkins-sg
Allow: 8080 from MY IP 
Allow: 22 from Anywhere from MY IP
Allow: 8080 from vprofile-sonarqube-sg
   ```

#### :lock: Nexus

- On your console under `EC2`->`security group` click `create security group`.
- Create a security group for the `Nexus server` with the following details.

 ```sh
Name: vprofile-nexus-sg
Description: vprofile-nexus-sg
Tag:  vprofile-nexus-sg
Allow: 8081 from MY IP
Allow: 22 from Anywhere from MY IP
Allow: 8081 from vprofile-jenkins-sg
   ```

#### :lock: SonarQube


- On your console under `EC2`->`security group` click `create security group`.
- Create a security group for the `SonQube server` with the following details.

 ```sh
Name: vprofile-sonarqube-sg
Description: vprofile-sonarqube-sg
Tag: vprofile-sonarqube-sg
Allow: 80 from MY IP
Allow: 80 from vprofile-jenkins-sg
   ```
   
<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :bulb: Launch Instances with user data 

- Now let's create three EC2 instances and launch Instances with user data
- 
#### :bulb: Jenkins

- On your console under `EC2`->`Instances` click `launch instances`.
- We will create Jenkins-server with below details.

```sh
Name: jenkins-server
AMI: Ubuntu 20.04
SecGrp: vprofile-jenkins-sg
InstanceType: t2.small
KeyPair: ci-vprofile-key
   ```
- Create the script to provision our `Jenkins server` with the Userdata script below.


```sh
#!/bin/bash
sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install maven -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
###
   ```

#### :bulb: SonarQube

- On your console under `EC2`->`Instances` click `launch instances`.
- We will create SonarQube-server with below details.

```sh
Name: sonar-server
AMI: Ubuntu 18.04
InstanceType: t2.medium
SecGrp: vprofile-sonar-sg
KeyPair: ci-vprofile-key
   ```

- Create the script to provision our `SonarQube server` with the Userdata script below.


```sh
#!/bin/bash
cp /etc/sysctl.conf /root/sysctl.conf_backup
cat <<EOT> /etc/sysctl.conf
vm.max_map_count=262144
fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
EOT
cp /etc/security/limits.conf /root/sec_limit.conf_backup
cat <<EOT> /etc/security/limits.conf
sonarqube   -   nofile   65536
sonarqube   -   nproc    409
EOT

sudo apt-get update -y
sudo apt-get install openjdk-11-jdk -y
sudo update-alternatives --config java

java -version

sudo apt update
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
sudo apt install postgresql postgresql-contrib -y
#sudo -u postgres psql -c "SELECT version();"
sudo systemctl enable postgresql.service
sudo systemctl start  postgresql.service
sudo echo "postgres:admin123" | chpasswd
runuser -l postgres -c "createuser sonar"
sudo -i -u postgres psql -c "ALTER USER sonar WITH ENCRYPTED PASSWORD 'admin123';"
sudo -i -u postgres psql -c "CREATE DATABASE sonarqube OWNER sonar;"
sudo -i -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;"
systemctl restart  postgresql
#systemctl status -l   postgresql
netstat -tulpena | grep postgres
sudo mkdir -p /sonarqube/
cd /sonarqube/
sudo curl -O https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.3.0.34182.zip
sudo apt-get install zip -y
sudo unzip -o sonarqube-8.3.0.34182.zip -d /opt/
sudo mv /opt/sonarqube-8.3.0.34182/ /opt/sonarqube
sudo groupadd sonar
sudo useradd -c "SonarQube - User" -d /opt/sonarqube/ -g sonar sonar
sudo chown sonar:sonar /opt/sonarqube/ -R
cp /opt/sonarqube/conf/sonar.properties /root/sonar.properties_backup
cat <<EOT> /opt/sonarqube/conf/sonar.properties
sonar.jdbc.username=sonar
sonar.jdbc.password=admin123
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
sonar.web.host=0.0.0.0
sonar.web.port=9000
sonar.web.javaAdditionalOpts=-server
sonar.search.javaOpts=-Xmx512m -Xms512m -XX:+HeapDumpOnOutOfMemoryError
sonar.log.level=INFO
sonar.path.logs=logs
EOT

cat <<EOT> /etc/systemd/system/sonarqube.service
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096


[Install]
WantedBy=multi-user.target
EOT

systemctl daemon-reload
systemctl enable sonarqube.service
#systemctl start sonarqube.service
#systemctl status -l sonarqube.service
apt-get install nginx -y
rm -rf /etc/nginx/sites-enabled/default
rm -rf /etc/nginx/sites-available/default
cat <<EOT> /etc/nginx/sites-available/sonarqube
server{
    listen      80;
    server_name sonarqube.groophy.in;

    access_log  /var/log/nginx/sonar.access.log;
    error_log   /var/log/nginx/sonar.error.log;

    proxy_buffers 16 64k;
    proxy_buffer_size 128k;

    location / {
        proxy_pass  http://127.0.0.1:9000;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_redirect off;
              
        proxy_set_header    Host            \$host;
        proxy_set_header    X-Real-IP       \$remote_addr;
        proxy_set_header    X-Forwarded-For \$proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto http;
    }
}
EOT
ln -s /etc/nginx/sites-available/sonarqube /etc/nginx/sites-enabled/sonarqube
systemctl enable nginx.service
#systemctl restart nginx.service
sudo ufw allow 80,9000,9001/tcp

echo "System reboot in 30 sec"
sleep 30
reboot
###
   ```
   
#### :bulb: Nexus

- On your console under `EC2`->`Instances` click `launch instances`.
- We will create Nexus-server with below details.

```sh
Name: nexus-server
AMI: Amazon Linux-2
InstanceType: t2.medium
SecGrp: vprofile-nexus-sg
KeyPair: vprofile-ci-key
KeyPair: ci-vprofile-key
   ```
- Create the script to provision our `Nexus server` with the Userdata script below.


```sh
#!/bin/bash
yum install java-1.8.0-openjdk.x86_64 wget -y   
mkdir -p /opt/nexus/   
mkdir -p /tmp/nexus/                           
cd /tmp/nexus/
NEXUSURL="https://download.sonatype.com/nexus/3/latest-unix.tar.gz"
wget $NEXUSURL -O nexus.tar.gz
EXTOUT=`tar xzvf nexus.tar.gz`
NEXUSDIR=`echo $EXTOUT | cut -d '/' -f1`
rm -rf /tmp/nexus/nexus.tar.gz
rsync -avzh /tmp/nexus/ /opt/nexus/
useradd nexus
chown -R nexus.nexus /opt/nexus 
cat <<EOT>> /etc/systemd/system/nexus.service
[Unit]                                                                          
Description=nexus service                                                       
After=network.target                                                            
                                                                  
[Service]                                                                       
Type=forking                                                                    
LimitNOFILE=65536                                                               
ExecStart=/opt/nexus/$NEXUSDIR/bin/nexus start                                  
ExecStop=/opt/nexus/$NEXUSDIR/bin/nexus stop                                    
User=nexus                                                                      
Restart=on-abort                                                                
                                                                  
[Install]                                                                       
WantedBy=multi-user.target                                                      
EOT
echo 'run_as_user="nexus"' > /opt/nexus/$NEXUSDIR/bin/nexus.rc
systemctl daemon-reload
systemctl start nexus
systemctl enable nexusSonarQube Server Setup
   ```

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :earth_africa: Jenkins Post Installation

-  Let's `SSH` into our `jenkins server` and check the status os Jenkins, let's also view the content of `/var/lib/jenkins/secrets/initialAdminPassword`

```sh
sudo -i /Doundloads/ci-viprofile-key.pem ubuntu@your Jenkins IP
systemctl status jenkins
cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

Go to browser, http://<public_ip_of_jenkins_server>:8080, enter initial Admin Passwrd and install suggested plugins and additional plugins. Then create your first admin user.

```sh
Maven Integration
Github Integration
Nexus Artifact Uploader
SonarQube Scanner
Slack Notification
Build Timestamp
   ```

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :earth_africa: Nexus Post Installation


- Login with SSH into nexus server and check system status for nexus also view the password at `/opt/nexus/sonatype-work/nexus3/admin.password`


```sh
sudo -i /Downdloads/ci-viprofile-key.pem ubuntu@your Nexus IP
systemctl status jenkins
cat /opt/nexus/sonatype-work/nexus3/admin.password
   ```
- Go to browser, http://<public_ip_of_nexus_server>:8081 , enter Username as `admin`, paste password from previous step, click on sign-in. Setup our `new password` and select Disable Anonymous Access.


- Let's create a repository that will be used to store our release artifacts with the following details.


```sh
maven2 hosted
Name: vprofile-release
Version policy: Release
   ```

- create a maven2 proxy repository which will store our dependencies. We will downdload our dependencies from this repository when ever we need them in our project

```sh
maven2 proxy
Name: vpro-maven-central
remote storage: https://repo1.maven.org/maven2/
   ```

- Create another repository which  will be used to store our snapshot artifacts.  any artifact with `-SNAPSHOT` extension will be stored in this repository

```sh
maven2 hosted
Name: vprofile-snapshot
Version policy: Snapshot
   ```
Finally, create a repository for `maven2 group type`. We will use this repository to group all maven repositories.

```sh
maven2 group
Name: vpro-maven-group
Member repositories: 
 - vpro-maven-central
 - vprofile-release
 - vprofile-snapshot
   ```

### :hammer_and_wrench: Build Job

-  Build the Artifact from our source code using Maven. Before our artifact, configure Maven and JDK8 in jenkins.

- Install JDK8 or latest on your server 

```sh
sudo apt update -y
sudo apt install openjdk-8-jdk -y
sudo -i
ls /usr/lib/jvm
### we should get both jdk-11 and jdk-8 in this path ###
java-1.11.0-openjdk-amd64  java-11-openjdk-amd64  openjdk-11
java-1.8.0-openjdk-amd64   java-8-openjdk-amd64
   ```

- Go to `Manage Jenkins`-> `Global Tool Configuration` configure the Installed `JDK8` manually, and specify its PATH `/usr/lib/jvm/java-8-openjdk-amd64' .

   
   
-- Add Nexus login credentials to Jenkins. Go to  ` Manage Jenkins`-> `Manage Credentials`-> `Global` -> `Add Credentials` 
 
 ```sh
username: admin
password: <pwd_setup_for_nexus>
ID: nexuslogin
description: nexuslogin
   ```
 - Create Jenkinsfile to build pipeline, code below
 - 
 ```sh
pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '172.31.10.139'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }
}
   ```
   
 - Create a New Job in Jenkins with properties below.
```sh
Pipeline from SCM 
Git
URL: <url_from_project> I will use SSH link
Crdentials: we will create github login credentials
#### add Jenkins credentials for github ####
Kind: SSH Username with private key
ID: githublogin
Description: githublogin
Username: git
Private key file: paste your private key here
#####
Branch: */ci-jenkins
path: Jenkinsfile
   ```


- Login jenkins server via SSH and complete host-key checking step. with command below, our host-key will be stored in .ssh/known_hosts file. Only then will error be fixed

 - Create a New Job in Jenkins with properties below.
 
```sh
sudo -i
sudo su - jenkins
git ls-remote -h -- git@github.com:Osomudeya/vpro-ci-jenkin.git HEAD
#####
Branch: */ci-jenkins
path: Jenkinsfile
   ```
- Now Build our project . Our build pipeline is successful


<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :rocket: Setup Slack Notification

- Login to `slack` and create a` workspace` by following the prompts. Then create a channel `jenkins-cicd` in our workspace

- Add jenkins to slack. 

- Go to Jenkins dashboard  `Configure system` -> `Slack`

 
```sh
Workspace:  example (in the workspace url example.slack.com)
credential: slacktoken 
default channel: #jenkins-cicd
   ```

- Add our sonar token to global credentials.

 
```sh
Kind: secret text
Secret: <paste_token>
name: slacktoken
description: slacktoken
   ```

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :package: Checkstyle code analysis Job

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :lock: Setup Sonar integration

-  Go to `Manage Jenkin`s -> `Global Tool Configuration` and configure Sonaqube


```sh
Add sonar scanner
name: sonarscanner
tick install automatically
   ```
- Next we need to go to Configure System, and find SonarQube servers section

```sh
tick environment variables
Add sonarqube
Name: sonarserver
Server URL: http://<private_ip_of_sonar_server>
Server authentication token: we need to create token from sonar website
   ```
   
 Add sonar token to global credentials.
 

```sh
Kind: secret text
Secret: <paste_token>
name: sonartoken
description: sonartoken
   ``` 
<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :earth_africa: Sonar code analysis job

- create a SonarQube code for our pipeline and commit/push changes to GitHub.


```sh
##new environment variables to be added to environment##
SONARSERVER = 'sonarserver'
SONARSCANNER = 'sonarscanner'
##new stages to be added##
 stage('CODE ANALYSIS with SONARQUBE') {
          
          environment {
             scannerHome = tool "${SONARSCANNER}"
          }
          steps {
            withSonarQubeEnv("${SONARSERVER}") {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
          }
   ``` 

- Our job will be triggerred and build automatically becauce of `the pull SM` cron job


- We can see quality gate results in SonarQube server.

- Let's create a Webhook in SonarQube to send the analysis results to jenkins

```sh
http://<private_ip_of_jenkins>:8080/sonarqube-webhook
   ``` 
- We will add below stage to our pipeline and commit changes to Github.


```sh
stage('QUALITY GATE') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
            }
            }
}
   ``` 
   
- Build the project 


<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :hammer_and_wrench: Artifact upload job
- We will automate publish latest artifact to Nexus repository after successful build. We also need to add `Build-Timestamp` to artifact name to get unique artifact each time we build. Go to `Manage Jenkins` -> `Configure System` under `Build Timestamp` add `yy-MM-dd_HHmm`.


- Also Add below stage to our pipeline

```sh
stage('UPLOAD ARTIFACT') {
                steps {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                        groupId: 'QA',
                        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                        repository: "${RELEASE_REPO}",
                        credentialsId: ${NEXUS_LOGIN},
                        artifacts: [
                            [artifactId: 'vproapp' ,
                            classifier: '',
                            file: 'target/vprofile-v2.war',
                            type: 'war']
                        ]
                    )
                }
        }
   ``` 
- Build the project 

- Artifact is uploaded to Nexus repository


<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :hammer_and_wrench: Connect all jobs together with-Build Pipeline

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :hammer_and_wrench: Set Automatic build trigger

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :hammer_and_wrench: Make Code change

- Add the code below to your Jenkinsfile in the same level with stages and push our changes.


```sh
post{
        always {
            echo 'Slack Notifications'
            slackSend channel: '#jenkinscicd',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
   ``` 
   
   
   

- We get our Notification from slack.

  
<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :package: Change Branch


- Login to your jenkins server.
- On your Jenkins dashboard, open the `Build` job click on `configure` and change to a new branch. We have to change to this new branch because it contains the code for continous delivery.


```sh
Scroll down to Source Code Management
Branch Specifier: */cd-jenkins
   ```

- Follow the procedure above and change the branch of all the jobs we created earlier but for the `Code Analysis` job add the following. 

- On your Jenkins dashboard, open the `Code Analysis` job click on `configure` and change to a new branch. 


```sh
Scroll down to Source Code Management
Branch Specifier: */cd-jenkins
Under post build action 
Report Violation
change checkstyle to 10 1000 100
save
   ```
   

<br/>
<div align="right">
    <b><a href="#Project-03">↥ back to top</a></b>
</div>
<br/>

### :package: Create SG

- Create two security group for the Windows server and Tomcat and backend servers


<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

#### :package: Windows server


- On your console under `EC2`-> `security group` click `create security group`.
- Create a security group for the `Windows server` with the following details.

```sh
Name: Windows-Server-SofTest
Description: Windows-Server-SofTest
Tag: Windows-Server-SofTest
Allow: RDP from MYIP
Allow: all traffic from vprofile-jenkins-sg

   ```


<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

#### :package: Tomcat and backend servers

- On your console under `EC2`-> `security group` click `create security group`.
- Create a security group for `Tomcat` with the following details.

```sh
Nam1e: vprofile-app-backend-staging
Description: vprofile-app-backend-staging
Tag: vprofile-app-staging-sg
Allow: 8080 from MY IP 
Allow: port 22 from MY IP
Allow: port 22 from vprofile-jenkins-sg
Allow: 8080 from Windows-Server-SofTest
Allow: RDP from MYIP
Allow: all traffic from vprofile-jenkins-sg

   ```

<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

### :package: Setup tomcat and backend server on ec2 with user data


- On your console under `EC2`->`Instances` click `launch instances`.
- We will create `Tomcat` with below details.


```sh
Name: Tomcat-server
AMI: Ubuntu 20.04
SecGrp: vprofile-app-backend-staging
InstanceType: t2.small
Tga: app01-staging-vprofile
Userdata: use the userdata given below.
KeyPair: ci-vprofile-key

   ```
   
 - Use the script below as userdata to launch our backend services


```sh
#!/bin/bash
sudo apt update
sudo apt install openjdk-8-jdk -y
sudo apt install git wget unzip -y
sudo apt install awscli -y
TOMURL="https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37.tar.gz"
cd /tmp/
wget $TOMURL -O tomcatbin.tar.gz
EXTOUT=`tar xzvf tomcatbin.tar.gz`
TOMDIR=`echo $EXTOUT | cut -d '/' -f1`
useradd --shell /sbin/nologin tomcat
rsync -avzh /tmp/$TOMDIR/ /usr/local/tomcat8/
rm -rf /usr/local/tomcat8/conf/tomcat-users.xml
rm -rf /usr/local/tomcat8/webapps/manager/META-INF/context.xml
touch /usr/local/tomcat8/webapps/manager/META-INF/context.xml
touch /usr/local/tomcat8/conf/tomcat-users.xml
cat <<EOT>> /usr/local/tomcat8/conf/tomcat-users.xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<tomcat-users xmlns="http://tomcat.apache.org/xml"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
version="1.0">
<!--
  NOTE:  By default, no user is included in the "manager-gui" role required
  to operate the "/manager/html" web application.  If you wish to use this app,
  you must define such a user - the username and password are arbitrary. It is
  strongly recommended that you do NOT use one of the users in the commented out
  section below since they are intended for use with the examples web
  application.
-->
<!--
  NOTE:  The sample user and role entries below are intended for use with the
  examples web application. They are wrapped in a comment and thus are ignored
  when reading this file. If you wish to configure these users for use with the
  examples web application, do not forget to remove the <!.. ..> that surrounds
  them. You will also need to set the passwords to something appropriate.
-->
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <user username="tomcat" password="admin123" roles="manager-gui,manager-script"/>
</tomcat-users>
EOT

cat <<EOT>> /usr/local/tomcat8/webapps/manager/META-INF/context.xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<Context antiResourceLocking="false" privileged="true" >
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
-->
</Context>
EOT
chown -R tomcat.tomcat /usr/local/tomcat8 
cat <<EOT>> /etc/systemd/system/tomcat.service
[Unit]
Description=Tomcat
After=network.target

[Service]
User=tomcat
WorkingDirectory=/usr/local/tomcat8
Environment=CATALINA_HOME=/usr/local/tomcat8
Environment=CATALINE_BASE=/usr/local/tomcat8
ExecStart=/usr/local/tomcat8/bin/catalina.sh run
ExecStop=/usr/local/tomcat8/bin/shutdown.sh
SyslogIdentifier=tomcat-%i

[Install]
WantedBy=multi-user.target
EOT

systemctl daemon-reload
systemctl start tomcat
systemctl enable tomcat


   ```

   
- On your console under EC2->Instances click launch instances.
- We will create Backend-server with below details.


```sh
Name: Backens-Server
AMI: centos 7
SecGrp: vprofile-app-backend-staging
InstanceType: t2.small
Tga: be01-staging-vprofile
Userdata: use the userdata given below.
KeyPair: ci-vprofile-key

   ```
   
- Use the script below as userdata to launch our backend services

```sh
#!/bin/bash
DATABASE_PASS='admin123'
yum update -y
yum install epel-release -y
yum install mariadb-server -y
yum install wget git unzip -y

#mysql_secure_installation
sed -i 's/^127.0.0.1/0.0.0.0/' /etc/my.cnf

# starting & enabling mariadb-server
systemctl start mariadb
systemctl enable mariadb

#restore the dump file for the application
cd /tmp/
wget https://raw.githubusercontent.com/devopshydclub/vprofile-repo/vp-rem/src/main/resources/db_backup.sql
mysqladmin -u root password "$DATABASE_PASS"
mysql -u root -p"$DATABASE_PASS" -e "UPDATE mysql.user SET Password=PASSWORD('$DATABASE_PASS') WHERE User='root'"
mysql -u root -p"$DATABASE_PASS" -e "DELETE FROM mysql.user WHERE User='root' AND Host NOT IN ('localhost', '127.0.0.1', '::1')"
mysql -u root -p"$DATABASE_PASS" -e "DELETE FROM mysql.user WHERE User=''"
mysql -u root -p"$DATABASE_PASS" -e "DELETE FROM mysql.db WHERE Db='test' OR Db='test\_%'"
mysql -u root -p"$DATABASE_PASS" -e "FLUSH PRIVILEGES"
mysql -u root -p"$DATABASE_PASS" -e "create database accounts"
mysql -u root -p"$DATABASE_PASS" -e "grant all privileges on accounts.* TO 'admin'@'localhost' identified by 'admin123'"
mysql -u root -p"$DATABASE_PASS" -e "grant all privileges on accounts.* TO 'admin'@'%' identified by 'admin123'"
mysql -u root -p"$DATABASE_PASS" accounts < /tmp/db_backup.sql
mysql -u root -p"$DATABASE_PASS" -e "FLUSH PRIVILEGES"

# Restart mariadb-server
systemctl restart mariadb
# SETUP MEMCACHE
yum install memcached -y
systemctl start memcached
systemctl enable memcached
systemctl status memcached
memcached -p 11211 -U 11111 -u memcached -d
sleep 30
yum install socat -y
yum install wget -y
wget https://www.rabbitmq.com/releases/rabbitmq-server/v3.6.10/rabbitmq-server-3.6.10-1.el7.noarch.rpm
rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
yum update
rpm -Uvh rabbitmq-server-3.6.10-1.el7.noarch.rpm
systemctl start rabbitmq-server
systemctl enable rabbitmq-server
systemctl status rabbitmq-server
echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config
rabbitmqctl add_user test test
rabbitmqctl set_user_tags test administrator
systemctl restart rabbitmq-server



   ```
   
<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

### :package: Create Jenkins Job to Deploy artifact to Staging


- Goto your AWS console copy the SonarQube public IP address.
- Paste the IP on your browser and login into the server using the your server credentials. 
- Change the quality gates of the SonarQube server to allow 100 bugs in the code. 



- Open your browser and enter `jenkins public IP` on your url. Login into your Jenkins server using your credential. 
- On your jenkins dashboard, goto `Manage jenkins`-> `manage plugins` and install a plugin called `Deploy to container`
- After installing the plugin, create a job by going to `New Item` and use the following detals.


```sh
Item name: vprofile-staging-deploy
click freestyle
Scroll down  to Build and add build step
select copy the artifact from Build Job that was created earlier.
Artifact to copy: **/*.war
save
   ```
- RUN this job and when it is done you should have your artifact under the workspace
- After running the job, open the `vprofile-staging-deploy`job and goto `configure` and macke the following changes to the job.

```sh
Scroll down to post build action
Add post buil action
select deploy war/ear to a container
WAR/EAR file: target/vprofile-v2.war
context path:vproapp
container: tomcat 8
Add credentials: 
username: tomcat
password:admin123
ID: tomcat-stagging-Login
Description: tomcat-stagging-Login
click add
select the credentials you just added
url: <private IP of tomcate>:8080
SAVE CHANGES
   ```
- RUN this job and when it is done, open your browser and enter  <tomcat public IP>:8080 go to the manager app of tomcat and login using the credentials username `tomcat`, password `admin123`  and you should see our `vproapp` automatically deployed to the server. Click on it and you should see the login page.

  


- On your Jenkins dashboard, open the `Build` job click on `configure` and make the following changes.This script will update our application.properties file to connect to our database.

- Replace db01, mc01 and rmq01 in the script below with the private ip of your backend server on AWS 
- 
```sh
Scroll down to Build  
execute shell: copy and paste the cript below.

cat << EOT > src/main/resouces/application.properties

#JDBC Configutation for Database Connection
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://db01:3306/accounts?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull
jdbc.username=admin
jdbc.password=admin123

#Memcached Configuration For Active and StandBy Host
#For Active Host
memcached.active.host=mc01
memcached.active.port=11211
#For StandBy Host
memcached.standBy.host=127.0.0.2
memcached.standBy.port=11211

#RabbitMq Configuration
rabbitmq.address=rmq01
rabbitmq.port=5672
rabbitmq.username=test
rabbitmq.password=test

#Elasticesearch Configuration
elasticsearch.host =192.168.1.85
elasticsearch.port =9300
elasticsearch.cluster=vprofile
elasticsearch.node=vprofilenode
EOT

   ```
 
- connect the job to your pipeline  
  
- On your Jenkins dasboard, open the `Deploy-to-Nexus` job click on `configure` and make the following changes.


```sh
Scroll down to post build action
Add post buil action
select build on other projects 
project to build : vprofile-stagging-deploy 
SAVE CHANGES
   ```
- On your jenkins dashboard, goto `Manage jenkins`-> `manage plugins` and install a plugin called `HTTP request`
- After installing the plugin, create a job by going to `New Item` and use the following detals.


```sh
Item name: TestURL
click freestyle
Scroll down  to Build and add build step
select execute shell
command: sleep 60
add another build step
select HTTP request
URL: http://<tomcat server public Ip>:8080/vproapp/login
save
   ```
  
- connect the job to your pipeline   
- On your Jenkins dasboard, open the `vprofile-stagging-deploy ` job click on `configure` and make the following changes.


```sh
Scroll down to post build action
Add post buil action
select build on other projects 
project to build : TestURL
Add another post buil action
  select slack Notification
Choose the options you wan to get notified for.
  click on advance 
  select slack token
  channel:#vprofile-jenkins 
  test connection
SAVE CHANGES
   ```
  
  
- Also add slack notification to all jobs in your pipeline.
  
  

- Create a new view and called it vprofile-continuous-delivery click pipeline view then  ok

- On edit view enter the following details 

```sh
initial job: Build 
click ok
   ```
   
- Now RUN your vprofile-continuous-delivery pipeline 
  
<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

### :package: Add windows node as slave to Jenkins

- Let's launch our windows server instance 
- On your console under `EC2`-> `Instances` click `launch instances`.
- We will create Windows-server with below details.

```sh
Name: Windows-server
AMI:   server 2019 base
SecGrp: Windows-Server-SofTest
InstanceType: t2.small
Userdata: use the userdata given below
KeyPair: ci-vprofile-key

   ```
   
```sh
<powershell>
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
choco install jdk8 -y 
choco install maven -y 
choco install googlechrome -y
choco install git -y
mkdir C:\jenkins
</powershell>

   ```
   
- Add a rule to jenkins secuirty group to allow all traffic from windows server security group
- On the AWS console. select your Windows server then click `connect` -> `RDP Client`- > `Get password`. Now upload your private key of ci-vprofile-key
- Click on decrypt password to get your password. Store the password and the public IP of your windows instance
- Now RDP into your intances using th public IP of the windows server. Enter your credentials to login into the server 

- Open the browser on your windows server and enter `jenkins public IP` on your url  on port 8080. Login into your Jenkins server using your credential. 
- On your jenkins dashboard goto `Manage jenkins`-> `configure system` and give the details below


```sh
Scroll down to Jenkins location
Jenkins URL: http://<JNKINS PRIVATE IP>/8080/
Save changes
   ```
   
- On your jenkins dasboard goto `Manage jenkins`-> `Manage Nodes and Clouds` and give the details below


```sh
Clic add new node
Name: vprofile-softwareTest
select parmanent agent 
click ok
Remote root directory: c/jenkins (please create this directory on your windows server c drive before adding it here)
launch method: launch agent by connecting it to the master 
custom work directory: c/jenkins
select use socket 
Save changes
   ```

- On your jenkins goto `Manage jenkins`-> `configure global security` and give the details below


```sh
scroll down to Agent
TCP port for inbound agent: select fixed and enter 8090
Save changes
   ```
   
- On your jenkins goto `Manage jenkins`-> `Manage Nodes and Clouds` and copy the entire  `run agent from command line ....` command and goto your CMD and run the command.
- Now our windows server is connected as a slave.


<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>
 
### :package: Create Job to run Software Tests
  
- On your jenkins goto `New Item` create a job and give the details below

```sh
Item name: Selenium-Test-Suits
click freestyle
copy it fro the Build job 
Under general, select retrict where this project can run the put vprofile_softwareTest-Node
Scroll down and copy it from Build Job that was created earlier.
Change the branch to */seleniumsuite
Add build step
select execute windows batch command 
Command: enter the following command make sure you replace the url in the command with tomcat public ip
  
  
echo package DevOPS.devOPS; > src\test\java\DevOPS\devOPS\Variables.java
echo public class Variables { >> src\test\java\DevOPS\devOPS\Variables.java
echo 	public static final String URL ="http://<tomcat public IP>:8080/vproapp/login"; >> src\test\java\DevOPS\devOPS\Variables.java
echo 	public static final String UserName ="admin_vp"; >> src\test\java\DevOPS\devOPS\Variables.java
echo 	public static final String Password ="admin_vp"; >> src\test\java\DevOPS\devOPS\Variables.java
echo 	public static final String ScreenShotPath ="C:\\jenkins\\screenshots"; >> src\test\java\DevOPS\devOPS\Variables.java
echo }>> src\test\java\DevOPS\devOPS\Variables.java

Goals: clean, test -Dsurefir.suiteXmlFiles=testing.xml 
  
Revove the execute shell script
Remove the archive artifact 
remove build other project 
Invoke top level maven target 
Save changes
   ```
  
  
- RUN the job. This job will automatically login to our windows server, take the screenshot of the login page of our app and store it in jenkins server and windows server as well .

  
- Connec the job to our pipeline   
- On your Jenkins open the `TestURL` job click on `configure` and make the following changes.


```sh
Scroll down to post build action
Add post buil action
select trigger parameterze build on other projects 
project to build : Selenium-Test-Suite  

SAVE CHANGES
   ```
Run the pipeline job 
<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

### :package: Deploy artifact to Production tomcat server
  
  
- On your console under EC2->Instances click launch instances.
- We will create `production` server with below details.


```sh
Name:   Production server
AMI: Ubuntu 20.04
SecGrp: vprofile-app-staging-sg
InstanceType: t2.small
Tga: app01-staging-vprofile
KeyPair: ci-vprofile-key

   ```
- On your jenkins goto `New Item` create a job and give the details below

```sh
Item name: Deploy-to-Nexus-for-Prod
click freestyle
Scroll down and copy it from Deploy-to-Nexus Job that was created earlier.
  Add build step
  select invoke top level maven targets 
  Goal: install
  properties 
  Add aother build step
  select execute shell
  commands: enter the command below 
  
  
  cat << EOT > src/main/resouces/application.properties

#JDBC Configutation for Database Connection
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://db01:3306/accounts?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull
jdbc.username=admin
jdbc.password=admin123

#Memcached Configuration For Active and StandBy Host
#For Active Host
memcached.active.host=mc01
memcached.active.port=11211
#For StandBy Host
memcached.standBy.host=127.0.0.2
memcached.standBy.port=11211

#RabbitMq Configuration
rabbitmq.address=rmq01
rabbitmq.port=5672
rabbitmq.username=test
rabbitmq.password=test

#Elasticesearch Configuration
elasticsearch.host =192.168.1.85
elasticsearch.port =9300
elasticsearch.cluster=vprofile
elasticsearch.node=vprofilenode
EOT
  
   
Save changes
   ```
  
  

<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

### :package: Connect all the jobs with Build Pipeline
  
  
- Connect Job to the pipeline
- On your Jenkins open the `Selenium-Test-Suite` job click on `configure` and make the following changes.


```sh
Scroll down to post build action
Add post buil action
select build other projects manual step
project to build : Deploy-to-Nexus-Prod
Add parameters 
TIME= $BUILD_TIMEstamp
ID=$BUILD_ID
SAVE CHANGES
   ```

- Run the job, now sign into your nexus server you will see the production artifact ready 

<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

### :package: Test it by committing code to github

<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>

## :earth_africa: Verify from browser

<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>


## :page_facing_up: Resources

<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>


## :star2: Acknowledgment


<br/>
<div align="right">
    <b><a href="#Project-08">↥ back to top</a></b>
</div>
<br/>
