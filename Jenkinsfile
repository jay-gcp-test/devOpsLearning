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
                googleServiceAccount([file(credentialsId: 'd7d633bc-c422-4620-a0ac-9fca348caf4c', variable: 'GCP_CREDS')]) {
                    sh """
                        echo "$GCP_CREDS" > gcp-key.json
                        gcloud autho activate-service-account --key-file=gcp-key.json
                        gcloud config set project $PROJECT_ID
                        gcloud container clusters get-credentials $CLUSTER_NAME --zone $CLUSTER_ZONE

                        kubectl set image deployment/test-autO your-container-name=$IMAGE_NAME:$BUILD_NUMBER
                    """
                }
            }
        }
    }
}