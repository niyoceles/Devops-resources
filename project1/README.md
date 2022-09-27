# Devops simple project of CI/CD

## Tools:

- Github
- Jenkins (server)
- Docker
- Ansible

# This is step by step for CI/CD Pipeline

## Step 1. Install Jenkins (server)

For more ://www.jenkins.io/doc/book/installing/linux/#debianubuntu

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
