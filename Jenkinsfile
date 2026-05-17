pipeline {
    agent any

    environment {

        AWS_REGION = 'ap-south-2'
        AWS_ACCOUNT_ID = '944765969321'

        USER_REPO = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/user-service"
        PRODUCT_REPO = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/product-service"
        ORDER_REPO = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/order-service"
        GATEWAY_REPO = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/gateway-service"

        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Clone Source Code') {
            steps {

                git branch: 'main',
                url: 'https://github.com/nsarumathi/Git_MicroService_Containerisation.git'

            }
        }

        stage('Docker Build') {
            steps {

                sh '''
                docker compose build
                '''
            }
        }

        stage('AWS ECR Login') {
            steps {

                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''

            }
        }

        stage('Tag Images') {
            steps {

                sh '''
                docker tag user-service:latest $USER_REPO:$IMAGE_TAG
                docker tag product-service:latest $PRODUCT_REPO:$IMAGE_TAG
                docker tag order-service:latest $ORDER_REPO:$IMAGE_TAG
                docker tag gateway-service:latest $GATEWAY_REPO:$IMAGE_TAG
                '''

            }
        }

        stage('Push Images to ECR') {
            steps {

                sh '''
                docker push $USER_REPO:$IMAGE_TAG
                docker push $PRODUCT_REPO:$IMAGE_TAG
                docker push $ORDER_REPO:$IMAGE_TAG
                docker push $GATEWAY_REPO:$IMAGE_TAG
                '''

            }
        }
    }

    post {

        success {
            echo '✅ All Docker Images Successfully Pushed To AWS ECR'
        }

        failure {
            echo '❌ Pipeline Failed'
        }
    }
}
