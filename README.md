# Spring Boot Shopping Cart Web App

## About

This is a demo project for practicing Spring + Thymeleaf. The idea was to build some basic shopping cart web app.

It was made using **Spring Boot**, **Spring Security**, **Thymeleaf**, **Spring Data JPA**, **Spring Data REST and Docker**. 
Database is in memory **H2**.

There is a login and registration functionality included.

Users can shop for products. Each user has his own shopping cart (session functionality).
Checkout is transactional.

## Configuration

### Configuration Files

Folder **src/resources/** contains config files for **shopping-cart** Spring Boot application.

* **src/resources/application.properties** - main configuration file. Here it is possible to change admin username/password,
as well as change the port number.

## How to run

There are several ways to run the application. You can run it from the command line with included Maven Wrapper, Maven or Docker. 

Once the app starts, go to the web browser and visit `http://localhost:8070/home`

Admin username: **admin**

Admin password: **admin**

User username: **user**

User password: **password**

### Maven Wrapper

#### Using the Maven Plugin

Go to the root folder of the application and type:
```bash
$ chmod +x scripts/mvnw
$ scripts/mvnw spring-boot:run
```

#### Using Executable Jar

Or you can build the JAR file with 
```bash
$ scripts/mvnw clean package
``` 

Then you can run the JAR file:
```bash
$ java -jar target/shopping-cart-0.0.1-SNAPSHOT.jar
```

### Maven

Open a terminal and run the following commands to ensure that you have valid versions of Java and Maven installed:

```bash
$ java -version
java version "1.8.0_102"
Java(TM) SE Runtime Environment (build 1.8.0_102-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.102-b14, mixed mode)
```

```bash
$ mvn -v
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-10T16:41:47+00:00)
Maven home: /usr/local/Cellar/maven/3.3.9/libexec
Java version: 1.8.0_102, vendor: Oracle Corporation
```

#### Using the Maven Plugin

The Spring Boot Maven plugin includes a run goal that can be used to quickly compile and run your application. 
Applications run in an exploded form, as they do in your IDE. 
The following example shows a typical Maven command to run a Spring Boot application:
 
```bash
$ mvn spring-boot:run
``` 

#### Using Executable Jar

To create an executable jar run:

```bash
$ mvn clean package
``` 

To run that application, use the java -jar command, as follows:

```bash
$ java -jar target/shopping-cart-0.0.1-SNAPSHOT.jar
```

To exit the application, press **ctrl-c**.

### Docker

It is possible to run **shopping-cart** using Docker:

Build Docker image:
```bash
$ mvn clean package
$ docker build -t shopping-cart:dev -f docker/Dockerfile .
```

Run Docker container:
```bash
$ docker run --rm -i -p 8070:8070 \
      --name shopping-cart \
      shopping-cart:dev
```

##### Helper script

It is possible to run all of the above with helper script:

```bash
$ chmod +x scripts/run_docker.sh
$ scripts/run_docker.sh
```

## Docker 

Folder **docker** contains:

* **docker/shopping-cart/Dockerfile** - Docker build file for executing shopping-cart Docker image. 
Instructions to build artifacts, copy build artifacts to docker image and then run app on proper port with proper configuration file.

## Util Scripts

* **scripts/run_docker.sh.sh** - util script for running shopping-cart Docker container using **docker/Dockerfile**

## Tests

Tests can be run by executing following command from the root of the project:

```bash
$ mvn test
```

## Helper Tools

### HAL REST Browser

Go to the web browser and visit `http://localhost:8070/`

You will need to be authenticated to be able to see this page.

### H2 Database web interface

Go to the web browser and visit `http://localhost:8070/h2-console`

In field **JDBC URL** put 
```
jdbc:h2:mem:shopping_cart_db
```

In `/src/main/resources/application.properties` file it is possible to change both
web interface url path, as well as the datasource url.




--------------- -------------------------------------  Notes---------------------------------------------------------


1.Create three instance(jenkins,nexus and sonar qube)
--
Installing jenkins on one server---
a)install java first 

"apt install openjdk-17-jre-headless"

b)Install jenkins and open the port 8080
--


""" sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
    echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins       """"


2.Now install sonarqube.
--

a)insatlling docker first
--

"  sudo apt install docker.io"

"sudo usermod -aG docker $USER && newgrp docker" or "sudo chmod 777 /var/run/docker.sock"

b) "  docker run -d -p 9000:9000 sonarqube:lts-community "


3.installing nexus
--

a)install the docker first.
b)install nexus with the help of docker image " docker run -d -p 8081:8081 sonatype/nexus3"

4.Install plugins inside the jenkins
--

a)OWASP Dependency-Check
b)SonarQube Scanner
c)DOCKER
d)Nexus Artifact Uploader
e)Eclipse Temurin installer
f)config file provider(for nexus steup)

5)go to the tool tabs in jenkins and do all the setting releated to the tools which one we have insatlled.
--
6)setup cred in jenkins and also done sonar setup with jenkins.
7)setup the nexus --go to the plugin in jenkins and installed the config file provider and after the you can see the managed files in managed jenkins then provide the maven-snapshots and maven-release inside the server section.
--

7)Kubeadm installation for master node
--
  1  clear
    2  sudo apt update
    3  sudo apt upgrade
    4  sudo apt-get update
    5  sudo swapoff -a 
    6  sudo sed -i '/ swap / s/^/#/' /etc/fstab
    7  # Install Docker
    8  sudo apt update
    9  sudo apt install -y docker.io
   10  sudo systemctl enable docker
   11  sudo systemctl start docker
   12  # Verify Docker
   13  docker --version
   14  docker ps
   15  sudo chmod 666 /var/run/docker.sock
   16  docker ps
   17  sudo apt update
   18  # Add Kubernetes repository (use the new official URL)
   19  sudo apt update
   20  sudo apt install -y apt-transport-https ca-certificates curl
   21  curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
   22  echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
   23  # Install tools
   24  sudo apt update
   25  sudo apt install -y kubelet kubeadm kubectl
   26  sudo apt-mark hold kubelet kubeadm kubectl  # Prevent auto-updates
   27  # Verify
   28  kubeadm version
   29  kuectl version
   30  kubectl version
   31  kubelet version
   32  kubelet --version
   33  kubeadm version
   34  cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

   35  cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

   36  sudo sysctl --system
   37  sudo kubeadm init
   38  history
   kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/calico.yaml

# Get the join command (SAVE THIS OUTPUT)
kubeadm token create --print-join-command

   9).Kubeadm installation for worker node
   --
   chandanchoudhary7315@worker-node:~$ history
    1  sudo apt-get update && upgrade
    2  sudo apt-get upgrade
    3  sudo swapoff -a                         # Disable swap temporarily
    4  sudo sed -i '/ swap / s/^/#/' /etc/fstab # Disable permanently
    5  # Install Docker
    6  sudo apt update
    7  sudo apt install -y docker.io
    8  sudo systemctl enable docker
    9  sudo systemctl start docker
   10  # Verify Docker
   11  docker --version
   12  docker ps
   13  sudo chmod 666 /var/run/docker.sock
   14  docker ps
   15  # Add Kubernetes repository (use the new official URL)
   16  sudo apt update
   17  sudo apt install -y apt-transport-https ca-certificates curl
   18  curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
   19  echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
   20  # Install tools
   21  sudo apt update
   22  sudo apt install -y kubelet kubeadm kubectl
   23  sudo apt-mark hold kubelet kubeadm kubectl  # Prevent auto-updates
   24  # Verify
   25  kubeadm version
   26  cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

   27  cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

   28  sudo sysctl --system
   29  kubeadm join 10.154.0.2:6443 --token s5scan.09ye12pw39pkqsn5 --discovery-token-ca-cert-hash sha256:9336fe0a8f1a197f56a821b13bfa8dfc849e3a1f412f713d2b6267eaba1ae571
   30  sudo kubeadm join 10.154.0.2:6443 --token s5scan.09ye12pw39pkqsn5 --discovery-token-ca-cert-hash sha256:9336fe0a8f1a197f56a821b13bfa8dfc849e3a1f412f713d2b6267eaba1ae571
   31  history



