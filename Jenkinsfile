pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "roxane451/jenkins_devops_exams"
        DOCKER_REGISTRY = "https://https://hub.docker.com/repositories/roxane451"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/roxane451/cuddly-octo-barnacle.git'
        }

        stage('Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${GIT_COMMIT}")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Ajouter les tests pour ton application ici.
                    echo 'Tests réussis'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${GIT_COMMIT}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f kubernetes/dev-deployment.yml --namespace=dev'
                    sh 'kubectl apply -f kubernetes/qa-deployment.yml --namespace=qa'
                    sh 'kubectl apply -f kubernetes/staging-deployment.yml --namespace=staging'
                }
            }
        }

        stage('Manual Approval for Production') {
            steps {
                input message: 'Approve production deployment?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    sh 'kubectl apply -f kubernetes/prod-deployment.yml --namespace=prod'
                }
            }
        }
    }
}
}
