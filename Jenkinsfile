pipeline {
    agent any

    environment {
        IMAGE_NAME = 'your-dockerhub-username/my-app'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:${IMAGE_TAG}").inside {
                        sh 'echo "Running Tests..."'
                        // Replace with actual test commands
                        // e.g., npm test, pytest, etc.
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    docker logout
                    """
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                // Replace <user>@<EC2-IP> with actual values or use credentials
                sshagent (credentials: ['ec2-ssh-key']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@<EC2-IP> '
                        docker pull ${IMAGE_NAME}:${IMAGE_TAG} &&
                        docker stop my-app || true &&
                        docker rm my-app || true &&
                        docker run -d --name my-app -p 80:8080 ${IMAGE_NAME}:${IMAGE_TAG}
                    '
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD pipeline succeeded!'
        }
        failure {
            echo '❌ CI/CD pipeline failed!'
        }
    }
}

