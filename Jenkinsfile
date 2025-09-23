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
    }
}
