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

        stage('Checkout Selected Branch') {
            steps {
                echo "Checking out branch: ${params.BRANCH}"

                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "*/${params.BRANCH}"]],
                    userRemoteConfigs: [[
                        url: 'https://github.com/rituraj466/nodejs-deploy.git'
                    ]]
                ])
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
                echo "Building Docker image with tag: ${params.IMAGE_TAG}"

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
                echo "Deploying application to DEV environment"
            }
        }

        stage('Deploy QA') {
            when {
                expression {
                    params.ENVIRONMENT == 'qa'
                }
            }
            steps {
                echo "Deploying application to QA environment"
            }
        }

        stage('Deploy PROD') {
            when {
                expression {
                    params.ENVIRONMENT == 'prod'
                }
            }
            steps {
                echo "Deploying application to PROD environment"
            }
        }

        stage('Run Container') {
            when {
                expression {
                    params.RUN_CONTAINER == true
                }
            }
            steps {
                echo "Running Docker container with image: myapp:${params.IMAGE_TAG}"

                sh """
                    docker rm -f myapp-container || true
                    docker run -d --name myapp-container -p 3000:3000 myapp:${params.IMAGE_TAG}
                """
            }
        }
    }
}
