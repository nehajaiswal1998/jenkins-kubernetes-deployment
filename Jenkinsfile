pipeline {
    environment {
        dockerimagename = "neha8888/react-app"
    }

    agent any

    stages {

        stage('Checkout Source') {
            steps {
                git branch: '*/main', url: 'https://github.com/nehajaiswal1998/jenkins-kubernetes-deployment.git'
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }

        stage('Pushing Image') {
            environment {
                registryCredential = 'dockerHub'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("${dockerimagename}:latest")
                    }
                }
            }
        }

        stage('Deploying React.js container to Kubernetes') {
            environment {
                KUBECONFIG_CRED = credentials('kubeconfig-credentials')
            }

            steps {
                script {
                    withCredentials([kubeconfig(credentialsId: 'kubeconfig-credentials')]) {
                        sh 'kubectl get pods'
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                    }
                }
            }
        }
    }
}
