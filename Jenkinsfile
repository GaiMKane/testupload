pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Cloning repository..."
                    // Clone the 'main' branch
                    git branch: 'master', url: 'https://github.com/GaiMKane/testupload.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image for the main branch..."
                    bat "docker build -t to-do-list ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Use a fixed port for the main branch
                    def branchPort = 3008

                    echo "Stopping and removing any existing container for the main branch..."
                    bat "docker stop website-container-main || exit 0"
                    bat "docker rm website-container-main || exit 0"

                    echo "Running new container for the main branch on port ${branchPort}..."
                    bat "docker run --rm -d --name website-container-main -p ${branchPort}:80 to-do-list"

                    echo "Container for the main branch is running on port ${branchPort}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        always {
            echo 'Pipeline completed!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
