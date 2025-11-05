pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                git url: "https://github.com/sammad00/two-tier-flask-app.git"
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t two-tier-flask-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Developer/Test phase complete...'
            }
        }

        stage('Push to Docker Hub') {
            steps {
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

        stage('Deploy') {
            steps {
                sh 'docker compose up -d --build'


            }
        }
    }
}
