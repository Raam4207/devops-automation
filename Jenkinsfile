pipeline {
    agent any
    tools {
       maven 'mvn 3.9.1'
    }
    stages{
        stage('Clone'){
            steps{
                git branch: 'main', url: 'git@github.com:Raam4207/devops-automation.git'
                
            }
        }
        stage('Build & Test'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t raam2023/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'DOCKER_PWD', usernameVariable: 'DOCKER_USER')]) {
                       sh 'docker login -u ${DOCKER_USER} -p ${DOCKER_PWD}'
                       sh 'docker push ${DOCKER_USER}/devops-integration'
                }
            }
        }
        }
//         stage('Deploy to k8s'){
//             steps{
//                 script{
//                     kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
//                 }
//             }
//         }
    }
}
