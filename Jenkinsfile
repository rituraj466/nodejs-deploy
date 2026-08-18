pipeline {
    agent any

    stages {

        stage('Display Parameters') {
            steps {
                echo "RUN_TESTS = ${params.RUN_TESTS}"
                echo "RUN_CONTAINER = ${params.RUN_CONTAINER}"
                echo "IMAGE_TAG = ${params.IMAGE_TAG}"
                echo "BRANCH = ${params.BRANCH}"
                echo "ENVIRONMENT = ${params.ENVIRONMENT}"
                echo "PASSWORD = ******"
            }
        }

        stage('Checkout') {
            steps {
                echo "Selected branch: ${params.BRANCH}"
            }
        }

        stage('Run Tests') {
            when {
                expression {
                    params.RUN_TESTS == true
                }
            }
            steps {
                echo "Running application tests..."
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image: myapp:${params.IMAGE_TAG}"
                sh "docker build -t myapp:${params.IMAGE_TAG} ."
            }
        }

        stage('Deploy DEV') {
            when {
                expression {
                    params.ENVIRONMENT == 'dev'
                }
            }
            steps {
                echo "Deploying to DEV environment"
            }
        }

        stage('Deploy QA') {
            when {
                expression {
                    params.ENVIRONMENT == 'qa'
                }
            }
            steps {
                echo "Deploying to QA environment"
            }
        }

        stage('Deploy PROD') {
            when {
                expression {
                    params.ENVIRONMENT == 'prod'
                }
            }
            steps {
                echo "Deploying to PROD environment"
            }
        }

        stage('Run Container') {
            when {
                expression {
                    params.RUN_CONTAINER == true
                }
            }
            steps {
                echo "Running Docker container..."
                sh "docker run -d --name myapp-container myapp:${params.IMAGE_TAG}"
            }
        }
    }
}
