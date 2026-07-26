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
                echo $("docker ps | grep 5000 | awk '{print$1}')
                '''
            }
        }
        stage('Health Check') {

            steps {

                sh '''
                sleep 15
                curl http://localhost:5000/health
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
          echo "container is running"
          docker rm -f python-container || true
          '''

        }

    }

}
