pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://65.0.173.83:9000/' // Update with your SonarQube server URL
        SONAR_LOGIN = credentials('sonar-token') // Jenkins credentials ID for SonarQube login
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from Git repository
                git credentialsId: 'your-credentials-id', url: 'https://github.com/caaravn/sample-java-docker-file.git'
            }
        }

        stage('Build') {
            steps {
                // Build the Maven project
                sh 'mvn clean package'
            }
        }

        stage('Code Analysis') {
            steps {
                // Run SonarQube analysis
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image using Dockerfile
                sh 'docker build -t aravind .'
            }
        }


