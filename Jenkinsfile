pipeline {
    agent any
    tools {
       maven 'maven 3.8.6'
    }
    stages{
         stage('Build maven'){
           steps{
                 checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT_PWD', url: 'https://github.com/Raam4207/devops-automation.git']])
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
                    sh 'docker build -t raam2023/devintegration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([usernameColonPassword(credentialsId: 'docker-cred', variable: 'docker_crede')]) {
                   sh 'docker login -u ${docker-cred} -p ${docker_crede}'  
                   sh 'docker push ${docker-cred}/devintegration'
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
