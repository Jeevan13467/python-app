pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'flask-calculator:latest'
        DOCKERHUB_USERNAME = 'jeevan7790'  // Your Docker Hub username
        DOCKERHUB_ACCESS_TOKEN = 'dckr_pat_4n7KabdBBPt7SFkXKTPCUSO3ZkM'  // Your Docker Hub access token
        DOCKER_REGISTRY = 'jeevan7790/python-app'  // Your Docker repository
    }

    stages {
        stage('Checkout/source') {
            steps {
                // Clone the repository containing your Flask calculator application
                git 'https://github.com/Jeevan13467/python-app'  // Replace with your repository URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image for the Flask application
                    sh "sudo docker build -t ${firstimage} ."
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run unit tests inside the Docker container
                    sh """
                    sudo docker run --name test-container ${firstimage} /bin/sh -c 'python -m unittest discover -s /app/tests'
                    sudo docker rm test-container
                    """
                }
            }
        }

        stage('Push to Docker Registry') {
            steps {
                script {
                    // Login to Docker Hub
                    sh """
                    echo ${DOCKERHUB_ACCESS_TOKEN} | sudo docker login -u ${jeevan7790} --password-stdin
                    # Tag and push the Docker image
                    sudo docker tag ${firstimage} ${jeevan7790}/${firstimage}
                    sudo docker push ${jeevan7790}/${firstimage}
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the application by running the Docker container
                    sh """
                    sudo docker run -d -p 5000:80 ${firstimage}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
