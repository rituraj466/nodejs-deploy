pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/rituraj466/nodejs-deploy.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        def scannerHome = tool 'SonarScanner'
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
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
                sh 'docker build -t nodejs-app:latest .'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Quality Gate passed and Docker image was built.'
        }

        failure {
            echo 'FAILURE: Quality Gate failed or another stage failed.'
        }
    }
}
