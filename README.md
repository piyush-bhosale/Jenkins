# Jenkins CI/CD Pipeline for Java Application with Docker & SonarQube

## 🚀 Project Overview

This repository demonstrates a full **Jenkins CI/CD pipeline** for a Java-based application. The pipeline covers everything from **building the code**, running **unit tests**, performing **static code analysis**, to **containerizing the app** and **pushing the Docker image to AWS ECR**.

It reflects **real-world DevOps practices** using tools like Jenkins, Maven, SonarQube, Docker, and AWS — designed to be clean, modular, and recruiter-friendly.

---

## ⚙️ Tools & Technologies Used

| Tool        | Purpose                                  |
|-------------|-------------------------------------------|
| **Jenkins** | CI/CD orchestration                      |
| **Maven**   | Java build & dependency management       |
| **SonarQube** | Static code analysis                   |
| **Docker**  | Containerization                         |
| **AWS ECR** | Container registry for storing images    |
| **GitHub**  | Source control                           |
| **Slack** *(optional)* | Build notifications           |

---

## 📁 Repository Structure

| File                  | Description |
|-----------------------|-------------|
| `Jenkinsfile`         | Full pipeline: Build → Test → Scan → Package → Dockerize → Push to ECR |
| `Dockerfile`          | Builds a Docker image from the generated JAR file |
| `pom.xml`             | Maven config to compile & package the Java app |
| `HelloWorld.java`     | Simple Java class to simulate real application |
| `sonar-project.properties` | Configuration for SonarQube scanning |
| `README.md`           | Project documentation (this file) |

---

## 🧪 What This Project Demonstrates

### **1. CI/CD Automation with Jenkins**
A full declarative pipeline using `Jenkinsfile` with clean and efficient stages.

### **2. Build & Test**
Runs `mvn clean compile` and `mvn test` to compile code and verify functionality.

### **3. Code Quality Scanning (SonarQube)**
Scans Java code using SonarQube and enforces **quality gate policies** before proceeding.

### **4. Docker Image Build**
Packages the JAR as a Docker image using the provided `Dockerfile`.

### **5. Push to AWS ECR**
Tags and pushes the Docker image to **Amazon Elastic Container Registry (ECR)** using AWS CLI.

### **6. Slack Notifications (Optional)**
Sends success/failure notifications to Slack (can be enabled with credentials).

---

## 📦 How to Use This Repo

### **1. Clone the Repository**
```bash
git clone https://github.com/piyush-bhosale/Jenkins-Pipeline.git
cd Jenkins-Pipeline
