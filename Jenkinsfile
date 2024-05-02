pipeline {   
    agent any
    environment {
        registry = "211125766521.dkr.ecr.us-east-1.amazonaws.com/spring-app"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PradeepLokhande-GSLab/docker-spring-boot']])
            }
        }    
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }     
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }        

    }
}
