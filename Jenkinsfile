pipeline {
    agent {
        docker {
            image 'ohadd83/python-docker-agent:latest'
            args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    } 
//    agent any
//    agent {
//        docker {
//            image 'python:3.12'
//            args '-u root'
//        }
//    }

    environment {
        IMAGE_NAME = "python-app"
    }


    stages {


//        stage('Checkout') {
//
//            steps {
//
//               git 'https://github.com/your-user/python-app.git'
//
//            }
//        }



        stage('Install Dependencies') {

            steps {

                sh '''
                python3 -m pip install -r requirements.txt
                '''

            }
        }



        stage('Run Tests') {

            steps {

                sh '''
                pytest
                '''

            }
        }



        stage('Build Docker Image') {

            steps {

                sh '''
                docker build -t ${IMAGE_NAME}:latest .
                '''

            }
        }



        stage('Run Container') {

            steps {

                sh '''
                docker run -d \
                --name python-container \
                -p 5000:5000 \
                ${IMAGE_NAME}:latest
                '''

            }
        }

        stage('check if container is up') {
        
            steps {
                sh '''
                docker ps | grep 5000 | awk '{print $1}'
                '''
            }
        }
        stage('Health Check') {

            steps {

                sh '''
                sleep 5
# Dynamically grab the internal container IP
                TARGET_IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' python-container)
                echo $TARGET_IP
                curl http://${TARGET_IP}:5000/health
                '''

            }
        }


    }


    post {

        success {

            echo "Deployment Successful"

        }


        failure {

            echo "Pipeline Failed"

        }


       always {

            sh '''
          docker rm -f python-container || true
          '''

        }

    }

}
