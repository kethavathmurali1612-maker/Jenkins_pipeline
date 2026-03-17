pipeline {
    agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock -u root'
        }
    }
    pipeline {
    agent any

    environment {
        IMAGE_NAME = "kethavathmurali/2023bcd0008_45"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag ${IMAGE_NAME} ${IMAGE_NAME}:latest'
            }
        }

        stage('Test Docker Login') {
    steps {
        withCredentials([usernamePassword(
        credentialsId: 'dockerhub',
        usernameVariable: 'DOCKER_USER',
        passwordVariable: 'DOCKER_PASS'
        )]) {
            sh '''
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            '''
        }
    }
}

        stage('Push Docker Image') {
            steps {
                sh 'docker push ${IMAGE_NAME}'
                sh 'docker push ${IMAGE_NAME}:latest'
            }
        }

    }
}
