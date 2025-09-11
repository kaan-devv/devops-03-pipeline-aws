###### ====================================== !!!ATTENTION!!!  ==============================================

This Project was created by Mimar Aslan and contains valuable information I personally gained from the DevOps course he provided.


# DevOps Pipeline

## CI/CD Universe

CI/CD: (Jenkins, Git, GitHub, GitOps, GitHub Actions, GitLab, GitLab CI, Bitbucket, Bamboo)
Scripting (Python, Bash, PowerShell)
Containers: (Docker)
Orchestration: (Kubernetes, Helm)
Cloud (AWS, Azure, GCP)
Virtualization: (VMware, VirtualBox)
IaC: (Terraform, Ansible, CloudFormation)
Monitoring: (Prometheus, Grafana, ELK)

objectivec

<hr>

AWS CLI will be installed.

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

aws --version

shell

#### MacOS

ls -la ~/
mv ~/Downloads/MyAWSKeyPair.pem ~/.ssh/
chmod 400 ~/.ssh/MyAWSKeyPair.pem

nano ~/.ssh/config

mathematica

Host MyDevOpsAWS  
HostName PUBLIC_IP  
User ubuntu  
IdentityFile ~/.ssh/MyAWSKeyPair.pem  

Ctrl + O  
Enter  
Ctrl + X  

ssh MyAWSKeyPair

yaml

---

## === Machine 1: My Jenkins Master ============================

Windows  
We will create a Session -> SSH via MobaXterm.

Run these 2 commands in the terminal sequentially.

sudo apt update
sudo apt upgrade -y

yaml

---

We will assign a name instead of the internal IP.

sudo nano /etc/hostname

pgsql

We wrote the following as the name:

My-Jenkins-Master  

Press Ctrl + X.  
Press Y to confirm.  
Finally, press Enter.  

or  

Press Ctrl + O.  
Finally, press Enter.  

Restart the machine.

sudo init 6

nginx
or
sudo reboot

yaml

---

AWS EC2 machine was opened to the outside world.  
We went to the Security groups section.  
We allowed external access from port 8080.

---

======= We will install Java. ========================

Type Java into the terminal and press Enter. Take one of the commands that appear and run it.

sudo apt install openjdk-21-jre -y
java --version

yaml

---

======= We will install Jenkins. ========================

https://www.jenkins.io/doc/book/installing/linux/

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc
https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]"
https://pkg.jenkins.io/debian binary/ | sudo tee
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y

pgsql

Run the following commands sequentially.  
We dedicate this machine to Jenkins.  
When we shut down and restart the machine, Jenkins will automatically be running.

sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins

pgsql

I did not close this terminal, I only exited that state. My terminal is open.  
Ctrl + C  

Type this command in my terminal and we learned Jenkins' admin password.

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

yaml

---

## === Machine 2: My Jenkins Agent ============================

This machine is dedicated to Docker.

Windows  
We will create a Session -> SSH via MobaXterm.

Run these 2 commands in the terminal sequentially.

sudo apt update
sudo apt upgrade -y

yaml

---

We will assign a name instead of the internal IP.

sudo nano /etc/hostname

pgsql

We wrote the following as the name:

My-Jenkins-Agent  

Press Ctrl + X.  
Press Y to confirm.  
Finally, press Enter.  

Restart the machine.

sudo init 6

nginx
or
sudo reboot

yaml

---

======= We will install Java. ========================

Type Java into the terminal and press Enter. Take one of the commands that appear and run it.

sudo apt install openjdk-21-jre -y
java --version
java -version

yaml

---

===== We will install Docker. ==========================

Come to the terminal, just type docker and press Enter.

sudo apt install docker.io -y
sudo usermod -aG docker $USER
sudo reboot

yaml

---

We will connect the machines to each other.

=== For My Jenkins Master ============================

sudo nano /etc/ssh/sshd_config

yaml

Go to the Authentication section.  
Remove the comment sign `#` in front of the following two lines.

---

### Authentication:

PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2

vbnet

Press Ctrl + X.  
Press Y to confirm.  
Finally, press Enter.  

sudo service sshd reload

yaml

---

=== For My Jenkins Agent ============================

sudo nano /etc/ssh/sshd_config

yaml

Go to the Authentication section.  
Remove the comment sign `#` in front of the following two lines.

---

### Authentication:

PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2

vbnet

Press Ctrl + X.  
Press Y to confirm.  
Finally, press Enter.  

sudo service sshd reload

yaml

---

=== For My Jenkins Master ============================

pwd
cd /home/ubuntu

css

I am creating a password key for the master machine to be tracked.

ssh-keygen
cd /home/ubuntu/.ssh/
ll
sudo cat id_ed25519.pub

scss

Take and copy the line written inside.

ssh-ed25519 AAAAAAAAAAAAAAAAA ubuntu@My-Jenkins-Master

yaml

Press Enter until the end.

---

=== For My Jenkins Agent ============================

cd /home/ubuntu/.ssh/
ll
sudo cat authorized_keys

kotlin

Open this file.

sudo nano authorized_keys

yaml

Paste the following line copied from Master at the very bottom.

ssh-ed25519 AAAAAAAAAAAAAAAAA ubuntu@My-Jenkins-Master  

---

==== The side following the Agent ====

ssh-rsa BBBBBBBBBBBBBBBBBB MyAWSKeyPair  

---

==== The keygen key from Master to be followed ====

ssh-ed25519 AAAAAAAAAAAAAAAAA ubuntu@My-Jenkins-Master  

Press Ctrl + X.  
Press Y to confirm.  
Finally, press Enter.  

cd /home/ubuntu/.ssh/
sudo cat authorized_keys

yaml

---

Restart both Master and Agent machines.

sudo reboot

yaml

---

======================================  
http://PUBLIC_IP:8080/computer/(built-in)/configure  

Open Jenkins.  
Go to the Nodes section.  

Enter the Built-In Node machine.  

Nodes -> Built-In Node -> Configure  

Set the Number of executors to ZERO 0.  

To add the Agent machine to Jenkins  

Nodes -> New node  

http://PUBLIC_IP:8080/computer/new  

We gave it the name "My-Jenkins-Agent".  
We selected it as a Permanent Agent.  

When adding the Agent in Jenkins, you will take its own internal IP.  

---

====== Read and copy this key from the Master Machine and come to Jenkins. For Credentials ====  

cd /home/ubuntu/.ssh/
sudo cat id_ed25519

yaml

-----BEGIN OPENSSH PRIVATE KEY-----  
CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC  
-----END OPENSSH PRIVATE KEY-----  

---

We will create a GitHub Token.  

https://github.com/settings/tokens  

MyGitHubTokenForAWS  

ghp_ABCABCABCABCABCABCABCABCABCABC  

---

## === Machine 3: SonarQube Installation ==========

Windows  
We will create a Session -> SSH via MobaXterm.

Run these 2 commands in the terminal sequentially.

sudo apt update
sudo apt upgrade -y

yaml

---

We will assign a name instead of the internal IP.

sudo nano /etc/hostname

yaml

We wrote the following as the name:

My-SonarQube  

---

### HOMEWORK: Find a way to change the hostname with a single command.

sudo hostname My-SonarQube

yaml

---

Press Ctrl + X.  
Press Y to confirm.  
Finally, press Enter.  

Restart the machine.

sudo reboot

yaml

---

==== PostgreSQL Installation =====

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null

sudo apt update
sudo apt-get -y install postgresql postgresql-contrib

sudo systemctl enable postgresql
sudo systemctl status postgresql

sudo passwd postgres

yaml

password: 123456789  

---

=== I want to log into the database terminal as the top authority.

su - postgres

makefile

password: 123456789  

createuser sonar
psql
ALTER USER sonar WITH ENCRYPTED password 'sonar';
CREATE DATABASE sonarqube OWNER sonar;
grant all privileges on DATABASE sonarqube to sonar;
\q
exit

yaml

---

==== Adoptium repository ====

sudo bash
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list

sudo apt update
sudo apt install temurin-17-jdk -y

pgsql

The following command also does the same job.

sudo apt-get install temurin-17-jdk -y

yaml

---

JDK already exists. We will install JRE just as an example.

sudo apt install openjdk-17-jre -y

yaml

---

To select alternative versions of Java:

sudo update-alternatives --config java
java --version

yaml

---

### === We will increase Linux kernel limits. ===

---

== Vim and Nano are for writing inside files from the terminal.

sudo vim /etc/security/limits.conf

yaml

To add something, first press the i key on the keyboard.

---

== It is easier to work with Nano.

sudo nano /etc/security/limits.conf

yaml

---

== Add the following two lines at the very bottom of the file.

sonarqube - nofile 65536
sonarqube - nproc 4096

vbnet

To exit, press the ESC key.  
Type `:wq` and press Enter.  

sudo vim /etc/sysctl.conf

vbnet

To add something, first press the i key.  
The line to be added is:

vm.max_map_count = 262144

pgsql

To exit, press the ESC key.  
Type `:wq` and press Enter.  

Restart the machine.

sudo reboot

yaml

---

### ==== SonarQube Installation =====

pwd
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.9.0.112764.zip

kotlin

== To unzip a compressed file in Ubuntu, I installed this application.

sudo apt install unzip
sudo unzip sonarqube-25.9.0.112764.zip -d/opt
pwd
sudo mv /opt/sonarqube-25.9.0.112764 /opt/sonarqube

yaml

---

=== A sonar user will be created and permissions will be granted

sudo groupadd sonar
sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar
sudo chown sonar:sonar /opt/sonarqube -R

yaml

---

=== Connect this sonar user with the database

sudo vim /opt/sonarqube/conf/sonar.properties

sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube

yaml

---

=== We will create the Sonar service.

sudo vim /etc/systemd/system/sonar.service

csharp

Paste the following codes exactly into this file.

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

less

Commands to automatically run SonarQube when the machine starts:

sudo systemctl enable sonar
sudo systemctl start sonar
sudo systemctl status sonar

yaml

---

=== Log tracking ===

sudo tail -f /opt/sonarqube/logs/sonar.log

vbnet

Take the public IP value of the machine and log in via port 9000.

username: admin
password: admin

yaml

I gave a new password.  

Adana_01Adana_01  

---

Create a token for Jenkins.  

Administrator -> Security  

http://SONARQUBE_MACHINE_PUBLIC_IP:9000/account/security  

jenkins-sonarqube-token  

sqa_EEEEEEEEEEEEEEEEEEEEEEEEEEE  

---

Save `jenkins-sonarqube-token` as Security Text inside Jenkins.  

Install SonarQube plugins.  

Go into the System side and configure the SonarQube settings.  

Introduce Sonar Scanner as a Tool.  

Copy the Private IPv4 address value of the machine where Sonar is installed.  

http://JENKINS_MASTER_MACHINE_PUBLIC_IP:8080/
