pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                git 'https://github.com/kulkarni109/devops-cicd-123.git'
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker buildx build -t ak2228/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u ak2228 -p ${dockerhubpwd}'

}
                   sh 'docker push ak2228/devops-integration'
                }
            }
        }
        stage('EKS and Kubectl configuration'){
            steps{
                script{
                    sh 'aws eks update-kubeconfig --region ap-south-1 --name ankit-cluster'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    sh 'kubectl apply -f deploymentservice.yaml'
                }
            }
        }
    }
}
