pipeline {
    agent any
    environment {
        APP_NAME = "htmlwebapp"
        RELEASE = "1.0.0"
        DOCKER_USER = "amitsingh01"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages {
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

        }
        stage ('Cleanup Artifacts') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage ('Running the Container') {
            steps {
                script {
                    sh "docker run -it -d -p 9999:80 --name htmlwebtest ${IMAGE_NAME}:latest"
                }
            }
        }
        
    }
   
}