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
![ECS Vs EKS](https://github.com/user-attachments/assets/f16e4229-7045-4455-922b-58661de2b98c)
![Service Vs Ingress](https://github.com/user-attachments/assets/05886306-4219-4d16-8dad-007a9d0efe96)
1. Liveness Probe
Purpose: Checks if the container is still running and responsive.
Behavior:
If the probe fails, Kubernetes restarts the container.
Useful for detecting and recovering from application deadlocks or crashes.
2. Readiness Probe
Purpose: Checks if the container is ready to accept traffic.
Behavior:
If the probe fails, the container is removed from the Service's load balancer.
Useful for ensuring the application is fully initialized and ready to handle requests.
3. Startup Probe
Purpose: Ensures that the application starts successfully.
Behavior:
If the probe fails, the container is killed and restarted.
Unlike liveness probes, it prevents premature restarts during application startup.
Useful for applications with long initialization times.
![Common Parameter in Probes](https://github.com/user-attachments/assets/86f3e63f-de00-440a-8bc4-7e71f2f8351e)
1. StatefulSet
Purpose: Manages stateful applications that require unique, persistent identities and storage for each Pod.
Key Features:
Stable Network Identity: Each Pod has a unique, consistent hostname (e.g., pod-name-0, pod-name-1).
Stable Storage: Each Pod can have its own persistent volume, ensuring data consistency even after Pod restarts.
Ordered Deployment and Scaling:
Pods are created, updated, and terminated in a specific order (0 → n).
Use Case: Suitable for applications like databases (e.g., MySQL, Cassandra) or distributed systems where Pod identity and storage are critical.
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-statefulset
spec:
  serviceName: "my-service"
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image
          volumeMounts:
            - name: my-volume
              mountPath: /data
  volumeClaimTemplates:
    - metadata:
        name: my-volume
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
2. DaemonSet
Purpose: Ensures that a copy of a specific Pod runs on every (or a subset of) node(s) in the cluster.
Key Features:
One Pod per Node: Automatically schedules Pods on all eligible nodes.
Dynamic Updates: Automatically adds Pods to new nodes that are added to the cluster.
Use Case: Ideal for node-level operations, such as logging, monitoring, or networking agents (e.g., Fluentd, Prometheus Node Exporter
A Helm chart is a collection of files that describe a related set of Kubernetes resources. The chart's structure is organized in a specific directory layout. Here's the standard structure of a Helm chart and the purpose of each file or folder:

my-helm-chart/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── _helpers.tpl
│   └── NOTES.txt
├── charts/
├── README.md
├── .helmignore
2. Explanation of Files and Folders
1. Chart.yaml
Purpose: Metadata about the Helm chart.
Content:
apiVersion: v2           # Helm chart API version (v2 for Helm 3).
name: my-helm-chart      # Name of the chart.
version: 1.0.0           # Version of the chart.
description: A sample Helm chart for Kubernetes
appVersion: "1.16.0"     # Version of the application being packaged.
Importance: Required for every Helm chart. Used to identify the chart and its version.2. values.yaml
Purpose: Default configuration values for the chart.
Content:
replicaCount: 3
image:
  repository: nginx
  tag: "1.21.6"
  pullPolicy: IfNotPresent
service:
  type: ClusterIP
  port: 80
Importance: Users can override these values during chart installation with a custom values.yaml file or --set flags.
3. templates/
Purpose: Contains Kubernetes manifest templates for resources such as Deployments, Services, and Ingress.
Common Files:
deployment.yaml: Defines the Pod and ReplicaSet configuration.
service.yaml: Configures a Kubernetes Service.
ingress.yaml: Configures an Ingress resource.
_helpers.tpl: Defines reusable template helpers (e.g., naming conventions).
NOTES.txt: Contains user instructions displayed after installing the chart.
Example (deployment.yaml) :
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-app
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
    spec:
      containers:
        - name: app
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 80
   Importance: Contains the core logic for Kubernetes resources, making the chart reusable and dynamic.
   4. charts/
Purpose: Stores dependencies (other charts) required by this chart.
Example:
If your chart depends on another chart (e.g., a database chart), it’s placed here.
Dependencies can also be defined in Chart.yaml under the dependencies section.
Example in Chart.yaml:
dependencies:
  - name: mysql
    version: 8.0.0
    repository: https://charts.bitnami.com/bitnami
5. README.md
Purpose: Documentation for the chart.
Content:
Description of the chart.
Instructions on how to use it.
Examples of values.yaml overrides.
6. .helmignore
Purpose: Specifies files and directories to ignore when packaging the chart.
Content:
.DS_Store
.git/
.gitignore
*.bak
3. Installing a Helm Chart
Command to install:
helm install <release-name> ./my-helm-chart
Override values:
helm install <release-name> ./my-helm-chart -f custom-values.yaml
############################################################################
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  terraformVersion: '1.5.0'
  awsRegion: 'eu-west-1'
  tfVarsFile: 'non-prod.tfvars'

steps:
# Step 1: Install Terraform
- task: UseTerraform@0
  inputs:
    terraformVersion: $(terraformVersion)

# Step 2: AWS Authentication
- task: AWSCLI@1
  inputs:
    awsServiceConnection: '<your-service-connection-name>'
    regionName: $(awsRegion)

# Step 3: Initialize Terraform
- script: |
    terraform init
  displayName: 'Initialize Terraform'

# Step 4: Validate Terraform
- script: |
    terraform validate
  displayName: 'Validate Terraform'

# Step 5: Plan Terraform
- script: |
    terraform plan -out=tfplan -var-file=$(tfVarsFile)
  displayName: 'Plan Terraform'

# Step 6: Apply Terraform
- script: |
    terraform apply -auto-approve tfplan
  displayName: 'Apply Terraform'
  
=============================================

AWS Service Connection:

Replace <your-service-connection-name> with the name of your AWS service connection configured in Azure DevOps.
Terraform Variables File:

Ensure non-prod.tfvars contains all required variables for your Terraform modules.
AWS Region:

The pipeline uses eu-west-1 as the AWS region. Update this if your resources are in a different region.
Terraform Commands:

terraform init: Initializes Terraform.
terraform validate: Validates the configuration.
terraform plan: Generates an execution plan.
terraform apply: Applies the plan to deploy resources.

Steps to Connect Azure DevOps to AWS
1. Create an AWS IAM User
Log in to your AWS Management Console.
Go to IAM > Users > Add User.
Create a user with programmatic access (access key and secret key).
Attach the necessary policies (e.g., AdministratorAccess or a custom policy with permissions for Terraform resources).
Save the Access Key ID and Secret Access Key.

2. Configure Azure DevOps Service Connection
Log in to your Azure DevOps organization.
Go to Project Settings > Service Connections > New Service Connection.
Select AWS as the service connection type.
Enter the Access Key ID and Secret Access Key from the IAM user you created.
Test the connection and save it.
