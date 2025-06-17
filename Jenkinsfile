pipeline {
    agent any

    environment {
        IMAGE = "gopalraj321/hello-devops:latest"
    }

    stages {
      stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE% .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    docker push %IMAGE%
                    '''
                }
            }
        }

        stage('Deploy to Minikube') {
    steps {
        withEnv(['KUBECONFIG=C:\\ProgramData\\Jenkins\\.kube\\config']) {
            bat 'kubectl apply -f k8s-deployment.yaml'
        }
    }

    } 
   }
 }
