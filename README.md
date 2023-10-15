# Maven Installation process (Linux):
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


   # Apache Tomcat Installation process (Linux):
   [ https://tomcat.apache.org/download-90.cgi] - install tomcat url
    1  cd /opt
    2  sudo yum install java-17-amazon-corretto-devel -y
    3  sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.80/bin/apache-tomcat-9.0.80.tar.gz
    4  sudo tar xf apache-tomcat-9.0.80.tar.gz 
    5  sudo rm apache-tomcat-9.0.80.tar.gz 
    6  sudo mv apache-tomcat-9.0.80/ tomcat9
    7  sudo chown -R ec2-user:ec2-user tomcat9
    8  ls -lrt
    9  /opt/tomcat9/bin/startup.sh
   10  /opt/tomcat9/bin/shutdown.sh
   11  /opt/tomcat9/conf/server.xml - change port number

   # Jenkins Installation process (Linux):
   [https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/amazon-linux-install.html] - Install java-17 url
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
   11 Installing suggested plugins - username,password,confirmpwd,fullname,email - enter - save and finish - start
   

   
