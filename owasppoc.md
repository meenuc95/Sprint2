<img width="817" height="141" alt="Screenshot 2025-08-21 at 7 20 10 PM" src="https://github.com/user-attachments/assets/58d7c534-8097-42c0-a2e2-0f0e14a3aa7d" />

#  POC of Dependency Scanning in Java CI Checks



## Author Information

| Created by      | Created on         | Version  | Last updated On   | Pre Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|-----------------|--------------------|----------|-------------------|--------------|-------------|-------------|-------------|
| Meenu Chauhan   | 20-08-2025         | V 1.0    | -                 | Siddarth/Sahil| -           | -           | -           |


# Table of Contents  


1. [Introduction](#1-introduction)  
2. [Prerequisites](#2-prerequisites)  
3. [System Requirements](#3-system-requirements)  
4. [Setup and Execution](#4-setup-and-execution)  
5. [Conclusion](#5-conclusion)  
6. [Troubleshooting](#6-troubleshooting)  
7. [Contact Information](#7-contact-information)  
8. [References](#8-references) 


## 1. Introduction

This Proof of Concept (POC) demonstrates how to integrate dependency scanning into a Java project.  It uses OWASP Dependency-Check to identify known vulnerabilities in third-party libraries.  The POC showcases setup, execution, and report generation steps in a CI pipeline.  The goal is to ensure early detection of risks and improve overall application security posture.  

---

## 2. Prerequisites

- *Java*: Java 17 (JDK/JRE)   
- *Internet Access*: To update CVE database (NVD, NPM, etc.)   
- *Build Tool*: Maven/Gradle 
- *NVD API Key (optional but recommended)*: To updates NVD databases fast  
---


## 3. System Requirements


| Requirement        | Minimum                | Recommended                       |
|--------------------|------------------------|-----------------------------------|
| CPU                | 1 vCPU                | 2+ vCPU                           |
| RAM                | 2 GB                  | 4+ GB                             |
| Disk Space         | 2 GB (for CVE DB cache) | 5+ GB (large CVE database + reports) |



---
## 4. Setup and Execution

### 4.1 Install Java (JDK 17) and Required Tools
```bash
sudo apt update
sudo apt install -y openjdk-17-jdk wget unzip
java -version
```

### 4.2 Install OWASP Dependency-Check
```bash
wget https://github.com/jeremylong/DependencyCheck/releases/download/v12.1.0/dependency-check-12.1.0-release.zip
unzip dependency-check-12.1.0-release.zip
cd dependency-check 
```
<img width="1370" height="419" alt="Screenshot 2025-08-21 at 5 21 26 PM" src="https://github.com/user-attachments/assets/aa0fa676-c621-4aa8-9314-c7c149e7e5e3" />

<img width="800" height="56" alt="Screenshot 2025-08-21 at 5 21 38 PM" src="https://github.com/user-attachments/assets/35b65671-d87d-4c6e-b2ea-4d21688da719" />

### 4.3 Configure NVD API Key and Update DB
Generate an API key: https://nvd.nist.gov/developers/request-an-api-key
```bash
export NVD_API_KEY=YOUR_NVD_API_KEY_HERE
echo "$NVD_API_KEY"
./bin/dependency-check.sh --updateonly --nvdApiKey $NVD_API_KEY

```
<img width="1186" height="156" alt="Screenshot 2025-08-21 at 5 27 37 PM" src="https://github.com/user-attachments/assets/c8d2b90f-deb3-4218-949c-083f1cf9887e" />

### 4.4 Install Maven
```bash
sudo apt install -y maven
mvn -version
```

### 4.5 Build Project 

#### Step 1: Clone the repository

```bash
cd /home/ubuntu
git clone https://github.com/OT-MICROSERVICES/salary-api.git
```

#### Step 2: Change into the repository
```bash
cd /home/ubuntu/salary-api
```

#### Step 3: Build the project
```bash
# Build without running tests (faster)
mvn clean package -DskipTests

```
<img width="779" height="173" alt="Screenshot 2025-08-21 at 6 13 21 PM" src="https://github.com/user-attachments/assets/ee50d367-350d-4e42-9850-cb3347dc02a7" />


### 4.6 Run Dependency-Check Scan and Generate HTML Report
```bash
/home/ubuntu/dependency-check/bin/dependency-check.sh \
  --project "salary" \
  --scan /home/ubuntu/salary-api/target \
  --format HTML \
  --out /home/ubuntu/dc-report.html \
  --nvdApiKey "$NVD_API_KEY"
```
<img width="1393" height="337" alt="Screenshot 2025-08-21 at 6 13 33 PM" src="https://github.com/user-attachments/assets/f20b58e8-1e0b-4aa2-92a4-c9f6689afdb8" />

### 4.7 Serve the Report Locally (port 8080)
```bash
cd /home/ubuntu
python3 -m http.server 8080
```
<img width="868" height="89" alt="Screenshot 2025-08-21 at 6 14 03 PM" src="https://github.com/user-attachments/assets/64004730-94cb-45af-a2ba-6a945510a9e7" />

Open in a browser (replace with server’s public IP):

<img width="1364" height="720" alt="Screenshot 2025-08-21 at 6 14 23 PM" src="https://github.com/user-attachments/assets/e38fe1cf-947d-4234-83b3-e24fe06b167b" />

---

## 5. Conclusion

Dependency-Check helps identify vulnerable libraries early in the build process.  


---

## 6. Troubleshooting


| Issue | Possible Cause | Solution |
|-------|----------------|----------|
| dependency-check.sh: command not found | Dependency-Check binaries not in PATH | Run from extracted folder (./bin/dependency-check.sh) or add it to PATH |
| Empty / Blank HTML report | Wrong scan path (scanning pom.xml instead of compiled JAR/classes) | Use target folder after build: --scan /home/ubuntu/salary-api/target |
| mvn: command not found | Maven not installed | Install Maven: sudo apt install maven -y |


---


## 7. Contact information


## Contact Information

| Name           | Email address                           |
|----------------|-----------------------------------------|
| Meenu Chauhan  | meenu.chauhan.snaatak@mygurukulam.co    |


---

## 8. References

| *Title* | *Link* |
|-----------|----------|
| Dependancy Scanning |[Link](https://docs.gitlab.com/ee/user/application_security/dependency_scanning/) 
| Dependancy Scanning tool |[Link](https://finitestate.io/blog/best-java-scanner)
