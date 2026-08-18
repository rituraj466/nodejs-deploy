pipeline {
    agent any

    parameters {
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run test stage?')
        booleanParam(name: 'RUN_CONTAINER', defaultValue: false, description: 'Run the container after build?')
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Git branch to checkout')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'Target environment')
        password(name: 'PASSWORD', defaultValue: '', description: 'Deployment password (masked)')
    }

    environment {
        REPO_URL = 'https://github.com/rituraj466/nodejs-deploy.git'
        IMAGE_NAME = 'nodejs-deploy'
    }

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
                script {
                    // Null-safety: fall back to 'main' if BRANCH param is empty/null
                    def branchToCheckout = params.BRANCH?.trim() ? params.BRANCH.trim() : 'main'
                    echo "Checking out branch: ${branchToCheckout}"

                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: "*/${branchToCheckout}"]],
                        userRemoteConfigs: [[url: env.REPO_URL]]
                    ])
                }
            }
        }

        stage('Run Tests') {
            when {
                expression { return params.RUN_TESTS }
            }
            steps {
                echo 'Running tests...'
                // Original job had "echo message and remove npm test" per commit history.
                // Replace this echo with actual test commands, e.g.:
                // sh 'npm install && npm test'
                echo 'Tests step placeholder - update with real test command if needed.'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def tag = params.IMAGE_TAG?.trim() ? params.IMAGE_TAG.trim() : 'latest'
                    echo "Building Docker image: ${env.IMAGE_NAME}:${tag}"
                    sh "docker build -t ${env.IMAGE_NAME}:${tag} ."
                }
            }
        }

        stage('Deploy DEV') {
            when {
                expression { return params.ENVIRONMENT == 'dev' }
            }
            steps {
                echo 'Deploying to DEV environment...'
                // Add your dev deployment commands here
            }
        }

        stage('Deploy QA') {
            when {
                expression { return params.ENVIRONMENT == 'qa' }
            }
            steps {
                echo 'Deploying to QA environment...'
                // Add your qa deployment commands here
            }
        }

        stage('Deploy PROD') {
            when {
                expression { return params.ENVIRONMENT == 'prod' }
            }
            steps {
                echo 'Deploying to PROD environment...'
                // Add your prod deployment commands here
            }
        }

        stage('Run Container') {
            when {
                expression { return params.RUN_CONTAINER }
            }
            steps {
                script {
                    def tag = params.IMAGE_TAG?.trim() ? params.IMAGE_TAG.trim() : 'latest'
                    echo "Running container from image: ${env.IMAGE_NAME}:${tag}"
                    sh "docker run -d --name ${env.IMAGE_NAME}-container ${env.IMAGE_NAME}:${tag}"
                }
            }
