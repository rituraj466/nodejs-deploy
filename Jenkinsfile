pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'YOUR_GITHUB_REPO_URL'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t python-app:latest .'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Quality Gate passed and Docker image was built.'
        }

        failure {
            echo 'FAILURE: Quality Gate failed or another pipeline stage failed.'
        }
    }
}
