pipeline {
    agent {
        docker {
            image 'maven:3.6.3-jdk-8' // Use an image with the correct JDK version
            args '-v /root/.m2:/root/.m2' // Optional: to persist Maven dependencies
        }
    }
    environment {
        registry = '352415517565.dkr.ecr.us-east-1.amazonaws.com/devops_repository'
        registryCredential = 'jenkins-ecr'
        dockerimage = ''
    }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/koffimichel/helloworld_pipeline.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}