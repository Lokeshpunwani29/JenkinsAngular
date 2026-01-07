pipeline {
    agent any

    tools {
        // Use the NodeJS tool you configured in Jenkins
        nodejs 'node2217'
    }

    environment {
        APP_NAME   = "jenkins-angular"
        IMAGE_NAME = "jenkins-angular-image"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Lokeshpunwani29/JenkinsAngular'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop old container if exists
                bat 'docker stop %APP_NAME% || exit 0'

                // Remove old container if exists
                bat 'docker rm %APP_NAME% || exit 0'

                // Run new container
                bat 'docker run -d -p 8081:80 --name %APP_NAME% %IMAGE_NAME%'
            }
        }
    }

    post {
        success {
            echo 'Angular application deployed successfully 🚀'
            echo 'Access it at: http://localhost:8081'
        }
        failure {
            echo 'Build or deployment failed ❌'
        }
    }
}
