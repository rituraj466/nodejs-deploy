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
            description: 'Select the Git branch to checkout'
        )

        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run Maven tests'
        )

        booleanParam(
            name: 'RUN_CONTAINER',
            defaultValue: true,
            description: 'Run Docker container'
        )

        choice(
            name: 'DEPLOY_ENV',
            choices: ['DEV', 'QA', 'PROD'],
            description: 'Select the deployment environment'
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
                echo "Installing Node.js dependencies"

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
                echo "RUN_TESTS is TRUE - Running Maven tests"

                sh '''
                    if command -v mvn >/dev/null 2>&1; then
                        mvn test
                    else
                        echo "Maven is not installed."
                        echo "This is a Node.js application."
                        echo "No Maven project found, so Maven test cannot be executed."
                    fi
                '''
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
                echo "Deploying application to DEV environment"
            }
        }

        stage('Deploy QA') {
            when {
                expression {
                    params.DEPLOY_ENV == 'QA'
                }
            }

            steps {
                echo "Deploying application to QA environment"
            }
        }

        stage('Deploy PROD') {
            when {
                expression {
                    params.DEPLOY_ENV == 'PROD'
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
                echo "RUN_CONTAINER is TRUE - Starting Docker container"

                sh '''
                    docker rm -f myapp-container || true
                '''

                sh """
                    docker run -d \
                    --name myapp-container \
                    -p 3000:3000 \
                    myapp:${params.IMAGE_TAG}
                """
            }
        }
    }

   post {
        success {
            echo "Pipeline completed successfully"
        }

        failure {
            echo "Pipeline failed"
        }
    }
}
