pipeline{
    agent any
    tools {
        maven 'MAVEN_HOME'
    }
    stages {
        stage('Build Maven') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ManojHA24/spring-cloud-api-gateway-service.git']]])

                sh "mvn -Dmaven.test.failure.ignore=true clean compile package"
                
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t manojha/api_gateway:latest .'
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'manojha', variable: 'dockerhub')]) {
                    sh 'docker login -u devopshint -p ${dockerhub}'
                 }  
                 sh 'docker push manojha/api_gateway-latest'
                }
            }
        }
    }
}
