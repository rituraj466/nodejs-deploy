pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/rituraj466/nodejs-deploy.git'
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
            echo 'Pipeline completed successfully - Quality Gate PASSED'
        }

        failure {
            echo 'Pipeline failed - Quality Gate FAILED or another stage failed'
        }
    }
}
