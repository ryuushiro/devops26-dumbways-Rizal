pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKERHUB_USERNAME = 'ramdhanifauzi'
        IMAGE_NAME = 'wayshub-frontend'
        SERVER1_IP = '103.55.37.38'
    }

    stages {

        stage('Pull from GitHub') {
            steps {
                echo 'Pulling latest code...'
                git branch: 'main', url: 'https://github.com/kelompok1-dumbways/wayshub-frontend'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh """
                    docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest .
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing to Docker Hub...'
                sh """
                    echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                    docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest
                """
            }
        }

        stage('Deploy to Server 1') {
            steps {
                echo 'Deploying to Server 1...'
                sshagent(['server1-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@${SERVER1_IP} '
                            docker pull ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest &&
                            docker stop wayshub-frontend || true &&
                            docker rm wayshub-frontend || true &&
                            docker run -d --name wayshub-frontend --network wayshub-docker_wayshub-net -p 3000:3000 ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest
                        '
                    """
                }
            }
        }

    }

    post {
        success {
            echo 'Frontend pipeline completed successfully!'
        }
        failure {
            echo 'Frontend pipeline failed!'
        }
    }
}
