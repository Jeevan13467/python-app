pipeline {
    agent any 
    
    environment {
        DOCKER_IMAGE = 'flask-calculator:latest'
        DOCKERHUB_USERNAME = 'jeeavan7790'
        DOCKERHUB_ACCESS_TOKEN = 'dckr_pat_zjs9Mx10B6XJaMFcc_N3WfGDm6M'
        DOCKER_REGISTRY = 'jeevan7790/python-app'
    }
    
    // OPEN STAGES ONCE HERE
    stages {
        
        stage('checkout/source') {
            steps {
                git 'https://github.com/Jeevan13467/python-app.git'
            }
        }
        
        stage('Build Docker image') {
            steps {
                script {
                    // Fixed missing quotes around your docker build command here too!
                    sh "docker build -t ${DOCKER_IMAGE} ." 
                }
            }
        }
        
        stage('test') {
            steps {
                sh """
                docker run --name test-container ${DOCKER_IMAGE} /bin/sh -c 'python -m unittest discover -s /app/tests'
                docker rm test-container
                """
            }
        }
        
        stage('Push to Docker Registry') {
            steps {
                script {
                    sh """
                    echo ${DOCKERHUB_ACCESS_TOKEN} | sudo docker login -u ${DOCKERHUB_USERNAME} --password-stdin

                    sudo docker tag ${DOCKER_IMAGE} ${DOCKERHUB_USERNAME}/${DOCKER_IMAGE}

                    sudo docker push ${DOCKERHUB_USERNAME}/${DOCKER_IMAGE}
                    """
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sh """
                    docker run -d -p 5000:80 ${DOCKER_IMAGE}
                    """
                }
            }
        }
        
    } // CLOSE STAGES ONCE HERE
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
   
