# # Install & Setup Apache Maven on EC2 Instance(Linux): 
    Launch Ec2  instance
    1  sudo yum install maven -y
    2  mvn --version
    3  mvn archetype:generate -DgroupId=com.icici -DartifactId=project -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
    4  ls
    5  cd project/
    6  sudo yum install git -y
    7  git --version
    8  git init - after that create repository existing name (project)
    9  git remote add origin (repo URL)
   10  git branch -M main
   11  git status
   12  git add pom.xml 
   13  git add src/
   14  git commit -m "mvn"
   15  git config --global credential.helper store
   16  git push origin main - github(username - password or Token)
   17  mvn clean package - we got plugin error and reslove to add plugin in pom.xml
   18  sudo vi pom.xml - 
     <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.3.1</version>
      </plugin>
     </plugins>
   19  git status
   20  git add pom.xml 
   21  git commit -m "package"
   22  git push origin main
   23  git status
   24  mvn clean package# installation_process

# # Install & Setup Apache Tomact9 on EC2 Instance (Linux): 
     https://tomcat.apache.org/download-90.cgi - install tomcat
    Launch Ec2 Instance 
    1  cd /opt
    2  sudo yum install java-17-amazon-corretto-devel -y
    3  sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.80/bin/apache-tomcat-9.0.80.tar.gz
    4  sudo tar xf apache-tomcat-9.0.80.tar.gz 
    5  sudo rm -rf apache-tomcat-9.0.80.tar.gz 
    6  sudo mv apache-tomcat-9.0.80/ tomcat9
    7  sudo chown -R ec2-user:ec2-user tomcat9
    8  ls -lrt
    9  /opt/tomcat9/bin/startup.sh
   10  /opt/tomcat9/bin/shutdown.sh
   11  /opt/tomcat9/conf/server.xml - change port number

# # Install & Setup Jenkins on EC2 Instance(Linux): 
   Install java-17 - https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/amazon-linux-install.html 
   Launch Ec2 instance
   1 Go to - https://www.jenkins.io/ - Download - Redhat/Fedora/Alma/Rocky/CentOs
   2 sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   3 sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
   4 sudo yum install java-17-amazon-corretto-devel -y
   5 sudo yum install jenkins -y
   6 sudo systemctl start jenkins 
   7 sudo systemctl enable jenkins
   8 sudo systemctl status jenkins 
   9 take Publicip current instance - hit browser and default port number(publicip:8080)
   10 sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   11 Install suggested plugins - username,password,confirmpwd,fullname,email - enter - save and finish - start
   
# jenkins default port number change 
    1 cd /usr/lib/systemd/system
    2 sudo vi jenkins.service
    3 Environment="JENKINS_PORT=8383"
    4 sudo systemctl restart jenkins
    5 sudo systemctl daemon-reload 

#  # Install & Setup SonarQube on EC2 Instance (Linux):
    https://www.sonarsource.com/products/sonarqube/downloads/ - install sonarqube community edition
    Launch EC2, it requires minimum of 4GB memory and 2 CPU
    1  cd /opt
    2  sudo yum install java-17-amazon-corretto-devel -y
    3  sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.1.0.73491.zip
    4  sudo unzip sonarqube-10.1.0.73491.zip 
    5  sudo mv sonarqube-10.1.0.73491/ sonar10
    6  sudo chown -R ec2-user:ec2-user sonar10/
    7  cd 
    8  /opt/sonar10/bin/linux-x86-64/sonar.sh start
    9  /opt/sonar10/bin/linux-x86-64/sonar.sh status
   10  sudo vi /opt/sonar10/conf/sonar.properties

# # Install & Setup Ansible on EC2 Instance (Linux):
    1  sudo yum update -y
    2  sudo amazon-linux-extras install ansible2 -y
    3  ansible --version
    4  sudo vi /etc/ansible/hosts - PrivateIp ansible_user=ec2-user ansible_ssh_private_key_file=~/ansible.pem
    5  pwd
    6  sudo vi ansible.pem
    7  chmod 400 ansible.pem
    8  ansible -m ping 172.31.86.146 (apache privateip)
    9  cat /etc/ansible/hosts
   10  ansible 172.31.86.146 -m yum -a 'name=git state=present' - install git 
   11  ansible 172.31.86.146 -m yum -a 'name=git state=present' --become 

 # Install & Setup Nexus on EC2 Instance
  1  Launch EC2 Instance
  2 It will not work on t2.micro( which offers 1 CPU and 1GB Memory)
  3 Minimum of 2 CPU and 4GB memory is required
  4 Launch t3.medium instance
  5 Install java 17 on this machine
  6 sudo yum install java-1.8.0-openjdk-devel -y
  7 cd /opt
  8 sudo wget https://download.sonatype.com/nexus/3/nexus-3.49.0-02-unix.tar.gz
  9 sudo tar xf nexus-3.49.0-02-unix.tar.gz
  10 sudo mv nexus-3.49.0-02/ nexus3
  11 sudo chown -R ec2-user:ec2-user nexus3/ sonatype-work/
  12  cd
  13 /opt/nexus3/bin/linux_x86/nexus start
  14 /opt/nexus3/bin/linux_x86/nexus status 
