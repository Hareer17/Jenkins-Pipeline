pipeline {
    agent any
    stages {
        
         stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }

         }
         stage('Build Docker Image') {
            steps {
                script {
                    def imageName = "myapp:latest"
                    sh "docker build -t ${imageName} ."
                }
            }
        }

        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    def containerName = "myapp_container"
                    
                    // Check if container is running
                    def isRunning = sh(script: "docker ps -q -f name=${containerName}", returnStdout: true).trim()
                    
                    if (isRunning) {
                        echo "Stopping and removing existing container..."
                        sh "docker stop ${containerName}"
                        sh "docker rm ${containerName}"
                    } else {
                        echo "No existing container running."
                    }
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    def containerName = "myapp_container"
                    def imageName = "myapp:latest"

                    // Run the new container
                    sh "docker run -itd --name ${containerName} -p 3000:3000 ${imageName}"
                }
            }
        }
    }
}