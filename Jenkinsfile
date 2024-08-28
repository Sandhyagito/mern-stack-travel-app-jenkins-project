pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials-id'  // Replace with your Docker Hub credentials ID
        GIT_CREDENTIALS_ID = 'github-credentials-id'        // Replace with your GitHub credentials ID
        SONARQUBE_SERVER = 'sonarqube'                      // SonarQube server name configured in Jenkins
        GIT_REPO_URL = 'https://github.com/Sandhyagito/mern-stack-travel-app-jenkins-project.git'  // Replace with your GitHub repo URL
        GIT_BRANCH = 'main'                                 // Replace with your branch name
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                // Checkout code from GitHub
                git credentialsId: "${GIT_CREDENTIALS_ID}", branch: "${GIT_BRANCH}", url: "${GIT_REPO_URL}"
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    // Run SonarQube code analysis
                    withSonarQubeEnv("${SONARQUBE_SERVER}") {
                        sh 'mvn clean verify sonar:sonar'
                    }
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                script {
                    // Build Docker image for the frontend
                    docker.build('frontend-image', 'frontend/')
                }
            }
        }
        
        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Build Docker image for the backend
                    docker.build('backend-image', 'backend/')
                }
            }
        }
        
        stage('Push Docker Images') {
            steps {
                script {
                    // Push Docker images to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        docker.image('frontend-image').push('latest')
                        docker.image('backend-image').push('latest')
                    }
                }
            }
        }
        
        stage('Deploy Containers') {
            steps {
                script {
                    // Deploy the frontend and backend containers
                    echo 'Deploying frontend and backend containers...'
                    sh 'docker run -d -p 3000:3000 --name frontend-container frontend-image'
                    sh 'docker run -d -p 5000:5000 --name backend-container backend-image'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Add any cleanup steps if needed
        }
    }
}
