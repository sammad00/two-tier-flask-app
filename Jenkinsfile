pipeline {
    agent any

    stages {

        stage('Code Checkout') {
            steps {
                echo 'Cloning source code from GitHub...'
                git url: 'https://github.com/sammad00/two-tier-flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t two-tier-flask-app .'
            }
        }

        stage('Test Phase') {
            steps {
                echo 'Developer/Test phase complete...'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing image to Docker Hub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhubcred',
                    usernameVariable: 'DOCKER_HUB_USER',
                    passwordVariable: 'DOCKER_HUB_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_HUB_PASS" | docker login -u "$DOCKER_HUB_USER" --password-stdin
                        docker tag two-tier-flask-app "$DOCKER_HUB_USER/two-tier-flask-app:latest"
                        docker push "$DOCKER_HUB_USER/two-tier-flask-app:latest"
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Deploying container using Docker Compose...'
                sh '''
                    docker-compose up -d --build
                '''
            }
        }
    }
}
