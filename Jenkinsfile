pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Yogesh-10X/devops-task-manager.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t yogeshx10/task-manager:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f task-manager-container || true'
                sh 'docker run -d -p 5000:5000 --name task-manager-container yogeshx10/task-manager:latest'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push yogeshx10/task-manager:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
             steps {
                sh 'kubectl --kubeconfig=/var/jenkins_home/.kube/config apply -f deployment.yaml'
                sh 'kubectl --kubeconfig=/var/jenkins_home/.kube/config apply -f service.yaml'
            }
        }
    }
}