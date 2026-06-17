pipeline {
    agent any


 environment {
    AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_REGION = 'ap-south-1'
        DOCKER_IMAGE = 'flask-calculator:latest'
        DOCKERHUB_USERNAME = 'jeeavan7790'
        DOCKERHUB_ACCESS_TOKEN = credentials('docker-hub-token ')
        DOCKER_REGISTRY = 'jeevan7790/python-app'
 }

 stages{

    stage('checkout/source'){
        steps {
            git 'https://github.com/Jeevan13467/python-app.git'
        }
    }

 }
    stage('Build Docker image'){
        steps{
            script{
                sh "pwd"      
                sh docker build -t $(DOCKER_IMAGE)
                      }
        }
    }
   stages{
     stage('test'){
        steps{
            sh"""
            sudo docker run --name test container $(DOCKER_IMAGE) /bin/sh -c 'python -m unittest discover -s /app/tests'
              sudo docker rm test-container
              """
        }
        stage('Push to Docker Registry') {
            steps {
                script {
                    sh """
                    echo ${dockerhub_token} | sudo docker login -u ${DOCKERHUB_USERNAME} --password-stdin

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
                    sudo docker run -d -p 5000:80 ${DOCKER_IMAGE}
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
     }
   
