# SonarQube-Manual-Scanner-Setup

This project demonstrates how to set up a SonarQube server and manually trigger a scan using SonarScanner across two Linux virtual machines. It is useful for understanding the basics of SonarQube integration for code quality analysis.

## 🖥️ Architecture Overview

- **VM 1 - sonar-server:** Hosts the SonarQube Server.

- **VM 2 - sonar-scanner:** Hosts the SonarScanner CLI and the project codebase to be analyzed.

## Prerequisites

- Two Linux VMs (Ubuntu recommended)

- Minimum 2GB RAM for the SonarQube server

- Internet access on both VMs

## 🔧 Step 1: Setup SonarQube Server (on sonar-server VM)

### 1. Switch to root and update the system

> sudo su
>> apt update

### 2. Install Java 17

> apt install openjdk-17-jdk -y

### 3. Download and Setup SonarQube

 > cd /opt
 >> wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.3.0.104237.zip
 >>> unzip sonarqube-25.3.0.104237.zip
 >>>> mv sonarqube-25.3.0.104237 sonar

### 4. Create non-root user and set permissions

> useradd sonaruser
>> chown -R sonaruser:sonaruser /opt/sonar

### 5. Start SonarQube

> cd /opt/sonar/bin/linux-x86-64
>> ./sonar.sh start

### 6. Access the Web UI

URL: **http://<SONAR_SERVER_IP>:9000**

Login: **admin / admin**

**Change the password when prompted**

### 7. Generate a Token

**Go to Administration > Security**

Generate and save the token

## ⚙️ Step 2: Setup SonarScanner (on sonar-scanner VM)

### 1. Clone the project repository

> git clone <your-project-repo-url>

### 2. Install SonarScanner

> cd /opt
>> wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.0.2.4839-linux-x64.zip
>>> unzip sonar-scanner-cli-7.0.2.4839-linux-x64.zip
>>>> mv sonar-scanner-7.0.2.4839-linux-x64 sonarscanner

### 3. Configure SonarScanner

Edit the configuration file:

> nano /opt/sonarscanner/conf/sonar-scanner.properties

Add this line at the bottom:

> sonar.host.url=http://<SONAR_SERVER_IP>:9000

### 4. Verify Installation

> export PATH=$PATH:/opt/sonar-scanner/bin
>>  /opt/sonarscanner/bin/sonar-scanner -v

### 5. Create sonar-project.properties in the cloned project directory

> sonar.projectKey=your_project_key
>> sonar.sources=.
>>> sonar.host.url=http://<SONAR_SERVER_IP>:9000
>>>> sonar.login=<your_generated_token>

### 6. Run the Manual Scan

> /opt/sonarscanner/bin/sonar-scanner

# Result

Once the scan is complete, open your SonarQube dashboard at http://<SONAR_SERVER_IP>:9000 to view the detailed analysis report.

## Notes

- Do not run SonarQube as a root user due to Elasticsearch restrictions.

- Make sure ports (like 9000) are open in the firewall settings of your VM.


