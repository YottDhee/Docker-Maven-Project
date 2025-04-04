pipeline {
    agent any

    parameters {
        string(name: 'TAG', defaultValue: 'latest', description: 'Tag for the Docker image')
        choice(name: 'ENV', choices: ['test', 'uat', 'dev', 'prod','pre-prod'], description: 'Environment')
    }

    environment {
        IMAGE_NAME = "YottDheeDockerImages/${params.ENV}:${params.TAG}"
    }

    stages {
        stage('Login to Docker Registry') {
            steps {
                script {
		  withCredentials([
		usernamePassword(credentialsId: 'docker-credentials-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')
			]){ 
                    echo "Logging in to Docker registry"
		    sh"""
			echo \$DOCKER_PASSWORD | sudo docker login -u $DOCKER_USER --password-stdin
		    """
			}
                }
            }
        }

        stage('Build Docker Image ') {
            steps {
                script {
                    echo "Building Docker image: ${env.IMAGE_NAME}"
                    sh "sudo docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Push Image to Dockerhub') {
            steps {
                script {
                    echo "Pushing Docker image: ${env.IMAGE_NAME}"
                    sh "sudo docker push ${IMAGE_NAME}"
                }
            }
        }
    }
}