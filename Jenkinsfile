pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    stages {
        stage('Code Scan') {
            steps {
                echo 'Scanning Project Code'
                sh 'ls -ltr'
                sh '''
                   mvn sonar:sonar \
                       -Dsonar.host.url=http://54.82.70.112:9000 \
                       -Dsonar.login=squ_8e3bf6e920a645b3c98c8a223cda7bb89f612983
                '''
            }
        }
        stage('Build Artifact'){
            steps{
                echo 'Build Atrifact'
                sh 'mvn clean package'
            }
        }
        stage('Docker Build'){
            steps{
                echo 'Docker Image Building'
                sh   ' docker build -t lakshmanarao18/devops-frontend:${BUILD_NUMBER} . '
            }
        }
        stage('Push to Dockerhub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                    sh ' docker login -u lakshmanarao18 -p ${dockerhub}'
                    sh ' docker push lakshmanarao18/devops-frontend:${BUILD_NUMBER} '
                    echo ' pushed to Dockerhub'    
                    }
                }
            }
        }
        stage('Update Deployment File'){
            environment{
                GIT_REPO_NAME = 'cicd-deploymentfiles'
                GIT_USER_NAME = 'lakshmanaraokaraka57'
            }
            steps{
                echo ' Update Deployment Manifest Files'
                withCredentials([string(credentialsId: 'githubtoken', variable: 'githubtoken')]) {
                    sh '''
                    git config user.email "lasyakaraka@gmail.com"
                    git config user.name "Lakshmanarao"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    sed -i "s/devops-frontend:.*/devops-frontend:${BUILD_NUMBER}/g" deployment.yaml
                    '''
                }
            }
        }
        
    }
}
