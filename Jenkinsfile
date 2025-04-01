
pipeline {
    agent any

    environment {
        MAVEN_HOME = '/opt/maven'
        SONAR_HOST_URL = 'http://<your-sonarqube-host>:9000'
        SONAR_AUTH_TOKEN = credentials('SONAR_TOKEN')
        ARTIFACTORY_URL = 'https://your-jfrog-instance.jfrog.io/artifactory'
        ARTIFACTORY_REPO = 'libs-release-local'
        ARTIFACTORY_CREDENTIALS = credentials('JFROG_CRED_ID')
        AWS_ACCOUNT_ID = '<aws-account-id>'
        AWS_REGION = 'us-east-1'
        ECR_REPO_NAME = 'java-app-demo'
        SLACK_CHANNEL = '#build-status'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-username/java-app-demo.git'
                script {
                    env.GIT_COMMIT_HASH = sh(returnStdout: true, script: "git rev-parse --short HEAD").trim()
                    env.IMAGE_TAG = "${env.GIT_COMMIT_HASH}-${env.BUILD_NUMBER}"
                }
            }
        }

        stage('Clean & Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar -Dsonar.login=${SONAR_AUTH_TOKEN}"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Package JAR') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Publish to Artifactory') {
            steps {
                sh """
                    mvn deploy \
                    -DaltDeploymentRepository=release::default::${ARTIFACTORY_URL}/${ARTIFACTORY_REPO} \
                    --settings settings.xml
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${ECR_REPO_NAME}:${IMAGE_TAG} .
                    docker tag ${ECR_REPO_NAME}:${IMAGE_TAG} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                """
            }
        }

        stage('Push to ECR') {
            steps {
                sh """
                    aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                """
            }
        }
    }

    post {
        success {
            slackSend(channel: env.SLACK_CHANNEL, message: "✅ Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
        failure {
            slackSend(channel: env.SLACK_CHANNEL, message: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
    }
}
