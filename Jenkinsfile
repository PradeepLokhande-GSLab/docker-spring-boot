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
                    DockerImage = docker.build registry
                    DockerImage.tag("$BUILD_NUMBER")
                }
            }
        }
        stage ("Push to ECR") {
            steps {
                script {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 211125766521.dkr.ecr.us-east-1.amazonaws.com' 
                    sh 'docker push 211125766521.dkr.ecr.us-east-1.amazonaws.com/spring-app:$BUILD_NUMBER'
                }
            }
        }
        stage ("Helm deploy") {
            steps {
                script {
                    sh "helm upgrade first --install mychart --namespace helm --set image.tag=$BUILD_NUMBER"
                    //sh "helm upgrade first --install mychart --namespace helm --set image.tag=$BUILD_NUMBER"
                }
            }
        }
    }
}
