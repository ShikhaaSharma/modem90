pipeline {



    agent any



    environment {

        IMAGE_NAME = "shikha1818/my-k8s-demo"

        IMAGE_TAG = "v1"

    }



    stages {



        stage('Checkout Source Code') {

            steps {

                echo "Cloning GitHub Repository..."

                checkout scm

            }

        }



        stage('Build Docker Image') {

            steps {

                echo "Building Docker Image..."

                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'

            }

        }



        stage('Push Docker Image') {

            steps {

                echo "Pushing Image to DockerHub..."



                withCredentials([

                    usernamePassword(

                        credentialsId: 'dockerhub',

                        usernameVariable: 'DOCKER_USER',

                        passwordVariable: 'DOCKER_PASS'

                    )

                ]) {



                    sh '''

                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin

                    docker push $IMAGE_NAME:$IMAGE_TAG

                    docker logout

                    '''

                }

            }

        }



        stage('Deploy to Kubernetes') {

            steps {

                echo "Deploying Application..."



                sh '''

                kubectl apply -f k8s/deployment.yaml

                kubectl apply -f k8s/service.yaml

                '''

            }

        }



        stage('Verify Deployment') {

            steps {

                sh '''

                kubectl get deployment

                kubectl get pods

                kubectl get svc

                '''

            }

        }



    }



    post {



        success {

            echo "CI/CD Pipeline Executed Successfully"

        }



        failure {

            echo "Pipeline Failed"

        }



    }



}
