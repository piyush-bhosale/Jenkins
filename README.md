##Jenkins CI/CD Pipeline for Java Application##

**Project Overview**
This project showcases a complete Jenkins-based CI/CD pipeline for a Java application, built using Maven, scanned using SonarQube, packaged as a Docker image, and pushed to AWS ECR. It demonstrates a real-world DevOps workflow, integrating automation, code quality, and containerization using modern DevOps tools and best practices.

**1) Checkout Source Code**
Jenkins clones the Java project from GitHub.

**2) Clean & Compile**
Maven cleans the project and compiles the Java code to check for build errors.

**3) Unit Testing**
Maven runs unit tests (mvn test) to ensure the application behaves as expected.

**4) Static Code Analysis with SonarQube**
SonarQube scans the code for: Bugs, Vulnerabilities, Code smells, Maintainability issues

**5) SonarQube Quality Gate Check**
The pipeline waits for SonarQube’s Quality Gate to pass.
If it fails, the pipeline stops.

**6) Package the Application**
Maven packages the code into a .jar file (Java archive).

**7) Build Docker Image**
Docker builds a container image using the JAR file via the Dockerfile.

**8) Tag & Push to AWS ECR**
The image is tagged with a Git SHA + build number and pushed to your AWS ECR repo.

**9) Slack Notification**
On success or failure, Jenkins sends a notification to a Slack channel (optional).

