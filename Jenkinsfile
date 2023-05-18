pipeline {
    agent any
    tools {
       maven 'maven'
    }
    stages{
         stage('Build maven'){
           steps{
                 checkout([$class: 'GitSCM',branches: [[name: '*/main']], extensions: [], userRemoteconfigs: [[url: 'https://github.com/Raam4207/devops-automation.git'
                 sh 'mvn clean install'
             }
         }
        stage('Build & Test'){
            steps{
                 cleanWs()
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t raam2023/devops-integrate .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'DOCKER_PWD', usernameVariable: 'DOCKER_USER')]) {
                       sh 'docker login -u ${DOCKER_USER} -p ${DOCKER_PWD}'
                       sh 'docker push ${DOCKER_USER}/devops-integrate'
                }
            }
        }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                         sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}
