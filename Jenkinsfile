pipeline {

    agent any


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


        stage('Health Check') {

            steps {

                sh '''
                sleep 5
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
            docker rm -f python-container || true
            '''

        }

    }

}
