pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/hbopche/Docker-DevOps.git']])

                // Run Maven on a Unix agent.
                sh "mvn clean install"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build --build-arg JAR_FILE=target/devops-integration.jar -t hbopche/devops-integration .'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u hbopche -p ${dockerhubpwd}'    
}
                    sh 'docker push hbopche/devops-integration'
                }
            }
        }
        stage('Pull Docker Image from DockerHub') {
            steps {
                script {
                    sh 'docker pull devops-integration:latest'
                    sh 'docker run --name -d devops-integration:latest myapp'
    }
}
