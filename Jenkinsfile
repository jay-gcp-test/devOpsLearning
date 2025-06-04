pipeline {
    agent any

    environment {
        PROJECT_ID = 'diesel-acolyte-460018-b9'
        CLUSTER_NAME = 'test-cluster-1'
        CLUSTER_ZONE = 'us-central1'
        NAMESPACE = 'test-env'
        RELEASE_NAME = 'auto-nginx'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                withCredentials([file(credentialsId: 'Google Cloud Plain Text', variable: 'GCP_CREDS')]) {
                    sh """
                        gcloud auth activate-service-account --key-file=$GCP_CREDS
                        gcloud config set project $PROJECT_ID
                        gcloud container clusters get-credentials $CLUSTER_NAME --zone $CLUSTER_ZONE

                        helm upgrade --install $RELEASE_NAME ./auto-nginx -n $NAMESPACE
                    """
                }
            }
        }
    }
}