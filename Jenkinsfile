pipeline {
    agent any

    environment {
        PROJECT_ID = 'diesel-acolyte-460018-b9'
        CLUSTER_NAME = 'test-cluster-1'
        CLUSTER_ZONE = 'us-central1'
        NAMESPACE = 'test-env'
        REPOSITORY_NAME = 'test-repository'
        IMAGE_NAME = 'test-docker'
        RELEASE_NAME = 'auto-nginx'
        HELM_PATH = './auto-nginx'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker Image....'
                    sh 'docker build -t ${IMAGE_NAME}:${env.BUILD_ID}'
                }
            }
        }

        stage('Build and Push Docker Image to GCA') {
            steps {
                script {
                    echo 'Pushing Docker Image....'
                    withCredentials([file(credentialsId: 'Google Cloud Plain Text', variable: 'GCP_CREDS')]) {
                        sh '''
                            gcloud auth activate-service-account --key-file=$GCP_CREDS
                            gcloud config set project $PROJECT_ID
                            gcloud auth configure-docker ${CLUSTER_ZONE}-docker.pkg.dev --quiet

                            docker build -t ${IMAGE_NAME}:${env.BUILD_ID}
                            docker push ${IMAGE_NAME}:${env.BUILD_ID}

                        '''
                    }
                }
            }
        }

        stage('Deploy HELM') {
            steps {
                echo 'Deploying HELM Chart....'
                withCredentials([file(credentialsId: 'Google Cloud Plain Text', variable: 'GCP_CREDS')]) {
                    sh '''
                        gcloud auth activate-service-account --key-file=$GCP_CREDS
                        gcloud config set project $PROJECT_ID
                        gcloud container clusters get-credentials $CLUSTER_NAME --zone $CLUSTER_ZONE

                        helm upgrade --install $RELEASE_NAME $HELM_PATH -n $NAMESPACE
                    '''
                }
            }
        }
    }
}