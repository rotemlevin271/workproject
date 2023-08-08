pipeline {
    agent any

    environment {
        // Set environment variables for GitHub repository and Docker Hub credentials
        GITHUB_REPO = 'https://github.com/rotemlevin271/workproject.git'
        DOCKER_HUB_USERNAME = credentials('rotemlevin')
        DOCKER_HUB_PASSWORD = credentials('0546601040')
        DOCKER_IMAGE_NAME = 'webserver'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository
                git branch: 'master', url: "${GITHUB_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image from the Dockerfile in the repository
                script {
                    def dockerImage = docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}", "./Dockerfile") 
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                // Login to Docker Hub
                sh "echo ${DOCKER_HUB_PASSWORD} | docker login -u ${DOCKER_HUB_USERNAME} --password-stdin"
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Push the Docker image to Docker Hub
                script {
                    def dockerImage = docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                    dockerImage.push()
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/master']],
                          userRemoteConfigs: [[url: 'https://github.com/rotemlevin271/workproject.git']]
                ])
            }
        }
    }
}
