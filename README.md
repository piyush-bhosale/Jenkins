📌 How the Jenkins Pipeline Works
This Jenkins pipeline is designed to automate the complete CI/CD process for a Java-based application, ensuring code quality, fast delivery, and secure deployments.

🔄 Pipeline Workflow Overview
🔁 Code Checkout
The pipeline pulls the latest source code from a GitHub repository.

It also extracts the Git commit hash for traceable Docker image tagging.

🧹 Clean & Compile
Uses Maven to clean the workspace and compile Java source code.

🧪 Unit Testing
Executes unit tests using mvn test to validate functionality before packaging.

🔍 Code Quality Check
Integrates with SonarQube to scan the source code for:

Bugs

Vulnerabilities

Code smells

Maintainability issues

✅ Quality Gate Enforcement
Waits for SonarQube's quality gate status.

If the gate fails, the pipeline aborts, enforcing high code standards.

📦 Package Application
Maven packages the Java application as a .jar file, skipping tests to speed up delivery.

📤 Upload to JFrog Artifactory
Deploys the generated .jar binary to a JFrog Artifactory repository for artifact storage and versioning.

🐳 Build Docker Image
Uses the Dockerfile to create a container image containing the Java app.

Tags the image with a unique identifier (commit hash + build number).

🚀 Push to AWS ECR
Authenticates with AWS Elastic Container Registry (ECR).

Pushes the Docker image for deployment in environments like EKS or ECS.

📢 Slack Notification
Sends a success or failure message to a specified Slack channel for team visibility.
