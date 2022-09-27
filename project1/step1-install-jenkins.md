## Step 1. Install Jenkins (server) with plugins which will be used

For more : https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

- ````curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null```

  ````

- ````echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null```

  ````

- ````sudo apt-get update
      sudo apt-get install jenkins```
  ````

### Installation of Java

Jenkins requires Java in order to run, yet certain distributions don’t include this by default and some Java versions are incompatible with Jenkins.

- `sudo apt update`
- `sudo apt install openjdk-11-jre`
- `java -version`

\_\_Start Jenkins
You can enable the Jenkins service to start at boot with the command:

sudo systemctl enable jenkins
You can start the Jenkins service with the command:

sudo systemctl start jenkins
You can check the status of the Jenkins service using the command:

sudo systemctl status jenkins\_\_

### Install required plugin in Jenkins

#### Install Git

- `sudo apt update`
- `sudo apt install git`
- `git --version`

#### Install & configure Maven build tool on Jenkins

Maven is a code build tool which used to convert your code to an artifact. this is a widely used plugin to build in continuous integration

#### Prerequisites

1. Jenkins server

#### Install Maven on Jenkins

1. Download maven packages https://maven.apache.org/download.cgi onto Jenkins server. In this case, I am using /opt/maven as my installation directory

- Link : https://maven.apache.org/download.cgi
  ```sh
   # Creating maven directory under /opt
   mkdir /opt/maven
   cd /opt/maven
   # downloading maven version 3.6.0
   wget http://mirrors.estointernet.in/apache/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz
   tar -xvzf apache-maven-3.6.1-bin.tar.gz
  ```

1. Setup M2_HOME and M2 paths in .bash_profile of the user and add these to the path variable
   ```sh
   vi ~/.bash_profile
   M2_HOME=/opt/maven/apache-maven-3.6.1
   M2=$M2_HOME/bin
   PATH=<Existing_PATH>:$M2_HOME:$M2
   ```

#### Checkpoint

1.  logoff and login to check maven version
    `sh mvn --version `
    So far we have completed the installation of maven software to support maven plugin on the jenkins console. Let's jump onto Jenkins to complete the remaining steps.

### Setup maven on Jenkins console and other plugins will be used

1. Install maven plugin without restart

- `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`
- `Manage Jenkins` > `Jenkins Plugins` > `available`
  >
  - `Maven Integration`
  - `Publish Over SSH` will be used to connect server over ssh (ansible-server)
  - `Git`
  - `Github`
  - `Deploy to container plugin`
  - `

2. Configure maven path

- `Manage Jenkins` > `Global Tool Configuration` > `Maven`
