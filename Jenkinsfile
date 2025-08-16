    pipeline {
        agent any 

        environment {
            DOCKER_IMAGE = "shivaavvari/noteapp"
            DOCKER_CREDENTIAL_ID = "bcc28708-1540-454d-81ea-30e7b5fd2fbc" // ID of your Docker Hub credentials in Jenkins
        }

        stages {
            stage('Checkout Code') {
                steps {
                    git url: 'https://github.com/shivaavvari/noteapp.git', branch: 'main' // Replace with your repo URL and branch
                }
            }

            stage('Build Docker Image') {
                steps {
                    script {
                        sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
                        sh "docker tag ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                    }
                }
            }

            stage('Push to Docker Hub') {
                steps {
                    script {
                        withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIAL_ID}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                            sh' echo "$DOCKER_PASSWORD" | docker login --username "$DOCKER_USERNAME" --password-stdin '
                            sh " docker push ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                            sh " docker push ${DOCKER_IMAGE}:latest"
                            sh " docker logout" // Optional: Logout for security
                        }
                    }
                }
            }
        }
    }
