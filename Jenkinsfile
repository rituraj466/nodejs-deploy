pipeline {

    agent any

    parameters {

        string(
            name: 'IMAGE_TAG',
            defaultValue: 'v1.0',
            description: 'Docker image tag'
        )

        choice(
            name: 'GIT_BRANCH',
            choices: ['master', 'develop', 'release'],
            description: 'Select Git branch'
        )

        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run tests'
        )

        booleanParam(
            name: 'RUN_CONTAINER',
            defaultValue: true,
            description: 'Run Docker container'
        )

        choice(
            name: 'DEPLOY_ENV',
            choices: ['DEV', 'QA', 'PROD'],
            description: 'Deployment environment'
        )
    }

    stages {

        stage('Display Parameters') {
            steps {
                echo "IMAGE_TAG     = ${params.IMAGE_TAG}"
                echo "GIT_BRANCH    = ${params.GIT_BRANCH}"
                echo "RUN_TESTS     = ${params.RUN_TESTS}"
                echo "RUN_CONTAINER = ${params.RUN_CONTAINER}"
                echo "DEPLOY_ENV    = ${params.DEPLOY_ENV}"
            }
        }

        stage('Checkout') {
            steps {
                echo "Checking out branch: ${params.GIT_BRANCH}"

                git branch: "${params.GIT_BRANCH}",
                    url: 'https://github.com/rituraj466/nodejs-deploy.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            when {
                expression {
                    params.RUN_TESTS == true
                }
            }

            steps {
                echo "RUN_TESTS is TRUE - Running tests"

                sh 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                echo "Building Docker image: myapp:${params.IMAGE_TAG}"

                sh """
                    docker build -t myapp:${params.IMAGE_TAG} .
                """
            }
        }

        stage('Deploy DEV') {
            when {
                expression {
                    params.DEPLOY_ENV == 'DEV'
                }
            }

            steps {
                echo "Deploying application to DEV"
            }
        }

        stage('Deploy QA') {
            when {
                expression {
                    params.DEPLOY_ENV == 'QA'
                }
            }

            steps {
                echo "Deploying application to QA"
            }
        }

        stage('Deploy PROD') {
            when {
                expression {
                    params.DEPLOY_ENV == 'PROD'
                }
            }

            steps {
                echo "Deploying application to PROD"
            }
        }

        stage('Run Container') {
            when {
                expression {
                    params.RUN_CONTAINER == true
                }
            }

            steps {
                echo "RUN_CONTAINER is TRUE - Starting container"

                sh """
                    docker rm -f myapp-container || true
                    docker run -d \
                    --name myapp-container \
                    -p 3000:3000 \
                    myapp:${params.IMAGE_TAG}
                """
            }
        }
    }
}
